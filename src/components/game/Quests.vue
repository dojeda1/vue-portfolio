<template>
    <template v-if="task == 'questBoard' && !$parent.$parent.menu">
        <p>{{$parent.infoText}}</p>
        <div class="quest-board">
            <div class="quest"
            v-for="(quest, index) in $parent.questBoard"
            :key="index"
            :class="{ 'disabled' : !playerTurn}">
                <div>
                    <h5>{{quest.name}}</h5>
                    <p>{{quest.info}}</p>
                    <p>Region: {{quest.region}}</p>
                    <p>Reward: {{quest.amount}} {{quest.reward}}</p>
                </div>
                <p>
                    <button class="btn-blue" :class="{ 'disabled' : !playerTurn}" @click="handleAccept(index)">Accept<i class="material-icons right">check</i></button>
                </p>
                <!-- <p>
                    <button class="btn-blue" :class="{ 'disabled' : !playerTurn}" @click="handleLeave">Abandon<i class="material-icons right">delete</i></button>
                    <button class="btn-blue" :class="{ 'disabled' : !playerTurn}" @click="handleLeave">Redeem<i class="material-icons right">check</i></button>
                </p> -->
            </div>
        </div>
        <p>
            <button class="btn-inv" :class="{ 'disabled' : !playerTurn}" @click="handleLeave"><i class="material-icons left">arrow_back</i>Leave</button>
        </p>
    </template>
    <template v-else-if="$parent.$parent.menu = 'Quests'">
        <div class="message-box">
            <h5>{{ $parent.message }}</h5>
            <p v-for="(msg, index) in $parent.messageBox" :key="index">
                {{ msg }}
            </p>
        </div>
        <p>Your Quests - Redeem in Town</p>
        <div class="quest-board">
            <div class="quest"
            v-for="(quest, index) in $parent.player.quests"
            :key="index"
            :class="{ 'disabled' : !playerTurn}">
                <div>
                    <h5>{{quest.name}}</h5>
                    <p>{{quest.info}}</p>
                    <p :class="{'text-red': quest.region != $parent.region.name}">Region: {{quest.region}}</p>
                    <p>Reward: {{quest.amount}} {{quest.reward}}</p>
                    <p :class="{'text-blue': quest.count >= quest.goal}">Progress: {{quest.count}}/{{quest.goal}}</p>
                </div>
                <!-- <p>
                    <button class="btn-blue" :class="{ 'disabled' : !playerTurn}" @click="handleAccept(index)">Accept<i class="material-icons right">check</i></button>
                </p> -->
                <p>
                    <button class="btn-inv" :class="{ 'disabled' : !playerTurn}" @click="handleAbandon(index)">Abandon<i class="material-icons right">delete</i></button>
                    <button  v-if="$parent.location == 'Town'" class="btn-blue"
                    :class="{ 'disabled' : !playerTurn || quest.count < quest.goal || quest.region != $parent.region.name}"
                    @click="handleRedeem(quest,index)">Redeem<i class="material-icons right">check</i></button>
                </p>
            </div>
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
            task: 'questBoard',
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
        handleLeave() {
            this.$parent.message = "You left the Quest Board."
            this.$parent.changeScene('Town');
        },
        handleBack() {
            this.$parent.$parent.menu = false;
        },
        handleAccept(index) {
            if (this.$parent.player.quests.length < 3) {
                let quests = this.$parent.player.quests
                let questBoard = this.$parent.questBoard
                this.$parent.addQuest(questBoard, quests, index);
                this.$parent.removeQuest(questBoard, index);
                // this.setState({
                //     quests: quests,
                //     tavernQuests: tavernQuests,
                //     message: "You got a new quest!",
                //     step: "select quest"
                // })
                this.$parent.message = "You've got a new quest!"
                if (!questBoard.length) {
                    this.$parent.infoText = "There are no more Quests at this time."
                }
            } else {
                this.$parent.message = "You can only have 3 quests at a time."
            }
            console.log('PlayerQuests',this.$parent.player.quests)
        },
        handleRedeem(quest,index) {
            console.log('Quest:',quest)
            const player = this.$parent.player
            if (quest.reward == "gold") {
                player.gold += quest.amount
                player.totalGold += quest.amount
                player.totalQuests++
            } else {
                let itemArray = [];
                switch (quest.rarity) {
                    case 1:
                        itemArray = this.$parent.items1;
                        break;
                    case 2:
                        itemArray = this.$parent.items2;
                        break;
                    case 3:
                        itemArray = this.$parent.items3;
                        break;
                    default:
                    // code block
                }
                console.log("Item Array")
                console.log(itemArray)
                for (let i = 0; i < quest.amount; i++) {
                    this.$parent.addItem(player.inventory, itemArray[quest.itemIndex]);
                }
                console.log("item reward.")
            }
            if (quest.type === "fetch") {
                for (let i = 0; i < quest.goal; i++) {
                    this.$parent.removeItem(player.inventory, quest.task)
                }
            }
            this.$parent.message = "You earned " + quest.amount + " " + quest.reward + "."
            this.$parent.removeQuest(player.quests, index);
        },
        handleAbandon(index) {
            this.$parent.removeQuest(this.$parent.player.quests, index);
        }
    },
    created: function() {
        // this.$parent.location = 'Wild';
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
    .quest-board {
        display: grid;
        grid-gap: 10px;
        margin-top: 10px;
        grid-template-columns: 1fr 1fr 1fr;
    }
    .quest {
        display: grid;
        grid-template-rows: 1fr min-content;
        border: 2px solid gray;
        grid-column: span 1;
        border-radius: 10px;
        padding: 10px;
        /* min-height: 162px; */
    }
    @media screen and (max-width: 600px) {
        .quest {
            grid-column: span 3;
        }
    }
</style>
