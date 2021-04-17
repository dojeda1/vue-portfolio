<template>
    <p>
        DC: {{$parent.dungeonCount}}/{{this.$parent.region.dungeonGoal}}
    </p>
    <template v-if="task == 'castle'">
        <p>Where to next?</p>
        <p v-if="$parent.dungeonCount >= this.$parent.region.dungeonGoal">
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn}" @click="handleFightBoss">Fight Boss</button>
        </p>
        <p v-else>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn}" @click="handleExplore">Venture Deeper</button>
        </p>
        <p>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn}" @click="handleUseItem">Use Item</button>
        </p>
        <p>
            <button class="btn-inv" :class="{ 'disabled' : !playerTurn}" @click="handleExit"><i className="material-icons left">arrow_back</i>Exit Castle</button>
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
                @click="handleAttemptItem(item,index)">{{ item.name }} x {{item.qty}}</button>
        </div>
        <p>
            <button class="btn-inv" :class="{ 'disabled' : !playerTurn}" @click="handleBack"><i class="material-icons left">arrow_back</i>Back</button>
        </p>
    </template>
</template>

<script>
export default {
    name: 'Castle',
    data() {
        return {
            task: 'castle',
            playerTurn: true
        }
    },
    methods: {
        log(msg) {
            console.log('Log:',msg);
        },
        handleExplore() {
            const exploreCheck = this.$parent.randNum(1, 4)
            if (exploreCheck == 1) {
                this.$parent.chestEncounter();
            } else {
                this.$parent.viciousEncounter();
            }
        },
        handleBack() {
            this.task = 'castle';
        },
        handleExit() {
            this.$parent.message = 'You left the castle...';
            this.$parent.changeScene('Wild');
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
        handleFightBoss() {
            if (!this.$parent.region.endBossKills == 0) {
                this.$parent.endBossEncounter();
            } else {
                this.$parent.viciousEncounter();
            }
        }
    },
    
    created: function() {
        this.$parent.location = 'Castle';
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>

</style>
