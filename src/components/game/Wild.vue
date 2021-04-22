<template>
    <template v-if="task == 'wild'">
        <p>Where to next?</p>
        <p>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn}" @click="handleExplore">Explore</button>
        </p>
        <p>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn}" @click="handleTown">Go to Town</button>
        </p>
        <p>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn}" @click="handleUseItem">Use Item</button>
        </p>
        <p>
            <button class="btn-inv"
            @click="handleMoveBack"
            v-if="$parent.regions[$parent.region - 1]"
            :class="{ 'disabled' : !playerTurn}">
            <i className="material-icons left">arrow_back</i>{{$parent.regions[$parent.region - 1].name}}
            </button>
            <button class="btn-inv"
            @click="handleMoveForward"
            v-if="$parent.regions[$parent.region + 1]"
            :class="{ 'disabled' : !playerTurn || $parent.regions[$parent.region].kills < 3}">
            {{$parent.regions[$parent.region + 1].name}}
            <template v-if="$parent.regions[$parent.region].kills < 3"> ({{$parent.regions[$parent.region].kills}}/3)</template>
            <template v-else>
                <i className="material-icons">arrow_forward</i>
            </template>
            </button>
            <button class="btn-inv"
            @click="handleCastle"
            v-if="$parent.region + 1 == $parent.regions.length"
            :class="{ 'disabled' : !playerTurn || $parent.regions[$parent.region].kills < 3}">
            Dark Castle
            <template v-if="$parent.regions[$parent.region].kills < 3"> ({{$parent.regions[$parent.region].kills}}/3)</template>
            <template v-else>
                <i className="material-icons">arrow_forward</i>
            </template>
            </button>
        </p>
    </template>
    <template v-else-if="task == 'use item'">
        <p>{{$parent.infoText}}</p>
        <div class="items" @mouseleave="$parent.infoText = 'Select an Item'">
            <button class="btn-blue"
            v-for="(item, index) in $parent.player.inventory"
            :key="index"
                :class="{ 'disabled' : !playerTurn}"
                @mouseover="$parent.infoText = item.info"
                @click="handleAttemptItem(item,index)">{{ item.name }} &times; {{item.qty}}</button>
        </div>
        <p>
            <button class="btn-inv" :class="{ 'disabled' : !playerTurn}" @click="handleBack"><i class="material-icons left">arrow_back</i>Back</button>
        </p>
    </template>
    <template v-else-if="task == 'next level'">
        <p>Continue forward?</p>
        <p>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn}" @click="handleYesForward">Yes</button>
        </p>
        <p>
            <button class="btn-inv" :class="{ 'disabled' : !playerTurn}" @click="handleNoForward"><i class="material-icons left">arrow_back</i>No</button>
        </p>
    </template>
    <template v-else-if="task == 'castle'">
        <p>Dare to enter?</p>
        <p>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn}" @click="handleYesCastle">Enter</button>
        </p>
        <p>
            <button class="btn-inv" :class="{ 'disabled' : !playerTurn}" @click="handleNoCaslte"><i class="material-icons left">arrow_back</i>Leave</button>
        </p>
    </template>
</template>

<script>
export default {
    name: 'Wild',
    data() {
        return {
            task: 'wild',
            playerTurn: true
        }
    },
    methods: {
        log(msg) {
            console.log('Log:',msg);
        },
        handleExplore() {
            const exploreCheck = this.$parent.randNum(1, 20)
            if (exploreCheck <= 2) {
                this.$parent.chestEncounter();
            } else if (exploreCheck <= 4) {
                this.$parent.dungeonEncounter();
            } else if (exploreCheck <= 6) {
                this.$parent.viciousEncounter();
            } else if (exploreCheck == 7) {
                this.$parent.merchantEncounter();
            } else {
                this.$parent.monsterEncounter();
            }
        },
        handleTown() {
            const ambushCheck = this.$parent.randNum(1, 10)
            if (ambushCheck == 1) {
                this.$parent.monsterEncounter(true)
            } else {
                this.$parent.message = "You traveled to town."
                var $this = this
                $this.playerTurn = false
                $this.$parent.player.animation = 'walk'
                this.$parent.resetMerchant();
                this.$parent.resetQuestBoard();
                this.$parent.meadCount = 0;
                setTimeout(function() {
                    $this.$parent.player.animation = 'idle'
                    $this.playerTurn = true
                    $this.$parent.changeScene('Town');
                },1200)
            }
        },
        handleBack() {
            this.task = 'wild';
        },
        handleUseItem() {
            if (this.$parent.player.inventory.length) {
                this.$parent.infoText = 'Select an item'
            } else {
                this.$parent.infoText = 'Inventory empty'
            }
            this.task = 'use item';
        },
        handleAttemptItem(item,index) {
            let $this = this;
            let player = this.$parent.player;
            this.$parent.handleAttemptItem(item,index,
                function() {
                    $this.playerTurn = false;
                    setTimeout(function() {
                        $this.playerTurn = true;
                        player.animation = 'idle'
                    }, 600)
                }
            );
        },
        handleMoveBack() {
            this.$parent.player.animation = 'walk'
            this.playerTurn = false
            const $this = this;
            setTimeout(function() {
                $this.$parent.region--
                $this.$parent.message = 'You went back to the ' + $this.$parent.regions[$this.$parent.region].name
                $this.$parent.player.animation = 'idle'
                $this.playerTurn = true
            },1200)
        },
        handleMoveForward() {
            this.task = 'next level'
            this.$parent.message = 'The road ahead is dangerous.'
        },
        handleYesForward() {
            this.$parent.movingForward = true;
            console.log('movingUP:',this.$parent.movingForward)
            if (this.$parent.regions[this.$parent.region].endBossKills == 0) {
                this.$parent.endBossEncounter();
            } else {
                this.$parent.viciousEncounter();
            }
        },
        handleNoForward() {
            this.task = 'wild'
            this.$parent.message = 'You decided against it.'
        },
        handleCastle() {
            this.task = 'castle'
            this.$parent.message = 'The Castle emits an evil aura...'
        },
        handleYesCastle() {
            this.$parent.dungeonCount = 0;
            this.$parent.message = "You step into the castle."
            this.$parent.changeScene('Castle');
        },
        handleNoCastle() {
            this.$parent.message = "You left the castle behind."
            this.$parent.changeScene('Wild');
        }
    },
    created: function() {
        this.$parent.location = 'Wild';
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>

</style>
