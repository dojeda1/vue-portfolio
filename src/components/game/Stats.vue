<template>
    <p class="screen-top">
        <img class="profile-sprite" :src="$parent.player.sprite" :alt="$parent.player.name">
        {{ $parent.player.name }} &middot; 
        <span 
        :class="{'text-gold': $parent.player.hp < $parent.player.hpMax/2,
        'text-red': $parent.player.hp < $parent.player.hpMax/4,
        'text-gray': $parent.player.hp <= 0}">
        HP: {{ $parent.player.hp }}/{{ $parent.player.hpMax }}</span> &middot;  
        <span 
        :class="{'text-gold': $parent.player.mp < $parent.player.mpMax/2,
        'text-red': $parent.player.mp < $parent.player.mpMax/4,
        'text-gray': $parent.player.hp <= 0}">
        MP: {{ $parent.player.mp }}/{{ $parent.player.mpMax }}</span> &middot; 
        <span 
        :class="{'text-gold': $parent.player.gold < 20,
        'text-red': $parent.player.gold < 10,
        'text-gray': $parent.player.hp <= 0}">
        {{ $parent.player.gold }}g</span>
    </p>
    <div class="screen-bottom">
        <div class="message-box">
            <h5>Stats</h5>
        </div>
        <div class="stats">
            <div class="stat">
                <h5>Player Stats</h5>
                <p>
                    {{$parent.player.name}},
                    Lv. {{$parent.player.level}} &middot;
                    {{$parent.player.race}} &middot;
                    {{$parent.player.class}}
                </p>
                <p>
                    HP: {{$parent.player.hp}}/{{$parent.player.hpMax}} &middot;
                    MP: {{$parent.player.mp}}/{{$parent.player.mpMax}} &middot;
                    XP: {{$parent.player.xp}}/{{$parent.player.nextLevel}} &middot;
                    {{$parent.player.gold}}g
                </p>
                <p>
                    STR: {{$parent.player.strength}} &middot;
                    DEF: {{$parent.player.defense}} &middot;
                    WILL: {{$parent.player.will}} &middot;
                    LUCK: {{$parent.player.luck}} &middot;
                    SPD: {{$parent.player.speed}}
                </p>
            </div>
            <div class="stat">
                <h5>Abilities</h5>
                <template v-for="(move, index) in $parent.player.abilities"
                :key="index">
                    <p v-if="move.active">{{ move.name }} - {{move.cost}}mp</p>
                </template>
            </div>
            <div class="stat">
                <h5>Inventory</h5>
                <div class="items">
                    <p v-for="(item, index) in $parent.player.inventory"
                    :key="index">{{ item.name }} &times; {{item.qty}}</p>
                </div>
            </div>
            <div class="stat">
                <h5>Game Stats</h5>
                <p>Monsters Killed: {{$parent.player.totalKills}}</p>
                <p>Gold Collected: {{$parent.player.totalGold}}</p>
                <p>Quests Completed: {{$parent.player.totalQuests}}</p>
                <p>Dungeons Completed: {{$parent.player.totalDungeons}}</p>
            </div>
            <div class="title">
                <h5 :class="{'text-blue': completionNum >= 100}">Completion: {{completionNum}}%</h5>
            </div>
            <div class="region" v-for="(region, index) in $parent.regions"
                :key="index">
                    <h5 v-if="region.discovered">{{ region.name }}</h5>
                    <h5 v-else>???</h5>
                    <p :class="{'text-blue': region.endBossKills >= 1}">End Bosses Defeated: {{region.endBossKills}}/1</p>
                    <p :class="{'text-blue': region.bossKills >= $parent.bosses[index].length}">Dungeon Bosses Killed: {{region.bossKills}}/{{$parent.bosses[index].length}}</p>
            </div>
        </div>
        <p class="dual-buttons">
            <button class="btn-inv" :class="{ 'disabled' : !playerTurn}" @click="handleBack"><i class="material-icons left">arrow_back</i>Back</button>
        </p>
    </div>
</template>

<script>
export default {
    name: 'Stats',
    data() {
        return {
            task: 'questBoard',
            playerTurn: true,
            completionNum: 0
        }
    },
    methods: {
        log(msg) {
            console.log('Log:',msg);
        },
        handleBack() {
            this.$parent.$parent.menu = false;
        },
    },
    created: function() {
        console.log('Stats Opened')
        this.completionNum = Math.floor(
            ((this.$parent.regions[0].endBossKills + 
            this.$parent.regions[0].bossKills + 
            this.$parent.regions[1].endBossKills + 
            this.$parent.regions[1].bossKills + 
            this.$parent.regions[2].endBossKills + 
            this.$parent.regions[2].bossKills)
            /
            (this.$parent.endBosses.length +
            this.$parent.bosses[0].length +
            this.$parent.bosses[1].length +
            this.$parent.bosses[2].length))
            * 100)
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
    .stats {
        display: grid;
        grid-gap: 10px;
        margin-top: 10px;
        grid-template-columns: repeat(6,1fr);
    }
    .stat {
        border: 2px solid gray;
        border-radius: 10px;
        padding: 10px;
        grid-column: span 3;
    }
    .region {
        border: 2px solid gray;
        border-radius: 10px;
        padding: 10px;
        grid-column: span 2;
    }
    .title {
        border: 2px solid gray;
        border-radius: 10px;
        padding: 10px;
        grid-column: span 6;
    }
    @media screen and (max-width: 600px) {
        .stat,.region,.title {
            grid-column: span 6;
        }
    }
</style>
