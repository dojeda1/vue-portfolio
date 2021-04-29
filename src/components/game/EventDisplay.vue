<template>
    <div class="event-display">
        <div class="left-display">
            <div class="sprite-container">
                <span class="note"
                :class="{ 'item-rise': $parent.player.note != ''}"
                >{{$parent.player.note}}</span>
                <img class="item-sprite"
                :class="{ 'item-rise': $parent.player.itemSprite != ''}"
                :src="$parent.player.itemSprite" alt="Item Sprite">
                <img class="ability-sprite"
                :class="{ 'ability-right': $parent.player.abilitySprite != ''}"
                :src="$parent.player.abilitySprite" alt="Ability Sprite">
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
                <!-- <p>
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
                    MP: {{ $parent.player.mp }}/{{ $parent.player.mpMax }}</span>
                </p>
                <p>
                    <span >XP: {{ $parent.player.xp }}/{{ $parent.player.nextLevel }}</span> &middot; 
                    <span 
                    :class="{'text-gold': $parent.player.gold < 20,
                    'text-red': $parent.player.gold < 10,
                    'text-gray': $parent.player.hp <= 0}">
                    {{ $parent.player.gold }}g</span>
                </p> -->
                <p>
                    {{ $parent.player.name }} &middot; 
                    <span 
                    :class="{'text-gold': $parent.player.gold < 20,
                    'text-red': $parent.player.gold < 10,
                    'text-gray': $parent.player.hp <= 0}">
                    {{ $parent.player.gold }}g</span>
                </p>
                <div class="bars">
                    <div class="bar">
                        <div class="inner-bar bg-red"
                        :class="{'bg-gray' : $parent.player.hp <= 0}"
                        :style="{'width': hpBar($parent.player)}">
                        </div>
                        <!-- <div class="bar-text">{{ $parent.player.hp }}</div> -->
                    </div>
                    <div class="bar">
                        <div class="inner-bar bg-blue"
                        :class="{'bg-gray' : $parent.player.hp <= 0}"
                        :style="{'width': mpBar($parent.player)}">
                        </div>
                        <!-- <div class="bar-text">{{ $parent.player.mp }}</div> -->
                    </div>
                    <div class="bar">
                        <div class="inner-bar bg-green"
                        :class="{'bg-gray' : $parent.player.hp <= 0}"
                        :style="{'width': xpBar($parent.player)}">
                        </div>
                        <!-- <div class="bar-text">{{ $parent.player.xp }}</div> -->
                    </div>
                </div>
                <p class="text-gold"
                :class="{'text-gray': $parent.player.hp <= 0}">
                    {{statusList($parent.player)}}
                </p>
            </div>
        </div>
        <div class="right-display">
            <template v-if="$parent.scene == 'Battle'">
                <div class="sprite-container" :class="{'enter-left': $parent.newEvent}">
                    <span class="note"
                    :class="{ 'item-rise': $parent.currentEnemy.note != ''}"
                    >{{$parent.currentEnemy.note}}</span>
                    <img class="item-sprite"
                    :class="{ 'item-rise': $parent.currentEnemy.itemSprite != ''}"
                    :src="$parent.currentEnemy.itemSprite" alt="Item Sprite">
                    <img class="ability-sprite"
                    :class="{ 'ability-left': $parent.currentEnemy.abilitySprite != ''}"
                    :src="$parent.currentEnemy.abilitySprite" alt="Ability Sprite">
                    <img class="game-sprite"
                    :class="{ 'idle': $parent.currentEnemy.animation == 'idle',
                    'use-item': $parent.currentEnemy.animation == 'use item' ,
                    'walk': $parent.currentEnemy.animation == 'walk' ,
                    'jump': $parent.currentEnemy.animation == 'jump' ,
                    'dodge': $parent.currentEnemy.animation == 'dodge' ,
                    'buffer': $parent.currentEnemy.animation == 'buffer' ,
                    'die-right': $parent.currentEnemy.animation == 'die' ,
                    'damage': $parent.currentEnemy.animation == 'damage' ,
                    'attack-left': $parent.currentEnemy.animation == 'attack' }"
                    :src="$parent.currentEnemy.sprite"
                    :alt="$parent.currentEnemy.name">
                </div>
                <div class="display-text"
                :class="{'text-green': $parent.currentEnemy.type == 'common' || $parent.currentEnemy.type == 'vicious',
                'text-jade': $parent.currentEnemy.type == 'boss' || $parent.currentEnemy.type == 'endBoss',
                'text-purple': $parent.currentEnemy.type == 'finalBoss',
                'text-gray': $parent.currentEnemy.hp <= 0}">
                    <p>
                        {{ $parent.currentEnemy.name }}
                    </p>
                    <div class="bars">
                        <div class="bar">
                            <div class="inner-bar bg-red"
                            :class="{'bg-gray' : $parent.currentEnemy.hp <= 0}"
                            :style="{'width': hpBar($parent.currentEnemy)}">
                            </div>
                        </div>
                        <div class="bar" v-if="$parent.currentEnemy.mpMax > 0">
                            <div class="inner-bar bg-blue"
                            :class="{'bg-gray' : $parent.currentEnemy.hp <= 0}"
                            :style="{'width': mpBar($parent.currentEnemy)}">
                            </div>
                        </div>
                    </div>
                    <p class="text-gold"
                    :class="{'text-gray': $parent.currentEnemy.hp <= 0}">
                        {{statusList($parent.currentEnemy)}}
                    </p>
                </div>
            </template>
            <template v-else-if="$parent.scene == 'ChestEncounter'
            || $parent.scene == 'Quests'
            || $parent.scene == 'Shop'
            || $parent.scene == 'Tavern'
            || $parent.scene == 'WordGuess'
            || $parent.scene == 'DungeonEncounter'
            || $parent.scene == 'AdventurerEncounter'">
                <div class="sprite-container" :class="{'enter-left': $parent.newEvent}">
                    <span class="note"
                    :class="{ 'item-rise': $parent.currentEncounter.note != ''}"
                    >{{$parent.currentEncounter.note}}</span>
                    <img class="item-sprite"
                    :class="{ 'item-rise': $parent.currentEncounter.animation == 'use item'}"
                    :src="$parent.currentEncounter.itemSprite" :alt="$parent.currentEncounter.itemName">
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
                </div>
                <div class="display-text text-blue">
                    <p>{{ $parent.currentEncounter.name }}</p>
                </div>
            </template>
        </div>
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
        statusList(character) {
            let list = [];
            for (const effect in character.status) {
                if (character.status[effect] > 0) {
                    // list.push(`${effect}: ${character.status[effect]}`)
                    list.push(effect)
                }
            }
            return list.join(', ')
        },
        hpBar(character) {
            var width = (character.hp/character.hpMax)*100;
            if (width > 100) {
                width = 100
            } else if(width < 0) {
                width = 0
            }
            return width + '%'
        },
        mpBar(character) {
            var width = (character.mp/character.mpMax)*100;
            if (width > 100) {
                width = 100
            } else if(width < 0) {
                width = 0
            }
            return width + '%'
        },
        xpBar(character) {
            var width = ((character.xp - character.lastLevel)/
            (character.nextLevel - character.lastLevel))*100;
            if (width > 100) {
                width = 100
            } else if(width < 0) {
                width = 0
            }
            return width + '%'
        }
    },
    created: function() {
        this.$parent.messageBox = [];
        this.$parent.player.animation = 'idle'
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
    .bars {
        max-width: 128px;
        width: 100%;
        margin: auto;
    }
    .bar {
        position: relative;
        width: 100%;
        min-height: 2px;
        margin: 5px 0;
        background-color: #454545;
    }
    .bar .bar-text {
        position: relative;
        color:white;
        font-size: 10px;
        line-height: 1;
        padding-top: 1px;
    }
    .bar .inner-bar {
        position: absolute;
        left: 0;
        top: 0;
        height: 100%;
        transition: width .5s;
    }
</style>
