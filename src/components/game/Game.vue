<template>
<div id="game">
    <div class="container">
        <div class="game-header">
            <h1>
                <img class="game-sprite"
                src="/images/game/dragon.png" style="width: 32px">
                Fantasy RPG
                <img class="game-sprite"
                src="/images/game/dragon.png" style="width: 32px;transform: scaleX(-1);">
            </h1>
            <p>- {{ this.region.name}} | {{location}} -</p>
        </div>
        <TitleScreen v-if="scene == 'TitleScreen'"/>
        <CharacterCreation v-if="scene == 'CharacterCreation'"/>
        <Wild v-if="scene == 'Wild'"/>
        <Battle v-if="scene == 'Battle'"/>
    </div>
</div>
</template>

<script>
import TitleScreen from './TitleScreen.vue'
import CharacterCreation from './CharacterCreation.vue'
import Wild from './Wild.vue'
import Battle from './Battle.vue'
// import Town from './Town.vue'
// import Shop from './Shop.vue'
// import Tavern from './Tavern.vue'

//JSON Files
import playerDefault from './data/playerDefault.json';

import regions from "./data/regions.json";

import monsters1 from "./data/monsters1.json";
import monsters2 from "./data/monsters2.json";
import monsters3 from "./data/monsters3.json";

import bosses1 from "./data/bosses1.json";
import bosses2 from "./data/bosses2.json";
import bosses3 from "./data/bosses3.json";
import endBosses from "./data/endBosses.json";

import items1 from "./data/items1.json";
import items2 from "./data/items2.json";
import items3 from "./data/items3.json";

// import quests1 from "./data/quests1.json";
// import quests2 from "./data/quests2.json";
// import quests3 from "./data/quests3.json";

export default {
    name: 'Game',
    components: {
        TitleScreen,
        CharacterCreation,
        Wild,
        Battle,
        // Town,
        // Shop,
        // Tavern
    },
    data() {
        return {
            message: "",
            messageBox: [],
            infoText: "",
            region: regions[0],
            scene: "TitleScreen",
            location: "Title Screen",
            // task: "new or load",
            // step: null,
            // inputName: "",
            // movingForward: false,
            player: {},
            currentEnemy: {},
            // quests: [],
            // tavernQuests: [],
            // merchant: [],
            dungeonCount: 0,
            // castleCount: 0,
            // meadCount: 0,
            playerDefault: playerDefault,
            items1: items1,
            items2: items2,
            items3: items3,
            monsters1: monsters1,
            monsters2: monsters2,
            monsters3: monsters3,
            bosses1: bosses1,
            bosses2: bosses2,
            bosses3: bosses3,
            endBosses: endBosses,
            // showSave: false,
            // showStats: false,
            // showQuests: false,
            // showQuit: false
        }
    },
    methods: {
        log(msg) {
            console.log('Log:',msg);
        },
        resetPlayer() {
            this.player = JSON.parse(JSON.stringify(playerDefault));
        },
        changeScene(nextScene) {
            this.scene = nextScene;
        },
        randNum(x, y) {
            return Math.floor(Math.random() * y) + x;
        },
        anA(word) {
            var first = word.charAt(0);
            var anA = "";
            if (first == "A" || first == "E" || first == "I" || first == "O" || first == "U") {
                anA = "an";
                return anA;
            } else {
                anA = "a";
                return anA;
            }
        },
        hasSpecial(arr,name) {
            let hasSpecial = false;
            arr.forEach(function(special) {
                if (special.name == name) {
                    hasSpecial = true;
                }
            })
            return hasSpecial;
        },
        returnSpecial(arr,name) {
            var $special;
            arr.forEach(function(special) {
                console.log('check...')
                if (special.name == name) {
                    $special = special
                }
            })
            return $special
        },
        addItem(array, item) {
            const newItem = JSON.parse(JSON.stringify(item));
            let inInventory = false;
            array.forEach(element => {
                if (newItem.name === element.name) {
                    inInventory = true;
                    element.qty++
                }
            });
            if (inInventory === false) {
                array.push(newItem);
            }
            function compare(a, b) {
                const orderA = a.order;
                const orderB = b.order;

                let comparison = 0;
                if (orderA > orderB) {
                    comparison = 1;
                } else if (orderA < orderB) {
                    comparison = -1;
                }
                return comparison;
            }

            array.sort(compare);
            // this.setState({
            //     player: this.state.player,
            //     currentEnemy: this.state.currentEnemy,
            //     merchant: this.state.merchant
            // }, this.fetchQuestCheck(item.name));
            // this.fetchQuestCheck(item.name));
        },
        removeItem(array, itemName) {
            array.forEach((element, i) => {
                if (itemName === element.name) {
                    element.qty--
                }
                if (element.qty <= 0) {
                    console.log("item zeroed out.")
                    array.splice(i, 1);
                }
            });
            // this.setState({
            //     player: this.state.player,
            //     currentEnemy: this.state.currentEnemy,
            //     merchant: this.state.merchant
            // }, this.fetchQuestCheck(itemName));
            // this.fetchQuestCheck(itemName));
        },
        transferItem(fromArr, toArr, item) {
            var transferItem = JSON.parse(JSON.stringify(item));
            this.addItem(toArr, transferItem);
            this.removeItem(fromArr, transferItem.name);
        },
        handleAttemptItem(item,index,cb) {
            console.log('chosen item: ' + item.name + ' ' + index)
            if (item.name == 'Old Hat') {
                this.message = 'It looks good on you...'
            } else if (item.name == 'Death Scroll' && this.scene != 'Battle') {
                this.message = 'Death Scroll can only be used in battle.'
            } else if (item.name.includes('Health Potion') && this.player.hp >= this.player.hpMax) {
                this.message = 'You are already at full Health.'
            } else if (item.name.includes('Mana Potion') && this.player.mp >= this.player.mpMax) {
                this.message = 'You are already at full Mana.'
            } else {
                this.messageBox = [];
                console.log('PLAYER',this.player)
                this.activateItem(this.player, this.currentEnemy, item, index);
                if (cb) {
                    cb();
                }
            }
        },
        activateItem(user, opponent, item, index) {
            user.animation = 'walk';
            console.log('opponent',opponent)
            console.log("item: " + item.name + " " + index)
            if (item.name.includes('Health Potion')) {
                let amount = Math.floor(user.hpMax * item.recover);
                user.hp += amount;
                if (user.hp > user.hpMax) {
                    user.hp = user.hpMax
                }
                this.removeItem(user.inventory, user.inventory[index].name);
                this.messageBox.push(user.name + ' recovered ' + amount + ' HP.')
            } else if (item.name.includes('Mana Potion')) {
                let amount = Math.floor(user.mpMax * item.recover);
                user.mp += amount;
                if (user.mp > user.mpMax) {
                    user.mp = user.mpMax
                }
                this.removeItem(user.inventory, user.inventory[index].name);
                this.messageBox.push(user.name + ' recovered ' + amount + ' MP.')
            } else if (item.name == 'Death Scroll') {
                if (opponent.type === 'boss' || opponent.type === 'endBoss') {
                    let power = Math.ceil(opponent.hp / 2);
                    opponent.hp -= power;
                    this.removeItem(user.inventory, user.inventory[index].name);
                    this.messageBox.push('Death Scroll only did ' + power + ' damage')
                } else {
                    opponent.hp = 0;
                    this.removeItem(user.inventory, user.inventory[index].name);
                    this.messageBox.push(user.name + ' read from the Death Scroll.')
                }
            } else {
                this.message = "SOMETHING WENT WRONG"
            }
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
    #game {
        color: white;
        width: calc(100% - 100px);
        height: 100%;
        background-color: #212121;
        background-image: url("/images/web-portfolio/game/map.jpg");
        background-size: cover;
        background-position: center; 
        position: fixed;
        top: 0;
        z-index: 1;
        overflow: auto;

        animation-timing-function: ease;
        animation: slide-in-left 3s;
        animation-iteration-count: 1;
    }

    .message-box {
        border: 2px solid gray;
        padding: 10px;
        /* min-height: 162px; */
    }

    @keyframes slide-in-left {
        0% { transform: translateX(-100%); }
        100% { transform: translateX(0); }
    }
    /* @keyframes slide-in-top {
        0% { transform: translateY(-100%); }
        100% { transform: translateY(0); }
    } */

    @keyframes fade-in {
        0% { opacity: 0; }
        50% { opacity: 0; }
        100% { opacity: 1; }
    }

    #game .container {
        animation-timing-function: ease;
        animation: fade-in 3s;
        animation-iteration-count: 1;
    }
    .game-header {
        text-align: center;
    }

    @media screen and (max-width: 600px) {
        #game {
            width: 100%;
            height: calc(100% - 60px);
            top: 60px;

            /* animation: slide-in-top 3s; */
        }
    }
</style>
