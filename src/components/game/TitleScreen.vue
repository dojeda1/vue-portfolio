<template>
    <h5>Choose an Option</h5>
    <p>
        <button @click="changeScene('CharacterCreation')">New Game</button>
        <button>Continue</button>
    </p>
</template>

<script>
export default {
    name: 'TitleScreen',
    data() {
        return {
            x: true,
        }
    },
    methods: {
        log(msg) {
            console.log('Log:',msg);
        },
        changeScene(nextScene) {
            this.$parent.changeScene(nextScene);
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>

</style>
