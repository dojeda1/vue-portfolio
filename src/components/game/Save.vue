<template>
    <p>
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
    <div class="message-box">
        <h5>{{ $parent.message }}</h5>
        <p v-for="(msg, index) in $parent.messageBox" :key="index">
            {{ msg }}
        </p>
    </div>
    <p v-if="$parent.location == 'Town'">Save your Progress</p>
    <p v-else>Visit Town to Save</p>
    <p>
        <button :class="{ 'disabled' : !playerTurn || $parent.location != 'Town'}" @click="handleSave">Save Game</button>
        <button :class="{ 'disabled' : !playerTurn }" @click="handleLoad">Load Game</button>
    </p>
    <p>
        <button class="btn-inv" :class="{ 'disabled' : !playerTurn }" @click="handleBack"><i class="material-icons left">arrow_back</i>Back</button>
    </p>
</template>

<script>
export default {
    name: 'Save',
    data() {
        return {
            player: {},
            playerTurn: true
        }
    },
    methods: {
        log(msg) {
            console.log('Log:',msg);
        },
        handleSave() {
            console.log('Game Saved')
            this.$parent.message = 'Your game has been Saved.'
            localStorage.setItem("player", JSON.stringify(this.$parent.player));
            localStorage.setItem("regions", JSON.stringify(this.$parent.regions));
            localStorage.setItem("region", JSON.stringify(this.$parent.region));
            // localStorage.setItem("bosses", JSON.stringify(this.$parent.bosses));
        },
        handleLoad() {
            this.parent.loadGame();
        },
        handleBack() {
            this.$parent.$parent.menu = false;
        },
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>

</style>
