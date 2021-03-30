<template>
<div id="game">
    <div class="container">
        <div class="game-header">
            <!-- <p>{{player.name}} | {{player.race}} | {{player.class}}</p> -->
            <h1>Fantasy RPG</h1>
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
            // messageSub: "",
            infoText: [],
            region: regions[0],
            scene: "TitleScreen",
            location: "Title Screen",
            // task: "new or load",
            // step: null,
            // inputName: "",
            // movingForward: false,
            player: JSON.parse(JSON.stringify(playerDefault)),
            currentEnemy: {},
            // quests: [],
            // tavernQuests: [],
            // merchant: [],
            dungeonCount: 0,
            // castleCount: 0,
            // meadCount: 0,
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
        z-index: 1;
        overflow: scroll;
    }

    .game-header {
        text-align: center;
    }
</style>
