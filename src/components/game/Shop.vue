<template>
    <template v-if="task == 'shop'">
        <p>What Next?</p>
        <p class='full-buttons'>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn}" @click="handleBuy">Buy</button>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn}" @click="handleSell">Sell</button>
        </p>
        <p class="dual-buttons">
            <button class="btn-inv" :class="{ 'disabled' : !playerTurn}" @click="handleLeave"><i class="material-icons left">arrow_back</i>Leave</button>
        </p>
    </template>
    <template v-else-if="task == 'buy'">
        <p>{{$parent.infoText}}</p>
        <p class="items full-buttons" @mouseleave="$parent.infoText = 'Select an item to buy'">
            <button class="btn-blue"
            v-for="(item, index) in $parent.merchant"
            :key="index"
                :class="{ 'disabled' : !playerTurn}"
                @mouseover="$parent.infoText = item.info"
                @click="handleBuyItem(item,index)">{{ item.name }} ({{item.buy}}g)
                <template v-if="item.qty > 1"> &times; {{item.qty}}</template>
                </button>
        </p>
        <p class="dual-buttons">
            <button class="btn-inv" :class="{ 'disabled' : !playerTurn}" @click="handleBack"><i class="material-icons left">arrow_back</i>Back</button>
        </p>
    </template>
    <template v-else-if="task == 'sell'">
        <p>{{$parent.infoText}}</p>
        <p class="items full-buttons" @mouseleave="$parent.infoText = 'Select an item to sell'">
            <button class="btn-blue"
            v-for="(item, index) in $parent.player.inventory"
            :key="index"
                :class="{ 'disabled' : !playerTurn || item.name == 'Head of Asteroth'}"
                @mouseover="$parent.infoText = item.info"
                @click="handleSellItem(item,index)">{{ item.name }} ({{item.sell}}g)
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
    name: 'Shop',
    data() {
        return {
            task: 'shop',
            playerTurn: true
        }
    },
    methods: {
        log(msg) {
            console.log('Log:',msg);
        },
        handleBuy() {
            if (this.$parent.merchant.length) {
                this.$parent.infoText = 'Select an item to buy'
            } else {
                this.$parent.infoText = 'Merchant has no more items.'
            }
            this.task = 'buy';
        },
        handleSell() {
            if (this.$parent.player.inventory.length) {
                this.$parent.infoText = 'Select an item to sell'
            } else {
                this.$parent.infoText = 'Inventory empty'
            }
            this.task = 'sell';
        },
        handleBuyItem(item,index) {
            const player = this.$parent.player
            const shop = this.$parent.merchant
            const price = item.buy;
            if (price <= player.gold) {
                player.gold -= price;
                this.$parent.transferItem(shop, player.inventory, shop[index])
                this.$parent.message = "You bought " + this.$parent.anA(item.name) + " " + item.name + " for " + price + "g."
                this.$parent.note(player,-price)
                this.$parent.note(this.$parent.currentEncounter,'+' + price);
                this.$parent.itemNote(player,item);
                if (!shop.length) {
                    this.$parent.infoText = "Merchant has no more items."
                }
            } else {
                this.$parent.message = "You don't have enough gold..."
            }
        },
        handleSellItem(item,index) {
            const player = this.$parent.player
            const shop = this.$parent.merchant
            const price = item.sell;
            player.gold += price;
            this.$parent.transferItem(player.inventory, shop, player.inventory[index])
            this.$parent.message = "You sold " + this.$parent.anA(item.name) + " " + item.name + " for " + price + "g."
            this.$parent.note(player,'+' + price)
            this.$parent.note(this.$parent.currentEncounter,-price);
            this.$parent.itemNote(this.$parent.currentEncounter,item);
            if (!player.inventory.length) {
                this.$parent.infoText = "Inventory empty"
            }
        },
        handleLeave() {
            if (this.$parent.location == 'Town') {
                this.$parent.message = "You left the shop."
                this.$parent.messageBox = [];
                this.$parent.changeScene('Town');
            } else if (this.$parent.location == 'Wild') {
                this.$parent.message = "You left the merchant behind."
                this.$parent.messageBox = [];
                this.$parent.changeScene('Wild');
            } else if (this.$parent.location == 'Dungeon') {
                this.$parent.message = "You left the merchant behind."
                this.$parent.messageBox = [];
                this.$parent.changeScene('Dungeon');
            } else if (this.$parent.location == 'Castle') {
                this.$parent.message = "You left the merchant behind."
                this.$parent.messageBox = [];
                this.$parent.changeScene('Castle');
            }
        },
        handleBack() {
            this.task = 'shop';
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
