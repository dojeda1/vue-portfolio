<template>
    <template v-if="task == 'select'">
        <p>{{$parent.infoText}}</p>
        <p class="items full-buttons" @mouseleave="$parent.infoText = 'Select an item'">
            <button class="btn-blue"
            v-for="(item, index) in $parent.merchant"
            :key="index"
                :class="{ 'disabled' : !playerTurn}"
                @mouseover="$parent.infoText = item.info"
                @click="handleSelectItem(item,index)">{{ item.name }}
                </button>
        </p>
    </template>
    <template v-else-if="task == 'end'">
        <p class="dual-buttons">
            <button class="btn-inv" :class="{ 'disabled' : !playerTurn}" @click="handleNext">Next</button>
        </p>
    </template>
</template>

<script>
export default {
    name: 'AdventurerEncounter',
    data() {
        return {
            task: 'select',
            playerTurn: true
        }
    },
    methods: {
        log(msg) {
            console.log('Log:',msg);
        },
        handleSelectItem(item,index) {
            const player = this.$parent.player
            const adventurer = this.$parent.merchant
            this.$parent.transferItem(adventurer, player.inventory, adventurer[index])
            this.$parent.message = "You chose the " + item.name + "."
            this.$parent.messageBox.push("The Adventurer bids you farewell.")
            this.task = 'end'
        },
        handleNext() {
            this.$parent.message = "You left the Old Aventurer behind."
            this.$parent.messageBox = []
            if (this.$parent.movingForward == true) {
                this.playerTurn = false;
                this.$parent.moveForward();
            } else {
                this.$parent.changeScene('Wild');
            }
        }
    },
    created: function() {
        // this.$parent.location = 'Wild';
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>

</style>
