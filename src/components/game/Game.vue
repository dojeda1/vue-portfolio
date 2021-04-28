<template>
<div id="game">
    <div class="container">
        <div class="game-header">
            <h1 class="game-title">Fantasy RPG</h1>
            <p>
                <img class="location-sprite"
                :src="regions[region].sprite">
                {{ regions[region].name}} &middot; {{location}}
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
            <WordGuess v-if="scene == 'WordGuess'"/>
            <Quests v-if="scene == 'Quests'"/>
            <Dungeon v-if="scene == 'Dungeon'"/>
            <Castle v-if="scene == 'Castle'"/>
            <Battle v-if="scene == 'Battle'"/>
            <ChestEncounter v-if="scene == 'ChestEncounter'"/>
            <DungeonEncounter v-if="scene == 'DungeonEncounter'"/>
            <AdventurerEncounter v-if="scene == 'AdventurerEncounter'"/>
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
import WordGuess from './WordGuess.vue'
import Quests from './Quests.vue'
import Save from './Save.vue'
import Stats from './Stats.vue'
import Quit from './Quit.vue'
import Dungeon from './Dungeon.vue'
import Castle from './Castle.vue'
import Battle from './Battle.vue'
import ChestEncounter from './ChestEncounter.vue'
import DungeonEncounter from './DungeonEncounter.vue'
import AdventurerEncounter from './AdventurerEncounter.vue'

//JSON Files
import playerDefault from './data/playerDefault.json';

import regions from "./data/regions.json";

import encounters from "./data/encounters.json";

import monsters from "./data/monsters.json";
import bosses from "./data/bosses.json";
import endBosses from "./data/endBosses.json";
import abilities from "./data/abilities.json";
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
        WordGuess,
        Quests,
        Save,
        Stats,
        Quit,
        Dungeon,
        Castle,
        Battle,
        ChestEncounter,
        DungeonEncounter,
        AdventurerEncounter
    },
    data() {
        return {
            profiles: [],
            message: "",
            messageBox: [],
            infoText: "",
            region: 0,
            regions: regions,
            scene: "TitleScreen",
            location: "Title Screen",
            movingForward: false,
            newEvent: false,
            player: {},
            currentEnemy: {},
            currentEncounter: {},
            questBoard: [],
            merchant: [],
            dungeonCount: 0,
            meadCount: 0,
            // playerDefault: playerDefault,
            encounters: encounters,
            abilities: abilities,
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
            this.player.id = this.randNum(0,1000000);
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
            for (let i = 0; i < this.randNum(1, 2); i++) {
                console.log('Add Items 4')
                randItem = this.randNum(0, items[4].length);
                this.addItem(merchant, items[4][randItem]);
            }
            if (this.regions[this.region].kills > 15
            && this.regions[this.region].bossKills < bosses[this.region].length
            && !this.findItem(this.player.inventory,'Map')
            && this.randNum(1, 3) == 1) {
                this.addItem(merchant, items[4][0]);
            }
            if (this.currentEncounter.name == "Traveling Merchant") {
                if (this.randNum(1, 40) == 1) {
                    console.log('Add Items 6')
                    const itemNum = this.randNum(0, items[6].length)
                    const item = items[6][itemNum];
                    this.addItem(merchant, item);
                }
                if (this.randNum(1, 40) == 1) {
                    console.log('Add Items 7')
                    const itemNum = this.randNum(0, items[7].length)
                    const item = items[7][itemNum];
                    this.addItem(merchant, item);
                }
            }
            console.log('Merchant',this.merchant)
        },
        resetAdventurer() {
            this.merchant = [];
            if (this.player.class == 'Warrior') {
                // this.addItem(this.merchant, items[7][0])
                this.addItem(this.merchant, items[7][1])
                this.addItem(this.merchant, items[7][2])
                // this.addItem(this.merchant, items[7][3])
                this.addItem(this.merchant, items[7][4])
                this.addItem(this.merchant, items[7][5])
            } else if (this.player.class == 'Mage') {
                this.addItem(this.merchant, items[7][0])
                // this.addItem(this.merchant, items[7][1])
                this.addItem(this.merchant, items[7][2])
                this.addItem(this.merchant, items[7][3])
                // this.addItem(this.merchant, items[7][4])
                this.addItem(this.merchant, items[7][5])
            } else if (this.player.class == 'Rogue') {
                this.addItem(this.merchant, items[7][0])
                this.addItem(this.merchant, items[7][1])
                // this.addItem(this.merchant, items[7][2])
                this.addItem(this.merchant, items[7][3])
                this.addItem(this.merchant, items[7][4])
                // this.addItem(this.merchant, items[7][5])
            }
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
        loadGame(profile) {
            console.log('Game Loaded')
            this.message = 'Your game has been Loaded.'
            this.messageBox = [];
            // this.player = JSON.parse(localStorage.getItem("player"));
            // this.regions = JSON.parse(localStorage.getItem("regions"));
            // this.region = JSON.parse(localStorage.getItem("region"));
            this.player = JSON.parse(JSON.stringify(profile.player));
            this.regions = JSON.parse(JSON.stringify(profile.regions));
            this.region = JSON.parse(JSON.stringify(profile.region));
            this.scene = 'Town'
            this.location = 'Town'
            this.resetMerchant()
            this.resetQuestBoard()
            this.$parent.menu = false;
            this.$parent.gameStarted = true;
            console.log('P1:',this.player)
            console.log('R:',this.region)
            console.log('Rs:',this.regions)
        },
        getProfiles() {
            console.log('Get Profiles')
            let profiles = JSON.parse(localStorage.getItem("profiles"));
            console.log('Profiles:',profiles)
            if (profiles === null) {
                console.log('Start New')
                this.profiles = [];
            } else {
                console.log('Start Old')
                this.profiles = profiles;
            }
        },
        saveGame() {
            console.log('Attempt save')
            if (this.getProfileById(this.player.id)) {
                let pro = this.getProfileById(this.player.id)
                console.log('PRO:',pro)
                pro.player = JSON.parse(JSON.stringify(this.player)),
                pro.regions = JSON.parse(JSON.stringify(this.regions)),
                pro.region = JSON.parse(JSON.stringify(this.region))
            } else {
                let pro = {
                    id: this.player.id,
                    player: JSON.parse(JSON.stringify(this.player)),
                    regions: JSON.parse(JSON.stringify(this.regions)),
                    region: JSON.parse(JSON.stringify(this.region))
                }
                console.log('New PRO:',pro)
                this.profiles.push(pro);
            }
            localStorage.setItem("profiles", JSON.stringify(this.profiles));
            console.log('Game Saved')
            this.message = 'Your game has been Saved.'
        },
        getProfileById(id) {
            let profile = false;
            this.profiles.forEach(function(pro) {
                if (pro.id == id) {
                    profile = pro;
                }
            })
            return profile;
        },
        checkCompletion(profile) {
            console.log('PRO:',profile)
            let completion = Math.floor(
            ((profile.regions[0].endBossKills + 
            profile.regions[0].bossKills + 
            profile.regions[1].endBossKills + 
            profile.regions[1].bossKills + 
            profile.regions[2].endBossKills + 
            profile.regions[2].bossKills)
            /
            (endBosses.length +
            bosses[0].length +
            bosses[1].length +
            bosses[2].length))
            * 100)
            return completion
        },
        enterEvent() {
            this.newEvent = true;
            const $this = this;
            setTimeout(function() {
                $this.newEvent = false;
            },300)
        },
        changeScene(nextScene) {
            this.scene = nextScene;
        },
        randNum(x, y) {
            return Math.floor(Math.random() * y) + x;
        },
        anA(word) {
            let first = word.charAt(0);
            let anA = "";
            if (first == "A" || first == "E" || first == "I" || first == "O" || first == "U") {
                anA = "an";
                return anA;
            } else {
                anA = "a";
                return anA;
            }
        },
        moveForward() {
            console.log('Move On')
            this.movingForward = false;
            this.player.animation = 'walk'
            this.playerTurn = false
            const $this = this;
            setTimeout(function() {
                $this.messageBox = [];
                $this.region++
                $this.regions[$this.region].discovered = true;
                $this.message = 'You moved on to the ' + $this.regions[$this.region].name
                $this.player.animation = 'idle'
                $this.playerTurn = true
                $this.changeScene('Wild');
            },1200)
        },
        findAbility(moveList, abilityName){
            let foundAbility = false;
            moveList.forEach(ability => {
                if (abilityName === ability.name) {
                    foundAbility = ability;
                }
            })
            return foundAbility
        },
        addAbility(toArr,name,active) {
            console.log(toArr,name,active)
            let newAbility;
            if (this.findAbility(toArr,name)) {
                console.log('had')
                newAbility = this.findAbility(toArr,name);
                newAbility.active = active;
            } else {
                console.log('not had')
                newAbility = JSON.parse(JSON.stringify(this.findAbility(abilities,name)));
                newAbility.active = active;
                toArr.push(newAbility);
            }
            console.log('New Ab:',newAbility);
            console.log('New P:',this.player);
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
            const transferItem = JSON.parse(JSON.stringify(item));
            this.addItem(toArr, transferItem);
            this.removeItem(fromArr, transferItem.name);
            console.log('fromArr:',fromArr)
            console.log('toArr:',toArr)
        },
        handleAttemptItem(item,index,cb) {
            console.log('chosen item: ' + item.name + ' ' + index)
            if (item.name == 'Old Hat') {
                this.message = 'It looks good on you...'
                this.player.itemSprite = item.sprite;
                this.player.itemName = item.name;
                this.player.animation = 'use item';
                this.itemNote(this.player,item);
                const $this = this;
                setTimeout(function() {
                    $this.player.animation = 'idle';
                }, 600)
            } else if (item.name == 'Rare Gem') {
                this.message = 'It glistens in the light...'
                this.player.itemSprite = item.sprite;
                this.player.itemName = item.name;
                this.player.animation = 'use item';
                this.itemNote(this.player,item);
                const $this = this;
                setTimeout(function() {
                    $this.player.animation = 'idle';
                }, 600)
            } else if (item.name.includes('Tome') && this.findAbility(this.player.abilities,item.ability).active) {
                this.message = "You've already learned " + item.ability + '.'
            } else if (item.name == 'Fairy' &&  this.player.hp > 0) {
                this.message = 'Fairy can only be used if dead.'
            } else if (item.name == 'Head of Asteroth' && item.charge > 0) {
                const kill = item.charge != 1 ? 'kills' : 'kill';
                this.message = 'Head of Asteroth needs ' + item.charge + ' more ' + kill + ' to recharge.'
            } else if (item.name == 'Head of Asteroth' && this.scene != 'Battle') {
                this.message = 'Head of Asteroth can only be used in battle.'
            } else if (item.name == 'Flame Scroll' && this.scene != 'Battle') {
                this.message = 'Flame Scroll can only be used in battle.'
            } else if (item.name == 'Blood Scroll' && this.scene != 'Battle') {
                this.message = 'Blood Scroll can only be used in battle.'
            } else if (item.name == 'Venom Scroll' && this.scene != 'Battle') {
                this.message = 'Venom Scroll can only be used in battle.'
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
        itemNote(character,item) {
            character.itemSprite = item.sprite;
            setTimeout(function() {
                character.itemSprite = ''
            },1000);
        },
        abilityNote(character,ability) {
            console.log('Ability Sprite')
            character.abilitySprite = ability.sprite;
            setTimeout(function() {
                character.abilitySprite = ''
            },150);
        },
        activateItem(user, opponent, item) {
            const itemName = item.name;
            // const $this = this;
            user.itemSprite = item.sprite;
            user.animation = 'use item';
            this.itemNote(user,item);
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
            } else if (item.name.includes('Tome')) {
                this.addAbility(user.abilities,item.ability,true);
                this.removeItem(user.inventory, itemName);
                this.messageBox.push(user.name + ' learned ' + item.ability + '!!!')
                // this.note(user,'+' + amount)
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
                const move = {
                    "sprite": "/images/game/spell.png",
                }
                if (opponent.type === 'finalBoss') {
                    let damage = Math.ceil(opponent.hp / 3);
                    opponent.hp -= damage;
                    this.removeItem(user.inventory, itemName);
                    const $this = this;
                    setTimeout(function() {
                        opponent.animation = 'damage'
                        $this.note(opponent,-damage)
                        $this.abilityNote(user,move)
                    },300);
                    this.messageBox.push('Death Scroll only did ' + damage + ' damage')
                } else if (opponent.type === 'boss' || opponent.type === 'endBoss') {
                    let damage = Math.ceil(opponent.hp / 2);
                    opponent.hp -= damage;
                    this.removeItem(user.inventory, itemName);
                    const $this = this;
                    setTimeout(function() {
                        opponent.animation = 'damage'
                        $this.note(opponent,-damage)
                        $this.abilityNote(user,move)
                    },300);
                    this.messageBox.push('Death Scroll only did ' + damage + ' damage')
                } else {
                    let damage = opponent.hp;
                    opponent.hp -= damage;
                    this.removeItem(user.inventory, itemName);
                    const $this = this;
                    setTimeout(function() {
                        opponent.animation = 'damage'
                        $this.note(opponent,-damage)
                        $this.abilityNote(user,move)
                    },300);
                    this.messageBox.push(user.name + ' read from the Death Scroll.')
                }
            } else if (item.name == 'Flame Scroll') {
                const move = {
                    "sprite": "/images/game/spell.png",
                }
                const $this = this;
                let missCheck = this.randNum(1, 100);
                let luckCheck = (user.luck - opponent.luck) + 10;
                if (luckCheck > 95) {
                    luckCheck = 95;
                } else if (luckCheck < 5) {
                    luckCheck = 5;
                }
                if (missCheck < luckCheck) {
                    this.removeItem(user.inventory, itemName);
                    setTimeout(function() {
                        opponent.animation = 'dodge'
                        $this.note(opponent,'miss')
                        $this.abilityNote(user,move)
                    },300);
                    this.messageBox.push(user.name + "'s Fire Scroll missed...");
                } else {
                    let damage = user.will;
                    opponent.hp -= damage;
                    this.removeItem(user.inventory, itemName);
                    setTimeout(function() {
                        opponent.animation = 'damage'
                        opponent.status['Burn'] = 4;
                        $this.note(opponent,-damage)
                        $this.abilityNote(user,move)
                    },300);
                    this.messageBox.push('Flame Scroll did ' + damage + ' damage')
                    this.messageBox.push("- " + opponent.name + " is now Burned. -");
                }
            } else if (item.name == 'Venom Scroll') {
                const move = {
                    "sprite": "/images/game/spell.png",
                }
                const $this = this;
                let missCheck = this.randNum(1, 100);
                let luckCheck = (user.luck - opponent.luck) + 10;
                if (luckCheck > 95) {
                    luckCheck = 95;
                } else if (luckCheck < 5) {
                    luckCheck = 5;
                }
                if (missCheck < luckCheck) {
                    this.removeItem(user.inventory, itemName);
                    setTimeout(function() {
                        opponent.animation = 'dodge'
                        $this.note(opponent,'miss')
                        $this.abilityNote(user,move)
                    },300);
                    this.messageBox.push(user.name + "'s Venom Scroll missed...");
                } else {
                    let damage = user.will;
                    opponent.hp -= damage;
                    this.removeItem(user.inventory, itemName);
                    setTimeout(function() {
                        opponent.animation = 'damage'
                        opponent.status['Poison'] = 4;
                        $this.note(opponent,-damage)
                        $this.abilityNote(user,move)
                    },300);
                    this.messageBox.push('Venom Scroll did ' + damage + ' damage')
                    this.messageBox.push("- " + opponent.name + " is now Poisoned. -");
                }
            } else if (item.name == 'Blood Scroll') {
                const move = {
                    "sprite": "/images/game/spell.png",
                }
                const $this = this;
                let missCheck = this.randNum(1, 100);
                let luckCheck = (user.luck - opponent.luck) + 10;
                if (luckCheck > 95) {
                    luckCheck = 95;
                } else if (luckCheck < 5) {
                    luckCheck = 5;
                }
                if (missCheck < luckCheck) {
                    this.removeItem(user.inventory, itemName);
                    setTimeout(function() {
                        opponent.animation = 'dodge'
                        $this.note(opponent,'miss')
                        $this.abilityNote(user,move)
                    },300);
                    this.messageBox.push(user.name + "'s Blood Scroll missed...");
                } else {
                    let damage = user.will;
                    opponent.hp -= damage;
                    this.removeItem(user.inventory, itemName);
                    setTimeout(function() {
                        opponent.animation = 'damage'
                        opponent.status['Bleed'] = 4;
                        $this.note(opponent,-damage)
                        $this.abilityNote(user,move)
                    },300);
                    this.messageBox.push('Blood Scroll did ' + damage + ' damage')
                    this.messageBox.push("- " + opponent.name + " is now Bleeding. -");
                }
            } else if (item.name == 'Head of Asteroth') {
                const move = {
                    "sprite": "/images/game/evil-eye.png",
                }
                let damage = opponent.hp - 5;
                if (damage < 0) {
                    damage = 0;
                }
                opponent.hp -= damage;
                item.charge = item.goal;
                const $this = this;
                setTimeout(function() {
                    opponent.animation = 'damage'
                    $this.note(opponent,-damage);
                    $this.abilityNote(user,move)
                },300);
                this.messageBox.push('The eyes of Asteroth bring ' + opponent.name + ' to the brink of death.')
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
        killQuestCheck(enemy) {
            let quests = this.player.quests;
            let questComplete = false;
            quests.forEach(quest => {
                if (quest.type == "kill" && quest.count < quest.goal && quest.region == this.regions[this.region].name) {
                    console.log("kill Quest: " + enemy.name);
                    console.log(enemy.name + ":" + quest.task)
                    if (enemy.name === quest.task) {
                        quest.count++;
                        console.log("QC: " + quest.count)
                    } else {
                        console.log("not correct enemy.")
                    }
                } else if(quest.type == "killType" && quest.count < quest.goal && quest.region == this.regions[this.region].name) {
                    console.log("kill Quest: " + enemy.type);
                    console.log(enemy.type + ":" + quest.task)
                    if (enemy.type === quest.task) {
                        quest.count++;
                        console.log("QC: " + quest.count)
                    } else {
                        console.log("not correct enemy.")
                    }
                }
                if (quest.count >= quest.goal) {
                    questComplete = true;
                }
            });
            this.$parent.questComplete = questComplete;
        },
        fetchQuestCheck(itemName) {
            let player = this.player;
            let quests = player.quests;
            let questComplete = false;
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
                if (quest.count >= quest.goal) {
                    questComplete = true;
                }
            })
            this.$parent.questComplete = questComplete;
            console.log(quests);
        },
        statusCheck(user) {
            console.log('Status Check...')
            let totalDamage = 0;
            let damage;
            if (user.status['Bleed'] > 0) {
                damage = 5
                totalDamage+= damage;
                this.messageBox.push('- Bleed hurt ' + user.name + ' for ' +  damage + ' damage. -')
                user.hp-= damage;
                user.status['Bleed']--
                if (user.status['Bleed'] <= 0) {
                    this.messageBox.push('- Bleed has worn out. -')
                }
            }
            if (user.status['Burn'] > 0) {
                damage = 5
                totalDamage+= damage;
                this.messageBox.push('- Burn hurt ' + user.name + ' for ' +  damage + ' damage. -')
                user.hp-= damage;
                user.status['Burn']--
                if (user.status['Burn'] <= 0) {
                    this.messageBox.push('- Burn has worn out. -')
                }
            }
            if (user.status['Poison'] > 0) {
                damage = 5
                totalDamage+= damage;
                this.messageBox.push('- Poison hurt ' + user.name + ' for ' +  damage + ' damage. -')
                user.hp-= damage;
                user.status['Poison']--
                if (user.status['Poison'] <= 0) {
                    this.messageBox.push('- Poison has worn out. -')
                }
            }
            const $this = this
            setTimeout(function() {
                $this.note(user,-totalDamage);
            },650);
        },
        dungeonEncounter(usedItem) {
            this.currentEncounter = JSON.parse(JSON.stringify(this.encounters[2]));
            usedItem ? this.message = "Map lead you to a Dungeon!" : this.message = "You discovered a Dungeon!";
            this.changeScene('DungeonEncounter');
            this.enterEvent();
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

            this.addEnemyAbilities(enemy);
            this.addEnemyItems(enemy);

            this.changeScene('Battle')
            this.enterEvent();

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

            this.addEnemyAbilities(enemy);
            this.addEnemyItems(enemy);

            this.changeScene('Battle')
            this.enterEvent();

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

            this.addEnemyAbilities(enemy);
            this.addEnemyItems(enemy);

            // console.log(this.state.currentEnemy);
            this.changeScene('Battle')
            this.enterEvent();

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
            if (enemy.type == "vicious") {
                for (let i = 0; i < this.randNum(0, 1); i++) {
                    console.log('Add Items 4')
                    randItem = this.randNum(0, items[4].length);
                    this.addItem(enemy.inventory, items[4][randItem]);
                }
            }
            if (enemy.name == "Mimic"
            || enemy.type == "boss"
            || enemy.type == "endBoss") {
                for (let i = 0; i < this.randNum(0, 1); i++) {
                    console.log('Add Items 5')
                    randItem = this.randNum(0, items[5].length);
                    this.addItem(enemy.inventory, items[5][randItem]);
                }
            }
        },
        addEnemyAbilities(enemy) {
            console.log('enemy',enemy)
            let moveList = enemy.abilities;
            console.log('abilities',moveList)
            let abilities = [];
            const $this = this;
            moveList.forEach(function(ability) {
                console.log('AB:',ability)
                $this.addAbility(abilities,ability,true);
            });
            enemy.abilities = abilities
        },
        chestEncounter() {
            this.currentEncounter = JSON.parse(JSON.stringify(encounters[0]));
            this.message = "You found a chest!"
            this.changeScene('ChestEncounter');
            this.enterEvent();
        },
        merchantEncounter() {
            this.currentEncounter = JSON.parse(JSON.stringify(this.encounters[6]));
            this.resetMerchant();
            this.message = "You came across a Traveling Merchant!"
            this.currentEncounter.animation = 'idle'
            this.changeScene('Shop');
            this.enterEvent();
        },
        adventurerEncounter() {
            this.currentEncounter = JSON.parse(JSON.stringify(this.encounters[7]));
            this.resetAdventurer();
            this.message = "An Old Adventurer offers you some wisdom."
            this.messageBox = [];
            this.infoText = 'Select an item'
            this.currentEncounter.animation = 'idle'
            this.changeScene('AdventurerEncounter');
            this.enterEvent();
        },
    },
    created: function() {
        this.$parent.menu = false;
        this.getProfiles()
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

    #game input {
        font-family: 'Montserrat Alternates', sans-serif;
        font-size: 100%;
        background-color: transparent;
        border: 2px solid gray;
        border-radius: 10px;
        padding: 10px;
        /* color: white; */
        margin-right: 10px;
    }

    #game input:focus {
        outline: none;
        border-color: white;
    }

    @media screen and (max-width: 600px) {
        #game {
            width: 100%;
            height: calc(100% - 60px);
            top: 60px;

            /* animation: slide-in-top 3s; */
        }
        .game-title {
            display: none;
        }
    }
</style>
