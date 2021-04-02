<template>
<h5>{{ $parent.message }}</h5>
    <div class="event-display">
        <div class="text-blue">
            <img class="game-sprite"
            :class="{ 'idle': $parent.player.animation == 'idle',
            'walk': $parent.player.animation == 'walk' ,
            'dodge': $parent.player.animation == 'dodge' ,
            'die-left': $parent.player.animation == 'die' ,
            'damage': $parent.player.animation == 'damage' ,
            'attack-right': $parent.player.animation == 'attack' }"
            :src="$parent.player.sprite"
            :alt="$parent.player.name">
            <p>
                {{ $parent.player.name }} | 
                <span class="text-blue">HP: {{ $parent.player.hp }}/{{ $parent.player.hpMax }}</span> |  
                <span class="text-blue">MP: {{ $parent.player.mp }}/{{ $parent.player.mpMax }}</span>
            </p>
            <p>
                <span class="grey-text">XP: {{ $parent.player.xp }}/{{ $parent.player.nextLevel }}</span> | 
                <span class="red-text">{{ $parent.player.gold }}g</span>
            </p>
        </div>
        <div>
            <!-- <img class="game-sprite"
            :class="{ 'idle': $parent.currentEnemy.animation == 'idle',
            'dodge': $parent.currentEnemy.animation == 'dodge' ,
            'die-right': $parent.currentEnemy.animation == 'die' ,
            'damage': $parent.currentEnemy.animation == 'damage' ,
            'attack-left': $parent.currentEnemy.animation == 'attack' }"
            :src="$parent.currentEnemy.sprite"
            :alt="$parent.currentEnemy.name">
            <p class="text-green">
                {{ $parent.currentEnemy.name }}
                <span class="white-text"> | </span>
                <span class="">HP: {{ $parent.currentEnemy.hp }}/{{ $parent.currentEnemy.hpMax }}</span>
                <span class="white-text"> | </span>
                <span class="">MP: {{ $parent.currentEnemy.mp }}/{{ $parent.currentEnemy.mpMax }}</span>
            </p> -->
        </div>
    </div>
    <template v-if="task == 'wild'">
        <p>Where to next?</p>
        <p>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn}" @click="handleExplore">Explore</button>
        </p>
        <p>
            <button class="btn-blue disabled" :class="{ 'disabled' : !playerTurn}">Go to Town</button>
        </p>
        <p>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn}" @click="handleUseItem">Use Item</button>
        </p>
        <p>
            <button class="btn-blue disabled" :class="{ 'disabled' : !playerTurn}">To Forest<i className="material-icons">arrow_forward</i></button>
        </p>
    </template>
    <template v-else-if="task == 'use item'">
        <p>Select an Item</p>
        <p class="btn-blue"
        v-for="(item, index) in $parent.player.inventory"
        :key="index">
            <button class="btn-blue"
            :class="{ 'disabled' : !playerTurn}"
            @click="handleAttemptItem(item,index)">{{ item.name }} x {{item.qty}}</button>
        </p>
        <p>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn}" @click="handleBack"><i class="material-icons left">arrow_back</i>Back</button>
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
        handleBack() {
            this.task = 'wild';
        },
        handleUseItem() {
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
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>

</style>
