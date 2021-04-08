<template>
    <template v-if="task == 'questBoard'">
        <p>{{$parent.infoText}}</p>
        <div class="quest-board">
            <div class="quest"
            v-for="(quest, index) in $parent.questBoard"
            :key="index"
            :class="{ 'disabled' : !playerTurn}">
                <h5>{{quest.name}}</h5>
                <p>{{quest.info}}</p>
                <p>Region: {{quest.region}}</p>
                <p>Reward: {{quest.amount}} {{quest.reward}}</p>
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
        // selectCashQuest() {
        //     this.setState({
        //         message: this.state.region.name + " Quest Rewards.",
        //         task: "tavern",
        //         step: "cash quest"
        //     })
        // }
        // selectRedeemReward(event) {
        //     let player = this.state.player
        //     let index = event.target.getAttribute("data-index");
        //     let type = event.target.getAttribute("data-type");
        //     let task = event.target.getAttribute("data-task");
        //     let goal = event.target.getAttribute("data-goal");
        //     let amount = event.target.getAttribute("data-amount");
        //     let reward = event.target.getAttribute("data-reward");
        //     let rarity = event.target.getAttribute("data-rarity");
        //     let itemIndex = event.target.getAttribute("data-item");
        //     if (reward === "gold") {
        //         player.gold += parseInt(amount)
        //         player.totalGold += parseInt(amount)
        //         player.totalQuests++
        //     } else {
        //         let itemArray = [];
        //         switch (rarity) {
        //             case "1":
        //                 itemArray = items1;
        //                 break;
        //             case "2":
        //                 itemArray = items2;
        //                 break;
        //             case "3":
        //                 itemArray = items3;
        //                 break;

        //             default:
        //             // code block
        //         }
        //         console.log("Item Array")
        //         console.log(itemArray)
        //         for (let i = 0; i < amount; i++) {
        //             this.addItem(player.inventory, itemArray[parseInt(itemIndex)]);
        //         }
        //         console.log("item reward.")
        //     }
        //     if (type === "fetch") {
        //         for (let i = 0; i < goal; i++) {
        //             this.removeItem(player.inventory, task)
        //         }
        //     }
        //     this.setState({
        //         message: "You earned " + amount + " " + reward + ".",
        //         player: player
        //     })
        //     this.removeQuest(this.state.quests, index);
        // },
        // selectAbandonQuest(event) {
        //     const index = event.target.getAttribute("data-index");
        //     let questArr = this.state.quests
        //     this.removeQuest(questArr, index);
        //     this.setState({
        //         quests: questArr
        //     });
        // }
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
