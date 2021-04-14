<template>
    <div class="message-box">
        <h5>Stats</h5>
    </div>
    <div class="stats">
        <div class="stat">
            <h5>Player Stats</h5>
            <p>
                {{$parent.player.name}},
                Lv. {{$parent.player.level}}
                {{$parent.player.race}}
                {{$parent.player.class}}
            </p>
            <p>
                HP: {{$parent.player.hp}}/{{$parent.player.hpMax}} |
                MP: {{$parent.player.mp}}/{{$parent.player.mpMax}} |
                XP: {{$parent.player.xp}}/{{$parent.player.nextLevel}} |
                {{$parent.player.gold}}g
            </p>
            <p>
                STR: {{$parent.player.strength}} |
                DEF: {{$parent.player.defense}} |
                MANA: {{$parent.player.mana}} |
                LUCK: {{$parent.player.luck}} |
                SPD: {{$parent.player.speed}}
            </p>
        </div>
        <div class="stat">
            <h5>Special Moves</h5>
            <template v-for="(move, index) in $parent.player.specials"
            :key="index">
                <p v-if="move.active">{{ move.name }} - {{move.cost}}mp</p>
            </template>
        </div>
        <div class="stat">
            <h5>Inventory</h5>
            <div class="items">
                <p v-for="(item, index) in $parent.player.inventory"
                :key="index">{{ item.name }} x {{item.qty}}</p>
            </div>
        </div>
        <div class="stat">
            <h5>Game Stats</h5>
            <p>Monsters Killed: {{$parent.player.totalKills}}</p>
            <p>Gold Collected: {{$parent.player.totalGold}}</p>
            <p>Quests Completed: {{$parent.player.totalQuests}}</p>
            <p>Dungeons Completed: {{$parent.player.totalDungeons}}</p>
        </div>
    </div>
    <p>
        <button class="btn-inv" :class="{ 'disabled' : !playerTurn}" @click="handleBack"><i class="material-icons left">arrow_back</i>Back</button>
    </p>
</template>

<script>
export default {
    name: 'Stats',
    data() {
        return {
            task: 'questBoard',
            playerTurn: true
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
        // this.$parent.location = 'Wild';
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
    .stats {
        display: grid;
        grid-gap: 10px;
        margin-top: 10px;
        grid-template-columns: 1fr 1fr;
    }
    .stat {
        border: 2px solid gray;
        border-radius: 10px;
        padding: 10px;
        grid-column: span 1;
    }
    @media screen and (max-width: 600px) {
        .stat {
            grid-column: span 2;
        }
    }
</style>
