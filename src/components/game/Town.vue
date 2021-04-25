<template>
    <template v-if="task == 'town'">
        <p>Where to next?</p>
        <p>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn}" @click="handleInn">Stay at Inn</button>
        </p>
        <p>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn}" @click="handleShop">Visit Shop</button>
        </p>
        <p>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn}" @click="handleTavern">Visit Tavern</button>
        </p>
        <p>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn}" @click="handleQuestBoard">View Quest Board</button>
        </p>
        <p>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn}" @click="handleUseItem">Use Item</button>
        </p>
        <p>
            <button class="btn-inv" :class="{ 'disabled' : !playerTurn}" @click="handleLeaveTown"><i class="material-icons left">arrow_back</i>Leave Town</button>
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
                @click="handleAttemptItem(item,index)">{{ item.name }}
                <template v-if="item.charge > 0"> ({{item.goal - item.charge}}/{{item.goal}})</template>
                <template v-if="item.qty > 1"> &times; {{item.qty}}</template>
                </button>
        </div>
        <p>
            <button class="btn-inv" :class="{ 'disabled' : !playerTurn}" @click="handleBack"><i class="material-icons left">arrow_back</i>Back</button>
        </p>
    </template>
    <template v-else-if="task == 'inn'">
        <p>Pay for the room?</p>
        <p>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn}" @click="handleYesInn">Yes</button>
        </p>
        <p>
            <button class="btn-inv" :class="{ 'disabled' : !playerTurn}" @click="handleNoInn"><i class="material-icons left">arrow_back</i>No</button>
        </p>
    </template>
</template>

<script>
export default {
    name: 'Town',
    data() {
        return {
            task: 'town',
            playerTurn: true
        }
    },
    methods: {
        log(msg) {
            console.log('Log:',msg);
        },
        handleBack() {
            this.task = 'town';
        },
        handleLeaveTown() {
            this.$parent.message = "You left town."
            this.$parent.messageBox = [];
            var $this = this
            $this.playerTurn = false
            $this.$parent.player.animation = 'walk'
            setTimeout(function() {
                $this.$parent.player.animation = 'idle'
                $this.playerTurn = true
                $this.$parent.changeScene('Wild');
            },1200)
        },
        handleInn() {
            const player = this.$parent.player
            if (player.hp < player.hpMax || player.mp < player.mpMax) {
                const cost = 10 + (player.level - 2) * 2;
                this.$parent.message = 'It costs ' + cost + 'g to stay the night. Recovers max HP and MP.'
                this.task = 'inn';
            } else {
                this.$parent.message = 'You are already at full Health and Mana.'
            }
        },
        handleYesInn() {
            let player = this.$parent.player;
            const cost = 10 + (player.level - 2) * 2;
            if (player.gold >= cost) {
                player.gold = player.gold - cost;
                player.hp = player.hpMax;
                player.mp = player.mpMax;
                this.$parent.message = 'You feel well rested.'
                player.animation = 'buffer'
                this.playerTurn = false
                var $this = this
                setTimeout(function() {
                    player.animation = 'idle'
                    $this.playerTurn = true
                    $this.task = 'town'
                },600) 
            } else {
                this.$parent.message = "You can't afford to stay here."
                this.task = 'town'
            }
        },
        handleNoInn() {
            this.$parent.message = "You decided against it."
            this.task = 'town'
        },
        handleShop() {
            this.$parent.message = 'You entered the Shop.'
            this.$parent.currentEncounter = JSON.parse(JSON.stringify(this.$parent.encounters[4]));
            this.$parent.currentEncounter.animation = 'idle'
            this.$parent.changeScene('Shop');
            this.$parent.enterEvent();
        },
        handleTavern() {
            this.$parent.message = 'You entered the Tavern.'
            this.$parent.currentEncounter = JSON.parse(JSON.stringify(this.$parent.encounters[5]));
            this.$parent.currentEncounter.animation = 'idle'
            this.$parent.changeScene('Tavern');
            this.$parent.enterEvent();
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
        handleQuestBoard() {
            this.$parent.message = 'You viewed the Quest Board.'
            this.$parent.currentEncounter = JSON.parse(JSON.stringify(this.$parent.encounters[3]));
            this.$parent.changeScene('Quests');
            this.$parent.enterEvent();
            if (!this.$parent.questBoard.length) {
                this.$parent.infoText = "There are no more Quests at this time."
            } else {
                this.$parent.infoText = "Accept a quest"
            }
        }
    },
    created: function() {
        this.$parent.location = 'Town';
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>

</style>
