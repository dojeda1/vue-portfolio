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
                console.log('Add Items 0')
                const itemNum = this.$parent.randNum(0, this.$parent.items[0].length)
                const item = this.$parent.items[0][itemNum];
                this.$parent.addItem(player.inventory, item);
                text.push("You got " + this.$parent.anA(item.name) + " " + item.name + ".");
            }
            for (let i = 0; i < this.$parent.randNum(0, 2); i++) {
                console.log('Add Items 1')
                const itemNum = this.$parent.randNum(0, this.$parent.items[1].length)
                const item = this.$parent.items[1][itemNum];
                this.$parent.addItem(player.inventory, item);
                text.push("You got " + this.$parent.anA(item.name) + " " + item.name + ".");
            }
            for (let i = 0; i < this.$parent.randNum(0, 2); i++) {
                console.log('Add Items 2')
                const itemNum = this.$parent.randNum(0, this.$parent.items[2].length)
                const item = this.$parent.items[2][itemNum];
                this.$parent.addItem(player.inventory, item);
                text.push("You got " + this.$parent.anA(item.name) + " " + item.name + ".");
            }
            for (let i = 0; i < this.$parent.randNum(0, 1); i++) {
                console.log('Add Items 3')
                const itemNum = this.$parent.randNum(0, this.$parent.items[3].length)
                const item = this.$parent.items[3][itemNum];
                this.$parent.addItem(player.inventory, item);
                text.push("You got " + this.$parent.anA(item.name) + " " + item.name + ".");
            }
            if (this.$parent.randNum(1, 4) == 1) {
                console.log('Add Items 4')
                const itemNum = this.$parent.randNum(0, this.$parent.items[4].length)
                const item = this.$parent.items[4][itemNum];
                this.$parent.addItem(player.inventory, item);
                text.push("You got " + this.$parent.anA(item.name) + " " + item.name + ".");
            }
            if (this.$parent.randNum(1, 50) == 1) {
                console.log('Add Items 5')
                const itemNum = this.$parent.randNum(0, this.$parent.items[5].length)
                const item = this.$parent.items[5][itemNum];
                this.$parent.addItem(player.inventory, item);
                text.push("You got " + this.$parent.anA(item.name) + " " + item.name + ".");
            }
        },
        mimicEncounter(alternateMessage) {
            // let rangeNum = 0;
            // let playerLevel = this.$parent.player.level;

            const regionIndex = this.$parent.region;
            // console.log("RI:" + regionIndex)
            const regionLevel = this.$parent.regions[regionIndex].level;
            // const regionTarget = this.$parent.region.targetLevel;

            this.$parent.currentEnemy = JSON.parse(JSON.stringify(this.$parent.encounters[1]));
            const enemy = this.$parent.currentEnemy;
            const message = alternateMessage || "You were tricked by " + this.$parent.anA(enemy.name) + " " + enemy.name + ".";
            this.$parent.message = message;
            console.log("message: " + message);
            enemy.note = '';
            enemy.animation = 'idle';
            enemy.isDead = false

            enemy.hpMax+=(regionLevel * 5);
            enemy.hp = enemy.hpMax;
            enemy.mp = enemy.mpMax;
            enemy.strength+=(regionLevel * 2);
            enemy.defense+=(regionLevel * 2);
            enemy.speed+=regionLevel;
            enemy.luck+=(regionLevel * 2);
            enemy.xp+=(regionLevel * 5);
            enemy.inventory = [];
            enemy.gold+=(regionLevel * 5);

            this.$parent.addEnemyItems(enemy);

            this.$parent.changeScene('Battle')

            console.log('Enemy:',this.$parent.currentEnemy);
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
