<template>
<div id="game">
    <div class="container">
        <p class="text-gray">SCENE: {{scene}}, DungComp: {{player.totalDungeons}}, P1 LV: {{player.level}}</p>
        <div class="game-header">
            <h1>Fantasy RPG</h1>
            <p>
                <img class="location-sprite"
                :src="region.sprite">
                {{ this.region.name}} | {{location}}
                <img class="location-sprite"
                :src="region.sprite">
            </p>
        </div>
        <template v-if="$parent.menu == false">
            <TitleScreen v-if="scene == 'TitleScreen'"/>
            <CharacterCreation v-if="scene == 'CharacterCreation'"/>
            <EventDisplay v-if="scene != 'TitleScreen'
            && scene != 'CharacterCreation'"/>
            <Wild v-if="scene == 'Wild'"/>
            <Town v-if="scene == 'Town'"/>
            <Shop v-if="scene == 'Shop'"/>
            <Tavern v-if="scene == 'Tavern'"/>
            <Quests v-if="scene == 'Quests'"/>
            <Dungeon v-if="scene == 'Dungeon'"/>
            <Battle v-if="scene == 'Battle'"/>
            <ChestEncounter v-if="scene == 'ChestEncounter'"/>
            <DungeonEncounter v-if="scene == 'DungeonEncounter'"/>
        </template>
        <template v-else>
            <Quests v-if="$parent.menu == 'Quests'"/>
        </template>
    </div>
</div>
</template>

<script>
import TitleScreen from './TitleScreen.vue'
import CharacterCreation from './CharacterCreation.vue'
import EventDisplay from './EventDisplay.vue'
import Wild from './Wild.vue'
import Town from './Town.vue'
import Shop from './Shop.vue'
import Tavern from './Tavern.vue'
import Quests from './Quests.vue'
import Dungeon from './Dungeon.vue'
import Battle from './Battle.vue'
import ChestEncounter from './ChestEncounter.vue'
import DungeonEncounter from './DungeonEncounter.vue'

//JSON Files
import playerDefault from './data/playerDefault.json';

import regions from "./data/regions.json";

import encounters from "./data/encounters.json";

// import monsters1 from "./data/monstersAll.json";
import monsters1 from "./data/monsters1.json";
import monsters2 from "./data/monsters2.json";
import monsters3 from "./data/monsters3.json";

// import bosses1 from "./data/bossesAll.json";
import bosses1 from "./data/bosses1.json";
import bosses2 from "./data/bosses2.json";
import bosses3 from "./data/bosses3.json";
import endBosses from "./data/endBosses.json";

import items1 from "./data/items1.json";
import items2 from "./data/items2.json";
import items3 from "./data/items3.json";

import quests1 from "./data/quests1.json";
import quests2 from "./data/quests2.json";
import quests3 from "./data/quests3.json";

export default {
    name: 'Game',
    components: {
        TitleScreen,
        CharacterCreation,
        EventDisplay,
        Wild,
        Town,
        Shop,
        Tavern,
        Quests,
        Dungeon,
        Battle,
        ChestEncounter,
        DungeonEncounter
    },
    data() {
        return {
            message: "",
            messageBox: [],
            infoText: "",
            region: regions[0],
            regions: regions,
            scene: "TitleScreen",
            location: "Title Screen",
            // task: "new or load",
            // step: null,
            // inputName: "",
            movingForward: false,
            player: {},
            currentEnemy: {},
            currentEncounter: {},
            questBoard: [],
            // tavernQuests: [],
            merchant: [],
            dungeonCount: 0,
            // castleCount: 0,
            meadCount: 0,
            // playerDefault: playerDefault,
            encounters: encounters,
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
        resetBosses() {
            this.player = JSON.parse(JSON.stringify(playerDefault));
            this.bosses1 = JSON.parse(JSON.stringify(bosses1));
            this.bosses2 = JSON.parse(JSON.stringify(bosses2));
            this.bosses3 = JSON.parse(JSON.stringify(bosses3));
            this.endBosses = JSON.parse(JSON.stringify(endBosses));
        },
        resetMerchant() {
            let randItem;
            this.merchant = [];
            const merchant = this.merchant;
            this.addItem(merchant,items1[0]);
            for (let i = 0; i < 4; i++) {
                randItem = this.randNum(0, items1.length);
                this.addItem(merchant, items1[randItem]);
            }
            for (let i = 0; i < 2; i++) {
                randItem = this.randNum(0, items2.length);
                this.addItem(merchant, items2[randItem]);
            }
            for (let i = 0; i < 1; i++) {
                randItem = this.randNum(0, items3.length);
                this.addItem(merchant, items3[randItem]);
            }
            console.log('Merchant',this.merchant)
        },
        resetQuestBoard() {
            let randQuest;
            let questArr;
            this.questBoard = [];
            switch (this.region.index) {
                case 1:
                    questArr = quests1
                    break;
                case 2:
                    questArr = quests2
                    break;
                case 3:
                    questArr = quests3
                    break;
                default:
                // code block
            }
            for (let i = 0; i < 3; i++) {
                randQuest = this.randNum(0, questArr.length);
                this.addQuest(questArr, this.questBoard, randQuest)
            }
            console.log('QuestBoard:',this.questBoard)
        },
        resetRegion() {
            this.region = regions[0];
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
        hasItem(inventory, itemName){
            let inInventory = false;
            inventory.forEach(element => {
                if (itemName === element.name) {
                    inInventory = true;
                }
            })
            return inInventory
        },
        addItem(array, item) {
            const newItem = JSON.parse(JSON.stringify(item));
            newItem.qty = 1;
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
            this.fetchQuestCheck(item.name);
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
            this.fetchQuestCheck(itemName);
        },
        transferItem(fromArr, toArr, item) {
            var transferItem = JSON.parse(JSON.stringify(item));
            this.addItem(toArr, transferItem);
            this.removeItem(fromArr, transferItem.name);
            console.log('fromArr:',fromArr)
            console.log('toArr:',toArr)
        },
        handleAttemptItem(item,index,cb) {
            console.log('chosen item: ' + item.name + ' ' + index)
            if (item.name == 'Old Hat') {
                this.message = 'It looks good on you...'
                const $this = this;
                $this.player.itemSprite = item.sprite;
                $this.player.itemName = item.name;
                $this.player.animation = 'use item';
                setTimeout(function() {
                    $this.player.animation = 'idle';
                }, 600)
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
        note(character,text) {
            character.note = text;
            setTimeout(function() {
                character.note = ''
            },600);
        },
        activateItem(user, opponent, item, index) {
            user.itemSprite = item.sprite;
            user.itemName = item.name;
            user.animation = 'use item';
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
                this.note(user,'+' + amount)
            } else if (item.name.includes('Mana Potion')) {
                let amount = Math.floor(user.mpMax * item.recover);
                user.mp += amount;
                if (user.mp > user.mpMax) {
                    user.mp = user.mpMax
                }
                this.removeItem(user.inventory, user.inventory[index].name);
                this.messageBox.push(user.name + ' recovered ' + amount + ' MP.')
                this.note(user,'+' + amount)
            } else if (item.name == 'Death Scroll') {
                if (opponent.type === 'boss' || opponent.type === 'endBoss') {
                    let power = Math.ceil(opponent.hp / 2);
                    opponent.hp -= power;
                    this.removeItem(user.inventory, user.inventory[index].name);
                    this.messageBox.push('Death Scroll only did ' + power + ' damage')
                    this.note(opponent,-power)
                } else {
                    this.removeItem(user.inventory, user.inventory[index].name);
                    this.messageBox.push(user.name + ' read from the Death Scroll.')
                    this.note(opponent,-opponent.hp)
                    opponent.hp = 0;
                }
            } else {
                this.message = "SOMETHING WENT WRONG"
            }
        },
        addQuest(fromArr, toArr, index) {
            let newQuest = JSON.parse(JSON.stringify(fromArr[index]));
            newQuest.index = index;
            toArr.push(newQuest);
            if (newQuest.type === "fetch") {
                this.fetchQuestCheck(newQuest.task);
            }
        },
        removeQuest(array, index) {
            console.log("quest removed.")
            array.splice(index, 1);
        },
        killQuestCheck(enemyName) {
            let quests = this.player.quests;
            quests.forEach(quest => {
                if (quest.type == "kill" && quest.count < quest.goal) {
                    console.log("kill Quest: " + enemyName);
                    console.log(enemyName + ":" + quest.task)
                    if (enemyName === quest.task) {
                        quest.count++;
                        // if (quest.count >= quest.goal) {
                        //     quest.completed = true
                        // }
                        console.log("QC: " + quest.count)
                    } else {
                        console.log("not correct enemy.")
                    }
                }
            })
        },
        fetchQuestCheck(itemName) {
            let player = this.player;
            let quests = player.quests;
            quests.forEach(quest => {
                if (quest.type == "fetch" && itemName == quest.task) {
                    let qty = 0;
                    console.log("fetch Quest: " + itemName);
                    console.log(itemName + ":" + quest.task)
                    if (this.hasItem(player.inventory, itemName)) {
                        player.inventory.forEach(item => {
                            if (item.name == itemName) {
                                qty = item.qty;
                                if (qty >= quest.goal) {
                                    qty = quest.goal
                                    // quest.completed = true
                                }
                                // else {
                                //     quest.completed = false
                                // }
                            }
                        })
                    }
                    // else {
                    //     quest.completed = false;
                    // }
                    quest.count = qty
                    console.log("quest count: " + quest.count)
                }
            })
            console.log(quests);
        },
        monsterEncounter(alternateMessage) {
            let rangeNum = 0;
            let playerLevel = this.player.level;

            const regionIndex = this.region.index;
            console.log("RI:" + regionIndex)
            const regionLevel = this.region.level;
            const regionTarget = this.region.targetLevel;

            let monsterArray;

            switch (regionIndex) {
                case 1:
                    monsterArray = monsters1;
                        break;
                    case 2:
                        monsterArray = monsters2;
                        break;
                    case 3:
                        monsterArray = monsters3;
                    break;

                default:
                // code block
            }
            if (playerLevel <= regionLevel) {
                rangeNum = 1;

            } else if (playerLevel > regionLevel && playerLevel < regionTarget) {
                rangeNum = Math.ceil(monsterArray.length * (playerLevel / regionTarget));
            } else {
                rangeNum = monsterArray.length;
            }
            let monNum = this.randNum(0, rangeNum);
            const message = alternateMessage || "You encountered " + this.anA(monsterArray[monNum].name) + " " + monsterArray[monNum].name + ".";
            this.message = message;
            console.log("message: " + message);
            this.currentEnemy = JSON.parse(JSON.stringify(monsterArray[monNum]));
            this.currentEnemy.hp = monsterArray[monNum].hpMax;
            this.currentEnemy.mp = monsterArray[monNum].mpMax;
            this.currentEnemy.note = '';
            this.currentEnemy.animation = 'idle';
            this.currentEnemy.isDead = false

            this.addEnemyItems(this.currentEnemy);

            this.changeScene('Battle')

            console.log('Enemy:',this.currentEnemy);
        },
        viciousEncounter(alternateMessage) {
            let rangeNum = 0;
            let playerLevel = this.player.level;

            const regionIndex = this.region.index;
            console.log("RI:" + regionIndex)
            const regionLevel = this.region.level;
            const regionTarget = this.region.targetLevel;

            let monsterArray;

            switch (regionIndex) {
                case 1:
                    monsterArray = monsters1;
                        break;
                    case 2:
                        monsterArray = monsters2;
                        break;
                    case 3:
                        monsterArray = monsters3;
                    break;

                default:
                // code block
            }
            if (playerLevel <= regionLevel) {
                rangeNum = 1;

            } else if (playerLevel > regionLevel && playerLevel < regionTarget) {
                rangeNum = Math.ceil(monsterArray.length * (playerLevel / regionTarget));
            } else {
                rangeNum = monsterArray.length;
            }
            let monNum = this.randNum(0, rangeNum);
            const message = alternateMessage || "You encountered a Vicious " + monsterArray[monNum].name + ".";
            this.message = message;
            console.log("message: " + message);
            this.currentEnemy = JSON.parse(JSON.stringify(monsterArray[monNum]));
            const enemy = this.currentEnemy;
            enemy.name = "Vicious " + monsterArray[monNum].name;
            enemy.type = "vicious";
            enemy.hpMax += 5;
            enemy.hp = monsterArray[monNum].hpMax + 5;
            enemy.mpMax += 5;
            enemy.mp = monsterArray[monNum].mpMax + 5;
            enemy.strength += 3;
            enemy.defense += 3;
            enemy.speed += 2;
            enemy.luck += 1
            enemy.xp += 10;
            enemy.gold += 30;

            enemy.note = '';
            enemy.animation = 'idle';
            enemy.isDead = false

            this.addEnemyItems(this.currentEnemy);

            this.changeScene('Battle')

            console.log('Enemy:',this.currentEnemy);
        },
        endBossEncounter(alternateMessage) {
            const bossIndex = this.region.index - 1;
            let message;
            if (this.movingForward === true) {
                message = alternateMessage || "You face " + endBosses[bossIndex].name + ", " + endBosses[bossIndex].title + ".";
            }
            message = endBosses[bossIndex].name + ", " + endBosses[bossIndex].title + " blocks your path."
            this.message = message;
            console.log("message: " + message);
            this.currentEnemy = JSON.parse(JSON.stringify(endBosses[bossIndex]));
            this.currentEnemy.hp = endBosses[bossIndex].hpMax;
            this.currentEnemy.mp = endBosses[bossIndex].mpMax;
            this.currentEnemy.animation = 'idle';
            this.currentEnemy.isDead = false

            this.addEnemyItems(this.currentEnemy);

            // console.log(this.state.currentEnemy);
            this.changeScene('Battle')

            console.log('Enemy:',this.currentEnemy);
        },
        addEnemyItems(enemy) {
            let randItem = this.randNum(0, items3.length);
            this.addItem(enemy.inventory, items3[randItem]);
            this.addItem(enemy.inventory, items1[0]);
            this.addItem(enemy.inventory, items1[1]);
        },
        chestEncounter() {
            this.currentEncounter = JSON.parse(JSON.stringify(encounters[0]));
            this.message = "You found a chest!"
            this.changeScene('ChestEncounter');
        },
    },
    created: function() {
        this.$parent.menu = false;
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

    .game-header p{
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .location-sprite {
        width: 32px;
        margin: -16px 10px;
        image-rendering: optimizeSpeed;
        image-rendering: -moz-crisp-edges;
        image-rendering: -o-crisp-edges;
        image-rendering: -webkit-optimize-contrast;
        image-rendering: pixelated;
        image-rendering: optimize-contrast;
        -ms-interpolation-mode: nearest-neighbor;
    }

    .message-box {
        border: 2px solid gray;
        border-radius: 10px;
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
