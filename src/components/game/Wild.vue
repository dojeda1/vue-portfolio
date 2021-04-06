<template>
    <template v-if="task == 'wild'">
        <p>Where to next?</p>
        <p>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn}" @click="handleExplore">Explore</button>
        </p>
        <p>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn}" @click="handleTown">Go to Town</button>
        </p>
        <p>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn}" @click="handleUseItem">Use Item</button>
        </p>
        <p>
            <button class="btn-blue disabled" :class="{ 'disabled' : !playerTurn}">To Forest<i className="material-icons">arrow_forward</i></button>
        </p>
    </template>
    <template v-else-if="task == 'use item'">
        <p>{{$parent.infoText}}</p>
        <div class="items" @mouseleave="$parent.infoText = 'Select an Item'">
            <button class="btn-blue"
            v-for="(item, index) in $parent.player.inventory"
            :key="index"
                :class="{ 'disabled' : !playerTurn}"
                @mouseover="$parent.infoText = item.info"
                @click="handleAttemptItem(item,index)">{{ item.name }} x {{item.qty}}</button>
        </div>
        <p>
            <button class="btn-inv" :class="{ 'disabled' : !playerTurn}" @click="handleBack"><i class="material-icons left">arrow_back</i>Back</button>
        </p>
    </template>
</template>

<script>
export default {
    name: 'Wild',
    data() {
        return {
            task: 'wild',
            playerTurn: true
        }
    },
    methods: {
        log(msg) {
            console.log('Log:',msg);
        },
        handleExplore() {
            const exploreCheck = this.$parent.randNum(1, 10)
            if (exploreCheck == 1) {
                this.chestEncounter();
            } else if (exploreCheck == 2) {
                this.dungeonEncounter();
            // } else if (exploreCheck === 300) {
            //     this.viciousEncounter();
            } else {
                this.monsterEncounter();
            }
        },
        handleTown() {
            this.$parent.message = "You traveled to town."
            var $this = this
            $this.playerTurn = false
            $this.$parent.player.animation = 'walk'
            setTimeout(function() {
                $this.$parent.player.animation = 'idle'
                $this.playerTurn = true
                $this.$parent.changeScene('Town');
            },900)
        },
        handleBack() {
            this.task = 'wild';
        },
        handleUseItem() {
            if (this.$parent.player.inventory.length) {
                this.$parent.infoText = 'Select an item'
            } else {
                this.$parent.infoText = 'Inventory empty'
            }
            this.task = 'use item';
        },
        handleAttemptItem(item,index) {
            let $this = this;
            let player = this.$parent.player;
            // let enemy = this.$parent.currentEnemy;
            this.$parent.handleAttemptItem(item,index,
                function() {
                    $this.playerTurn = false;
                    setTimeout(function() {
                        $this.playerTurn = true;
                        player.animation = 'idle'
                    }, 600)
                }
            );
        },
        chestEncounter() {
            this.$parent.currentEncounter = JSON.parse(JSON.stringify(this.$parent.encounters[0]));
            this.$parent.message = "You found a chest!"
            this.$parent.changeScene('ChestEncounter');
        },
        dungeonEncounter() {
            this.$parent.currentEncounter = JSON.parse(JSON.stringify(this.$parent.encounters[2]));
            this.$parent.message = "You discovered a dungeon!"
            this.$parent.changeScene('DungeonEncounter');
        },
        monsterEncounter(alternateMessage) {

            let rangeNum = 0;
            let playerLevel = this.$parent.player.level;

            const regionIndex = this.$parent.region.index;
            console.log("RI:" + regionIndex)
            const regionLevel = this.$parent.region.level;
            const regionTarget = this.$parent.region.targetLevel;

            let monsterArray;

            switch (regionIndex) {
                case 1:
                    monsterArray = this.$parent.monsters1;
                        break;
                    case 2:
                        monsterArray = this.$parent.monsters2;
                        break;
                    case 3:
                        monsterArray = this.$parent.monsters3;
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
            let monNum = this.$parent.randNum(0, rangeNum);
            const message = alternateMessage || "You encountered " + this.$parent.anA(monsterArray[monNum].name) + " " + monsterArray[monNum].name + ".";
            this.$parent.message = message;
            console.log("message: " + message);
            this.$parent.currentEnemy = JSON.parse(JSON.stringify(monsterArray[monNum]));
            this.$parent.currentEnemy.hp = monsterArray[monNum].hpMax;
            this.$parent.currentEnemy.mp = monsterArray[monNum].mpMax;
            this.$parent.currentEnemy.animation = 'idle';
            this.$parent.currentEnemy.isDead = false

            this.addEnemyItems(this.$parent.currentEnemy);

            // console.log(this.state.currentEnemy);
            this.$parent.changeScene('Battle')

            console.log('Enemy:',this.$parent.currentEnemy);
        },
        addEnemyItems(enemy) {
            let randItem = this.$parent.randNum(0, this.$parent.items3.length);
            this.$parent.addItem(enemy.inventory, this.$parent.items3[randItem]);
            this.$parent.addItem(enemy.inventory, this.$parent.items1[0]);
            this.$parent.addItem(enemy.inventory, this.$parent.items1[1]);
        },
    },
    created: function() {
        this.$parent.location = 'Wild';
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>

</style>
