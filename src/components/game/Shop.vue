<template>
    <template v-if="task == 'shop'">
        <p>What Next?</p>
        <p>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn}" @click="handleBuy">Buy</button>
        </p>
        <p>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn}" @click="handleSell">Sell</button>
        </p>
        <p>
            <button class="btn-inv" :class="{ 'disabled' : !playerTurn}" @click="handleLeave"><i class="material-icons left">arrow_back</i>Leave</button>
        </p>
    </template>
    <template v-else-if="task == 'buy'">
        <p>{{$parent.infoText}}</p>
        <div class="items" @mouseleave="$parent.infoText = 'Select an item to buy'">
            <button class="btn-blue"
            v-for="(item, index) in $parent.merchant"
            :key="index"
                :class="{ 'disabled' : !playerTurn}"
                @mouseover="$parent.infoText = item.info"
                @click="handleBuyItem(item,index)">{{ item.name }} ({{item.buy}}g) x {{item.qty}}</button>
        </div>
        <p>
            <button class="btn-inv" :class="{ 'disabled' : !playerTurn}" @click="handleBack"><i class="material-icons left">arrow_back</i>Back</button>
        </p>
    </template>
    <template v-else-if="task == 'sell'">
        <p>{{$parent.infoText}}</p>
        <div class="items" @mouseleave="$parent.infoText = 'Select an item to sell'">
            <button class="btn-blue"
            v-for="(item, index) in $parent.player.inventory"
            :key="index"
                :class="{ 'disabled' : !playerTurn}"
                @mouseover="$parent.infoText = item.info"
                @click="handleSellItem(item,index)">{{ item.name }} ({{item.sell}}g) x {{item.qty}}</button>
        </div>
        <p>
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
            if (!player.inventory.length) {
                this.$parent.infoText = "Inventory empty"
            }
        },
        handleLeave() {
            this.$parent.message = "You left the shop."
            this.$parent.changeScene('Town');
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
