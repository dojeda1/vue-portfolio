<template>
    <p>
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
    <template v-if="task == 'save'">
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
            <button class="btn-inv" :class="{ 'disabled' : !playerTurn }" @click="handleExit"><i class="material-icons left">arrow_back</i>Back</button>
        </p>
    </template>
    <template v-if="task == 'profiles'">
        <h5>Select a Character</h5>
        <div class="profiles">
            <div class="profile" v-for="(profile,index) in $parent.profiles"
            :key="index">
                <div>
                    <p>
                        <img class="profile-sprite" :src="profile.player.sprite" :alt="profile.player.name">
                        {{profile.player.name}},
                        Lv. {{profile.player.level}} &middot;
                        {{profile.player.race}} &middot;
                        {{profile.player.class}}
                    </p>
                    <p>
                        HP: {{profile.player.hp}}/{{profile.player.hpMax}} &middot;
                        MP: {{profile.player.mp}}/{{profile.player.mpMax}} &middot;
                        XP: {{profile.player.xp}}/{{profile.player.nextLevel}} &middot;
                        {{profile.player.gold}}g
                    </p>
                    <p>Completion: {{$parent.checkCompletion(profile)}}%</p>
                </div>
                <div class="section-buttons">
                    <button class="btn-inv" @click="handleDelete(index)"><i class="material-icons right">delete</i>Delete</button>
                    <button @click="handleSelectProfile(profile.id)">Continue</button>
                </div>
            </div>
        </div>
        <p>
            <button class="btn-inv" @click="handleBack"><i class="material-icons left">arrow_back</i>Back</button>
        </p>
    </template>
</template>

<script>
export default {
    name: 'Save',
    data() {
        return {
            task: 'save',
            player: {},
            playerTurn: true
        }
    },
    methods: {
        log(msg) {
            console.log('Log:',msg);
        },
        handleSave() {
            this.$parent.saveGame();
        },
        handleLoad() {
            this.task = 'profiles'
        },
        handleDelete(ind) {
            this.$parent.profiles.splice(ind, 1);
            localStorage.setItem("profiles", JSON.stringify(this.$parent.profiles));
            console.log('Profile Deleted')
        },
        handleSelectProfile(id) {
            let profile = this.$parent.getProfileById(id);
            this.$parent.loadGame(profile);
        },
        handleBack() {
            this.task = 'save'
        },
        handleExit() {
            this.$parent.$parent.menu = false;
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>

</style>
