<template>
    <p>
        Minion Kills: {{$parent.dungeonCount}}/{{this.$parent.regions[this.$parent.region].dungeonGoal}}
    </p>
    <template v-if="task == 'castle'">
        <p>Where to next?</p>
        <p class="full-buttons">
            <button v-if="$parent.dungeonCount >= this.$parent.regions[this.$parent.region].dungeonGoal"
            class="btn-blue" :class="{ 'disabled' : !playerTurn}" @click="handleFightBoss">Fight Boss</button>
            <button v-else
            class="btn-blue" :class="{ 'disabled' : !playerTurn}" @click="handleExplore">Venture Deeper</button>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn}" @click="handleUseItem">Use Item</button>
        </p>
        <p class='dual-buttons'>
            <button class="btn-inv" :class="{ 'disabled' : !playerTurn}" @click="handleExit"><i className="material-icons left">arrow_back</i>Exit Castle</button>
        </p>
    </template>
    <template v-else-if="task == 'use item'">
        <p>{{$parent.infoText}}</p>
        <p class="items full-buttons" @mouseleave="$parent.infoText = 'Select an Item'">
            <button class="btn-blue"
            v-for="(item, index) in $parent.player.inventory"
            :key="index"
                :class="{ 'disabled' : !playerTurn}"
                @mouseover="$parent.infoText = item.info"
                @click="handleAttemptItem(item,index)">{{ item.name }}
                <template v-if="item.charge > 0"> ({{item.goal - item.charge}}/{{item.goal}})</template>
                <template v-if="item.qty > 1"> &times; {{item.qty}}</template>
                </button>
        </p>
        <p class="dual-buttons">
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
            this.$parent.player.animation = 'walk';
            this.playerTurn = false;
            const exploreCheck = this.$parent.randNum(1, 4)
            const $this = this;
            setTimeout(function() {
                $this.$parent.player.animation = 'idle';
                $this.playerTurn = true;
                if (exploreCheck == 1) {
                    $this.$parent.chestEncounter();
                } else {
                    $this.$parent.viciousEncounter();
                }
            },600)
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
            if (this.$parent.regions[this.$parent.region].endBossKills == 0) {
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
