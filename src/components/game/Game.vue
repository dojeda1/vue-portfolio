<template>
<div id="game">
    <div class="container">
        <div class="game-header">
            <h1>Fantasy RPG</h1>
            <p>
                <img class="location-sprite"
                :src="regions[region].sprite">
                {{ regions[region].name}} | {{location}}
                <img class="location-sprite"
                :src="regions[region].sprite">
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
            <Castle v-if="scene == 'Castle'"/>
            <Battle v-if="scene == 'Battle'"/>
            <ChestEncounter v-if="scene == 'ChestEncounter'"/>
            <DungeonEncounter v-if="scene == 'DungeonEncounter'"/>
        </template>
        <template v-else>
            <Save v-if="$parent.menu == 'Save'"/>
            <Quests v-if="$parent.menu == 'Quests'"/>
            <Stats v-if="$parent.menu == 'Stats'"/>
            <Quit v-if="$parent.menu == 'Quit'"/>
        </template>
        <p class="text-gray">SCENE: {{scene}}</p>
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
import Save from './Save.vue'
import Stats from './Stats.vue'
import Quit from './Quit.vue'
import Dungeon from './Dungeon.vue'
import Castle from './Castle.vue'
import Battle from './Battle.vue'
import ChestEncounter from './ChestEncounter.vue'
import DungeonEncounter from './DungeonEncounter.vue'

//JSON Files
import playerDefault from './data/playerDefault.json';

import regions from "./data/regions.json";

import encounters from "./data/encounters.json";

import monsters from "./data/monsters.json";
import bosses from "./data/bosses.json";
import endBosses from "./data/endBosses.json";
import items from "./data/items.json";

import quests from "./data/quests.json";

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
        Save,
        Stats,
        Quit,
        Dungeon,
        Castle,
        Battle,
        ChestEncounter,
        DungeonEncounter
    },
    data() {
        return {
            message: "",
            messageBox: [],
            infoText: "",
            region: 0,
            regions: regions,
            scene: "TitleScreen",
            location: "Title Screen",
            movingForward: false,
            player: {},
            currentEnemy: {},
            currentEncounter: {},
            questBoard: [],
            merchant: [],
            dungeonCount: 0,
            meadCount: 0,
            // playerDefault: playerDefault,
            encounters: encounters,
            items: items,
            monsters: monsters,
            bosses: bosses,
            endBosses: endBosses
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
            this.bosses = JSON.parse(JSON.stringify(bosses));
            this.endBosses = JSON.parse(JSON.stringify(endBosses));
        },
        resetMerchant() {
            let randItem;
            this.merchant = [];
            const merchant = this.merchant;
            // this.addItem(merchant,items[0][0]);
            for (let i = 0; i < 4; i++) {
                console.log('Add Items 0')
                randItem = this.randNum(0, items[0].length);
                this.addItem(merchant, items[0][randItem]);
            }
            for (let i = 0; i < 2; i++) {
                console.log('Add Items 1')
                randItem = this.randNum(0, items[1].length);
                this.addItem(merchant, items[1][randItem]);
            }
            for (let i = 0; i < this.randNum(1, 2); i++) {
                console.log('Add Items 2')
                randItem = this.randNum(0, items[2].length);
                this.addItem(merchant, items[2][randItem]);
            }
            for (let i = 0; i < this.randNum(1, 3); i++) {
                console.log('Add Items 3')
                randItem = this.randNum(0, items[3].length);
                this.addItem(merchant, items[3][randItem]);
            }
            if (this.currentEncounter.name == "Traveling Merchant") {
                for (let i = 0; i < this.randNum(1, 3); i++) {
                    console.log('Add Items 4')
                    randItem = this.randNum(0, items[4].length);
                    this.addItem(merchant, items[4][randItem]);
                }
            } else {
                for (let i = 0; i < this.randNum(0, 1); i++) {
                    console.log('Add Items 4')
                    randItem = this.randNum(0, items[4].length);
                    this.addItem(merchant, items[4][randItem]);
                }
            }
            console.log('Merchant',this.merchant)
        },
        resetQuestBoard() {
            let randQuest;
            let questArr = quests[this.region];
            this.questBoard = [];
            for (let i = 0; i < 3; i++) {
                randQuest = this.randNum(0, questArr.length);
                this.addQuest(questArr, this.questBoard, randQuest)
            }
            console.log('QuestBoard:',this.questBoard)
        },
        resetRegion() {
            this.regions = JSON.parse(JSON.stringify(regions))
            this.region = 0;
        },
        loadGame() {
            console.log('Game Loaded')
            this.message = 'Your game has been Loaded.';
            this.messageBox = [];
            this.player = JSON.parse(localStorage.getItem("player"));
            this.regions = JSON.parse(localStorage.getItem("regions"));
            this.region = JSON.parse(localStorage.getItem("region"));
            this.bosses = JSON.parse(localStorage.getItem("bosses"));
            this.scene = 'Town'
            this.location = 'Town'
            this.resetMerchant()
            this.resetQuestBoard()
            console.log('P1:',this.player)
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
        findItem(inventory, itemName){
            let foundItem = false;
            inventory.forEach(item => {
                if (itemName === item.name) {
                    foundItem = item;
                }
            })
            return foundItem
        },
        findSpecial(moveList, specialName){
            let foundSpecial = false;
            moveList.forEach(special => {
                if (specialName === special.name) {
                    foundSpecial = special;
                }
            })
            return foundSpecial
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
            } else if (item.name == 'Fairy' &&  this.player.hp > 0) {
                this.message = 'Fairy can only be used if dead.'
            } else if (item.name == 'Head of Asteroth' && this.scene != 'Battle') {
                this.message = 'Head of Asteroth can only be used in battle.'
            } else if (item.name == 'Head of Asteroth' && item.charge > 0) {
                this.message = 'Head of Asteroth needs ' + item.charge + ' more kills to recharge.'
            } else if (item.name == 'Death Scroll' && this.scene != 'Battle') {
                this.message = 'Death Scroll can only be used in battle.'
            } else if (item.name == 'Map' && this.scene != 'Wild') {
                this.message = 'Map can only be used in the Wild.'
            } else if (item.name.includes('Health Potion') && this.player.hp >= this.player.hpMax) {
                this.message = 'You are already at full Health.'
            } else if (item.name.includes('Mana Potion') && this.player.mp >= this.player.mpMax) {
                this.message = 'You are already at full Mana.'
            } else if (item.name == 'Bandage' && this.player.status['Bleed'] <= 0) {
                this.message = "You aren't currently Bleeding."
            } else if (item.name == 'Antidote' && this.player.status['Poison'] <= 0) {
                this.message = "You aren't currently Poisoned."
            } else if (item.name == 'Remedy' && this.player.status['Burn'] <= 0) {
                this.message = "You aren't currently Burned."
            } else {
                this.messageBox = [];
                console.log('PLAYER',this.player)
                this.activateItem(this.player, this.currentEnemy, item);
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
        activateItem(user, opponent, item) {
            const itemName = item.name;
            // const $this = this;
            user.itemSprite = item.sprite;
            user.animation = 'use item';
            console.log('opponent',opponent)
            console.log("item: " + item.name)
            if (item.name.includes('Health Potion')) {
                let amount = Math.floor(user.hpMax * item.recover);
                user.hp += amount;
                if (user.hp > user.hpMax) {
                    user.hp = user.hpMax
                }
                this.removeItem(user.inventory, itemName);
                this.messageBox.push(user.name + ' recovered ' + amount + ' HP.')
                this.note(user,'+' + amount)
            } else if (item.name.includes('Mana Potion')) {
                let amount = Math.floor(user.mpMax * item.recover);
                user.mp += amount;
                if (user.mp > user.mpMax) {
                    user.mp = user.mpMax
                }
                this.removeItem(user.inventory, itemName);
                this.messageBox.push(user.name + ' recovered ' + amount + ' MP.')
                this.note(user,'+' + amount)
            } else if (item.name == 'Bandage') {
                user.status['Bleed'] = 0;
                this.removeItem(user.inventory, itemName);
                this.messageBox.push(user.name + "'s Bleed is healed.")
                // this.note(user,'!')
            } else if (item.name == 'Antidote') {
                user.status['Poison'] = 0;
                this.removeItem(user.inventory, itemName);
                this.messageBox.push(user.name + "'s Poison is cured")
                // this.note(user,'!')
            } else if (item.name == 'Remedy') {
                user.status['Burn'] = 0;
                this.removeItem(user.inventory, itemName);
                this.messageBox.push(user.name + "'s Burn is healed.")
                // this.note(user,'!')
            } else if (item.name == 'Map') {
                this.removeItem(user.inventory, itemName);
                this.message = user.name + ' used Map.'
                this.dungeonEncounter(true);
            } else if (item.name == 'Fairy') {
                let amount = Math.floor(user.hpMax * item.recover);
                user.isDead = false;
                user.hp = amount;
                this.removeItem(user.inventory, itemName);
                this.messageBox.push(user.name + ' was revived by Fairy')
                this.note(user,'+' + amount)
                setTimeout(function() {
                    user.animation = 'idle'
                },600);
            } else if (item.name == 'Death Scroll') {
                if (opponent.type === 'boss' || opponent.type === 'endBoss') {
                    let power = Math.ceil(opponent.hp / 2);
                    opponent.hp -= power;
                    this.removeItem(user.inventory, itemName);
                    this.messageBox.push('Death Scroll only did ' + power + ' damage')
                    this.note(opponent,-power)
                } else {
                    this.removeItem(user.inventory, itemName);
                    this.messageBox.push(user.name + ' read from the Death Scroll.')
                    this.note(opponent,-opponent.hp)
                    opponent.hp = 0;
                }
            } else if (item.name == 'Head of Asteroth') {
                let power = opponent.hp;
                opponent.hp -= power;
                item.charge = 6;
                this.messageBox.push('The eyes of Asteroth pierce ' + opponent.name + "'s soul.")
                this.note(opponent,-power)
            } else if (item.name == 'Health Elixir') {
                let amount = item.amount;
                user.hpMax += amount;
                user.hp += amount;
                this.removeItem(user.inventory, itemName);
                this.messageBox.push(user.name + "'s max HP has inceased by " + amount + '.')
                this.note(user,'+' + amount)
            } else if (item.name == 'Mana Elixir') {
                let amount = item.amount;
                user.mpMax += amount;
                user.mp += amount;
                this.removeItem(user.inventory, itemName);
                this.messageBox.push(user.name + "'s max MP has inceased by " + amount + '.')
                this.note(user,'+' + amount)
            } else if (item.name == 'Strength Elixir') {
                let amount = item.amount;
                user.strength += amount;
                this.removeItem(user.inventory, itemName);
                this.messageBox.push(user.name + "'s Strength has inceased by " + amount + '.')
                this.note(user,'+' + amount)
            } else if (item.name == 'Defense Elixir') {
                let amount = item.amount;
                user.defense += amount;
                this.removeItem(user.inventory, itemName);
                this.messageBox.push(user.name + "'s Defense has inceased by " + amount + '.')
                this.note(user,'+' + amount)
            } else if (item.name == 'Will Elixir') {
                let amount = item.amount;
                user.will += amount;
                this.removeItem(user.inventory, itemName);
                this.messageBox.push(user.name + "'s Will has inceased by " + amount + '.')
                this.note(user,'+' + amount)
            } else if (item.name == 'Speed Elixir') {
                let amount = item.amount;
                user.speed += amount;
                this.removeItem(user.inventory, itemName);
                this.messageBox.push(user.name + "'s Speed has inceased by " + amount + '.')
                this.note(user,'+' + amount)
            } else if (item.name == 'Luck Elixir') {
                let amount = item.amount;
                user.luck += amount;
                this.removeItem(user.inventory, itemName);
                this.messageBox.push(user.name + "'s Luck has inceased by " + amount + '.')
                this.note(user,'+' + amount)
            } else {
                this.message = "Item does nothing."
            }
            if (this.scene == 'Battle') {
                this.statusCheck(user);
            }
        },
        addQuest(fromArr, toArr, index) {
            let newQuest = JSON.parse(JSON.stringify(fromArr[index]));
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
                    if (this.findItem(player.inventory, itemName)) {
                        player.inventory.forEach(item => {
                            if (item.name == itemName) {
                                qty = item.qty;
                                if (qty >= quest.goal) {
                                    qty = quest.goal
                                }
                            }
                        })
                    }
                    quest.count = qty
                    console.log("quest count: " + quest.count)
                }
            })
            console.log(quests);
        },
        statusCheck(user) {
            const $this = this
            var totalDamage = 0;
            var damage;
            setTimeout(function() {
                if (user.status['Bleed'] > 0) {
                    damage = 5
                    totalDamage+= damage;
                    $this.messageBox.push('- Bleed hurt ' + user.name + ' for ' +  damage + ' damage. -')
                    user.hp-= 5;
                    user.status['Bleed']--
                    if (user.status['Bleed'] <= 0) {
                        $this.messageBox.push('- Bleed has worn out. -')
                    }
                    $this.note(user,-damage);
                }
                if (user.status['Burn'] > 0) {
                    damage = 5
                    totalDamage+= damage;
                    $this.messageBox.push('- Burn hurt ' + user.name + ' for ' +  damage + ' damage. -')
                    user.hp-= 5;
                    user.status['Burn']--
                    if (user.status['Burn'] <= 0) {
                        $this.messageBox.push('- Burn has worn out. -')
                    }
                    $this.note(user,-damage);
                }
                if (user.status['Poison'] > 0) {
                    damage = 5
                    totalDamage+= damage;
                    $this.messageBox.push('- Poison hurt ' + user.name + ' for ' +  damage + ' damage. -')
                    user.hp-= 5;
                    user.status['Poison']--
                    if (user.status['Poison'] <= 0) {
                        $this.messageBox.push('- Poison has worn out. -')
                    }
                }
                $this.note(user,-totalDamage);
            },650);
        },
        dungeonEncounter(usedItem) {
            this.currentEncounter = JSON.parse(JSON.stringify(this.encounters[2]));
            usedItem ? this.message = "Map lead you to a Dungeon!" : this.message = "You discovered a Dungeon!";
            this.changeScene('DungeonEncounter');
        },
        monsterEncounter(ambushed) {
            let rangeNum = 0;
            let playerLevel = this.player.level;

            const regionIndex = this.region;
            console.log("RI:" + regionIndex)
            const regionLevel = this.regions[regionIndex].level;
            const regionTarget = this.regions[regionIndex].targetLevel;
            let monsterArray = monsters[regionIndex];
            if (playerLevel <= regionLevel) {
                rangeNum = 1;

            } else if (playerLevel > regionLevel && playerLevel < regionTarget) {
                rangeNum = Math.ceil(monsterArray.length * (playerLevel / regionTarget));
            } else {
                rangeNum = monsterArray.length;
            }
            let monNum = this.randNum(0, rangeNum);
            let message;
            this.currentEnemy = JSON.parse(JSON.stringify(monsterArray[monNum]));
            const enemy = this.currentEnemy;
            if (ambushed) {
                message = "You were ambushed by " + this.anA(enemy.name) + " " + enemy.name + "!";
            } else {
                message = "You encountered " + this.anA(enemy.name) + " " + enemy.name + ".";
            }
            this.message = message;
            console.log("message: " + message);
            enemy.hp = monsterArray[monNum].hpMax;
            enemy.mp = monsterArray[monNum].mpMax;
            enemy.note = '';
            enemy.animation = 'idle';
            enemy.isDead = false
            enemy.ambushing = ambushed

            this.addEnemyItems(enemy);

            this.changeScene('Battle')

            console.log('Enemy:',this.currentEnemy);
        },
        viciousEncounter(ambushed) {
            let rangeNum = 0;
            let playerLevel = this.player.level;

            const regionIndex = this.region;
            console.log("RI:" + regionIndex)
            const regionLevel = this.regions[regionIndex].level;
            const regionTarget = this.regions[regionIndex].targetLevel;

            let monsterArray = monsters[regionIndex];
            if (playerLevel <= regionLevel) {
                rangeNum = 1;

            } else if (playerLevel > regionLevel && playerLevel < regionTarget) {
                rangeNum = Math.ceil(monsterArray.length * (playerLevel / regionTarget));
            } else {
                rangeNum = monsterArray.length;
            }
            let monNum = this.randNum(0, rangeNum);
            this.currentEnemy = JSON.parse(JSON.stringify(monsterArray[monNum]));
            const enemy = this.currentEnemy;
            let message;
            if (ambushed) {
                message = "You were ambushed by a Vicious " + enemy.name + "!";
            } else {
                message = "You encountered a Vicious " + enemy.name + ".";
            }
            this.message = message;
            console.log("message: " + message);
            enemy.name = "Vicious " + enemy.name;
            enemy.type = "vicious";
            enemy.hpMax += 5;
            enemy.hp = enemy.hpMax;
            enemy.mpMax += 5;
            enemy.mp = enemy.mpMax;
            enemy.strength += 3;
            enemy.defense += 3;
            enemy.speed += 2;
            enemy.luck += 1
            enemy.xp += 10;
            enemy.gold += 30;

            enemy.note = '';
            enemy.animation = 'idle';
            enemy.isDead = false
            enemy.ambushing = ambushed

            this.addEnemyItems(enemy);

            this.changeScene('Battle')

            console.log('Enemy:',this.currentEnemy);
        },
        endBossEncounter(alternateMessage) {
            const bossIndex = this.region;
            this.currentEnemy = JSON.parse(JSON.stringify(endBosses[bossIndex]));
            const enemy = this.currentEnemy;
            let message;
            if (this.movingForward === true) {
                message = enemy.name + ", " + enemy.title + " blocks your path."
            } else {
                message = alternateMessage || "You face " + enemy.name + ", " + enemy.title + ".";
            }
            this.message = message;
            console.log("message: " + message);
            enemy.hp = enemy.hpMax;
            enemy.mp = enemy.mpMax;
            enemy.animation = 'idle';
            enemy.isDead = false

            this.addEnemyItems(enemy);

            // console.log(this.state.currentEnemy);
            this.changeScene('Battle')

            console.log('Enemy:',this.currentEnemy);
        },
        addEnemyItems(enemy) {
            let randItem;
            console.log('Add Items 0')
            this.addItem(enemy.inventory, items[0][0]);
            this.addItem(enemy.inventory, items[0][1]);
            for (let i = 0; i < this.randNum(0, 2); i++) {
                console.log('Add Items 1')
                randItem = this.randNum(0, items[1].length);
                this.addItem(enemy.inventory, items[1][randItem]);
            }
            for (let i = 0; i < this.randNum(0, 1); i++) {
                console.log('Add Items 2')
                randItem = this.randNum(0, items[2].length);
                this.addItem(enemy.inventory, items[2][randItem]);
            }
            for (let i = 0; i < this.randNum(0, 1); i++) {
                console.log('Add Items 3')
                randItem = this.randNum(0, items[3].length);
                this.addItem(enemy.inventory, items[3][randItem]);
            }
            if (enemy.name == "Mimic"
            || enemy.type == "boss"
            || enemy.type == "endBoss") {
                for (let i = 0; i < this.randNum(0, 1); i++) {
                    console.log('Add Items 4')
                    randItem = this.randNum(0, items[4].length);
                    this.addItem(enemy.inventory, items[4][randItem]);
                }
            }
        },
        chestEncounter() {
            this.currentEncounter = JSON.parse(JSON.stringify(encounters[0]));
            this.message = "You found a chest!"
            this.changeScene('ChestEncounter');
        },
        merchantEncounter() {
            this.currentEncounter = JSON.parse(JSON.stringify(this.encounters[6]));
            this.resetMerchant();
            this.message = "You came across a Traveling Merchant!"
            this.currentEncounter.animation = 'idle'
            this.changeScene('Shop');
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
