<template>
    <div class="message-box">
        <h5>{{ $parent.message }}</h5>
        <p v-for="(msg, index) in $parent.messageBox" :key="index">
            {{ msg }}
        </p>
    </div>
    <p>Save your Progress - Save in Town</p>
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
            console.log('Game Loaded')
            this.$parent.message = 'Your game has been Loaded.'
            this.$parent.player = JSON.parse(localStorage.getItem("player"));
            this.$parent.regions = JSON.parse(localStorage.getItem("regions"));
            this.$parent.region = JSON.parse(localStorage.getItem("region"));
            // this.$parent.bosses = JSON.parse(localStorage.getItem("bosses"));
            this.$parent.scene = 'Town'
            this.$parent.location = 'Town'
            this.$parent.resetMerchant()
            this.$parent.resetQuestBoard()
            console.log('P1:',this.$parent.player)
            console.log('R:',this.$parent.region)
            console.log('Rs:',this.$parent.regions)
            // console.log('B:',this.$parent.bosses)
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
