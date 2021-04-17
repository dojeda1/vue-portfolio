<template>
    <template v-if="task == 'chest'">
        <p>What next?</p>
        <p>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn}" @click="handleOpenChest">Open Chest</button>
        </p>
        <p>
            <button class="btn-inv" :class="{ 'disabled' : !playerTurn}" @click="handleBack"><i class="material-icons left">arrow_back</i>Leave</button>
        </p>
    </template>
    <template v-else-if="task == 'end'">
        <p>
            <button class="btn-inv" @click="handleEnd">End</button>
        </p>
    </template>
</template>

<script>
export default {
    name: 'ChestEncounter',
    data() {
        return {
            task: 'chest',
            playerTurn: true
        }
    },
    methods: {
        log(msg) {
            console.log('Log:',msg);
        },
        handleOpenChest() {
            const chestCheck = this.$parent.randNum(1, 5)
            if (chestCheck == 1) {
                this.mimicEncounter();
            } else {
                this.openChest();
            }
        },
        handleBack() {
            this.$parent.message = "You left the chest behind."
            if (this.$parent.location == 'Wild') {
                this.$parent.changeScene('Wild');
            } else if (this.$parent.location == 'Dungeon') {
                this.$parent.changeScene('Dungeon');
            } else if (this.$parent.location == 'Castle') {
                this.$parent.changeScene('Castle');
            }
        },
        handleEnd() {
            this.$parent.messageBox = [];
            if (this.$parent.location == 'Wild') {
                this.$parent.changeScene('Wild');
            } else if (this.$parent.location == 'Dungeon') {
                this.$parent.changeScene('Dungeon');
            } else if (this.$parent.location == 'Castle') {
                this.$parent.changeScene('Castle');
            }
        },
        openChest() {
            this.$parent.currentEncounter.animation = 'buffer'
            this.$parent.currentEncounter.sprite = '/images/game/chest-open.png'
            this.$parent.message = "You opened the chest."
            this.task = 'end'
            const player = this.$parent.player;
            const goldNum = this.$parent.randNum(10, 30);
            player.gold += goldNum;
            player.totalGold += goldNum;

            const text = this.$parent.messageBox;
            text.push("--- RESULTS ---");
            text.push("Chest contained " + goldNum + " gold.");

            for (let i = 0; i < 2; i++) {
                const itemNum = this.$parent.randNum(0, this.$parent.items[0].length)
                const item = this.$parent.items[0][itemNum];
                this.$parent.addItem(player.inventory, item);
                text.push("You got " + this.$parent.anA(item.name) + " " + item.name + ".");
            }
            let uncommonCheck = this.$parent.randNum(0, 2);
            for (let i = 0; i < uncommonCheck; i++) {
                const itemNum = this.$parent.randNum(0, this.$parent.items[1].length)
                const item = this.$parent.items[1][itemNum];
                this.$parent.addItem(player.inventory, item);
                text.push("You got " + this.$parent.anA(item.name) + " " + item.name + ".");
            }
            // if (this.$parent.location === "Dungeon") {
            //     this.$parent.dungeonCount++
            //     if (this.$parent.dungeonCount >= this.$parent.region.dungeonGoal + 1) {
            //         player.totalDungeons++
            //     }
            // }
        },
        mimicEncounter(alternateMessage) {
            // let rangeNum = 0;
            // let playerLevel = this.$parent.player.level;

            // const regionIndex = this.$parent.region.index;
            // console.log("RI:" + regionIndex)
            const regionLevel = this.$parent.region.level;
            // const regionTarget = this.$parent.region.targetLevel;

            let mimic = this.$parent.encounters[1];
            const message = alternateMessage || "You were tricked by " + this.$parent.anA(mimic.name) + " " + mimic.name + ".";
            this.$parent.message = message;
            console.log("message: " + message);
            this.$parent.currentEnemy = JSON.parse(JSON.stringify(mimic));
            // this.$parent.currentEnemy.hp = mimic.hpMax;
            // this.$parent.currentEnemy.mp = mimic.mpMax;
            this.$parent.currentEnemy.note = '';
            this.$parent.currentEnemy.animation = 'idle';
            this.$parent.currentEnemy.isDead = false

            this.$parent.currentEnemy.hpMax = (regionLevel * 5) + mimic.hpMax;
            this.$parent.currentEnemy.hp = (regionLevel * 5) + mimic.hpMax;
            this.$parent.currentEnemy.mp = mimic.mpMax;
            this.$parent.currentEnemy.strength = regionLevel * 2 + mimic.strength;
            this.$parent.currentEnemy.defense = regionLevel * 2 + mimic.defense;
            this.$parent.currentEnemy.speed = regionLevel + mimic.speed;
            this.$parent.currentEnemy.luck = regionLevel * 2 + mimic.luck;
            this.$parent.currentEnemy.xp = regionLevel * 5 + mimic.xp;
            this.$parent.currentEnemy.inventory = [];
            this.$parent.currentEnemy.gold = 60 + regionLevel * 5;

            this.addEnemyItems(this.$parent.currentEnemy);

            this.$parent.changeScene('Battle')

            console.log('Enemy:',this.$parent.currentEnemy);
        },
        addEnemyItems(enemy) {
            let randItem = this.$parent.randNum(0, this.$parent.items[2].length);
            this.$parent.addItem(enemy.inventory, this.$parent.items[2][randItem]);
            this.$parent.addItem(enemy.inventory, this.$parent.items[0][0]);
            this.$parent.addItem(enemy.inventory, this.$parent.items[0][1]);
        },
    },
    created: function() {
        // this.$parent.location = 'Wild';
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>

</style>
