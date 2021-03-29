<template>
<h5>{{ $parent.message }}</h5>
    <p class="dom-blue-text">
        <i class="material-icons left">person</i>
        {{ $parent.player.name }}
        <span class="white-text"> | </span>
        <span class="dom-blue-text">HP: {{ $parent.player.hp }}/{{ $parent.player.maxHp }}</span>
        <span class="white-text"> | </span>
        <span class="dom-blue-text">MP: {{ $parent.player.mp }}/{{ $parent.player.maxMp }}</span>
        <span class="white-text"> | </span>
        <span class="grey-text">XP: {{ $parent.player.xp }}/{{ $parent.player.nextLevel }}</span>
        <span class="white-text"> | </span>
        <span class="red-text">{{ $parent.player.gold }}g</span>
    </p>
    <p>Where to next?</p>
    <p>
        <button @click="handleExplore">Explore</button>
    </p>
    <p>
        <button class="disabled">Go to Town</button>
    </p>
    <p>
        <button class="disabled">Use Item</button>
    </p>
    <p>
        <button class="disabled">To Forest<i className="material-icons">arrow_forward</i></button>
    </p>
</template>

<script>
export default {
    name: 'Wild',
    data() {
        return {
            x: true,
        }
    },
    methods: {
        log(msg) {
            console.log('Log:',msg);
        },
        handleExplore() {
            const exploreCheck = this.$parent.randNum(1, 10)
            if (exploreCheck === 100) {
            //     this.chestEncounter();
            // } else if (exploreCheck === 200) {
            //     this.dungeonEncounter();
            // } else if (exploreCheck === 300) {
            //     this.viciousEncounter();
            } else {
                this.monsterEncounter();
            }
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
            this.$parent.currentEnemy.hp = monsterArray[monNum].maxHp;
            this.$parent.currentEnemy.mp = monsterArray[monNum].maxMp;
            this.$parent.currentEnemy.isDead = false

            this.addEnemyItems(this.$parent.currentEnemy);

            // console.log(this.state.currentEnemy);
            this.$parent.scene = 'Battle'

            console.log('Enemy:',this.$parent.currentEnemy);
        },
        addEnemyItems(enemy) {
            let randItem = this.$parent.randNum(0, this.$parent.items3.length);
            this.$parent.addItem(enemy.inventory, this.$parent.items3[randItem]);
            this.$parent.addItem(enemy.inventory, this.$parent.items1[0]);
            this.$parent.addItem(enemy.inventory, this.$parent.items1[1]);
        },
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>

</style>
