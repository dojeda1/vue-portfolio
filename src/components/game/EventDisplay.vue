<template>
    <div class="event-display">
        <div class="left-display">
            <div class="sprite-container">
            <img class="item-sprite"
            :class="{ 'item-rise': $parent.player.animation == 'use item'}"
            :src="$parent.player.itemSprite" :alt="$parent.player.itemName">
            <img class="game-sprite"
            :class="{ 'idle': $parent.player.animation == 'idle',
            'use-item': $parent.player.animation == 'use item' ,
            'walk': $parent.player.animation == 'walk' ,
            'jump': $parent.player.animation == 'jump' ,
            'dodge': $parent.player.animation == 'dodge' ,
            'buffer': $parent.player.animation == 'buffer' ,
            'die-left': $parent.player.animation == 'die' ,
            'damage': $parent.player.animation == 'damage' ,
            'attack-right': $parent.player.animation == 'attack' }"
            :src="$parent.player.sprite"
            :alt="$parent.player.name">
            </div>
            <div class="display-text text-blue" :class="{'text-gray': $parent.player.hp <= 0}">
                <p>
                    {{ $parent.player.name }} | 
                    <span 
                    :class="{'text-red': $parent.player.hp < $parent.player.hpMax/2,
                    'text-gray': $parent.player.hp <= 0}">
                    HP: {{ $parent.player.hp }}/{{ $parent.player.hpMax }}</span> |  
                    <span 
                    :class="{'text-red': $parent.player.mp < $parent.player.mpMax/2,
                    'text-gray': $parent.player.hp <= 0}">
                    MP: {{ $parent.player.mp }}/{{ $parent.player.mpMax }}</span>
                </p>
                <p>
                    <span >XP: {{ $parent.player.xp }}/{{ $parent.player.nextLevel }}</span> | 
                    <span 
                    :class="{'text-red': $parent.player.gold < 10,
                    'text-gray': $parent.player.hp <= 0}">
                    {{ $parent.player.gold }}g</span>
                </p>
                <p>{{$parent.player.status.join()}}</p>
            </div>
        </div>
        <div class="right-display">
            <template v-if="$parent.scene == 'Battle'">
                <img class="game-sprite"
                :class="{ 'idle': $parent.currentEnemy.animation == 'idle',
                'walk': $parent.currentEnemy.animation == 'use-item' ,
                'walk': $parent.currentEnemy.animation == 'walk' ,
                'jump': $parent.currentEnemy.animation == 'jump' ,
                'dodge': $parent.currentEnemy.animation == 'dodge' ,
                'buffer': $parent.currentEnemy.animation == 'buffer' ,
                'die-right': $parent.currentEnemy.animation == 'die' ,
                'damage': $parent.currentEnemy.animation == 'damage' ,
                'attack-left': $parent.currentEnemy.animation == 'attack' }"
                :src="$parent.currentEnemy.sprite"
                :alt="$parent.currentEnemy.name">
                <div class="display-text"
                :class="{'text-green': $parent.currentEnemy.type == 'common' || $parent.currentEnemy.type == 'vicious',
                'text-jade': $parent.currentEnemy.type == 'boss',
                'text-gray': $parent.currentEnemy.hp <= 0}">
                    <p>
                        {{ $parent.currentEnemy.name }} | 
                        <span 
                        :class="{'text-red': $parent.currentEnemy.hp < $parent.currentEnemy.hpMax/2,
                        'text-gray': $parent.currentEnemy.hp <= 0}">
                        HP: {{ $parent.currentEnemy.hp }}/{{ $parent.currentEnemy.hpMax }}</span> | 
                        <span 
                        :class="{'text-red': $parent.currentEnemy.mp < $parent.currentEnemy.mpMax/2,
                        'text-gray': $parent.currentEnemy.hp <= 0}">MP: {{ $parent.currentEnemy.mp }}/{{ $parent.currentEnemy.mpMax }}</span>
                    </p>
                </div>
            </template>
            <template v-else-if="$parent.scene == 'ChestEncounter'
            || $parent.scene == 'DungeonEncounter'">
                <img class="game-sprite"
                :class="{ 'idle': $parent.currentEncounter.animation == 'idle',
                'walk': $parent.currentEncounter.animation == 'walk' ,
                'jump': $parent.currentEncounter.animation == 'jump' ,
                'dodge': $parent.currentEncounter.animation == 'dodge' ,
                'buffer': $parent.currentEncounter.animation == 'buffer' ,
                'die-right': $parent.currentEncounter.animation == 'die' ,
                'damage': $parent.currentEncounter.animation == 'damage' ,
                'attack-left': $parent.currentEncounter.animation == 'attack' }"
                :src="$parent.currentEncounter.sprite"
                :alt="$parent.currentEncounter.name">
                <div class="display-text text-blue">
                    <p>{{ $parent.currentEncounter.name }}</p>
                </div>
            </template>
        </div>
    </div>
    <div class="message-box">
        <h5>{{ $parent.message }}</h5>
        <p v-for="(msg, index) in $parent.messageBox" :key="index">
            {{ msg }}
        </p>
    </div>
</template>

<script>
export default {
    name: 'Event Display',
    data() {
        return {
            x: true
        }
    },
    methods: {
        log(msg) {
            console.log('Log:',msg);
        },
    },
    created: function() {
        this.$parent.messageBox = [];
        this.$parent.player.animation = 'idle'
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>

</style>
