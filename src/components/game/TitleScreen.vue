<template>
    <template v-if="task == 'title'">
        <h5>Choose an Option</h5>
        <p>
            <button @click="changeScene('CharacterCreation')">New Game</button>
            <button :class="{'disabled' : !$parent.profiles.length}" @click="handleLoad">Load Game</button>
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
                <div>
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
    name: 'TitleScreen',
    data() {
        return {
            task: 'title',
        }
    },
    methods: {
        log(msg) {
            console.log('Log:',msg);
        },
        handleBack() {
            this.task = 'title'
        },
        changeScene(nextScene) {
            this.$parent.changeScene(nextScene);
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
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
    .profiles {
        display: grid;
        grid-gap: 10px;
        margin-top: 10px;
        grid-template-columns: 1fr 1fr 1fr;
    }
    .profile {
        display: grid;
        grid-template-rows: 1fr min-content;
        border: 2px solid gray;
        grid-column: span 1;
        border-radius: 10px;
        padding: 10px;
    }
    @media screen and (max-width: 600px) {
        .profile {
            grid-column: span 3;
        }
    }
</style>
