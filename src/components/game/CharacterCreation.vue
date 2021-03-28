<template>
    <h5>Hello Adventurer, what is your name?</h5>
    <input type="text">
    <p>
        <button>Enter</button>
    </p>
    <h5>What is your race?</h5>
    <p>
        <button>Human</button>
        <button>Elf</button>
        <button>Dwarf</button>
    </p>
    <h5>What is your class?</h5>
    <p>
        <button>Warrior</button>
        <button>Mage</button>
        <button>Rogue</button>
    </p>
</template>

<script>
export default {
    name: 'CharacterCreation',
    data() {
        return {
            name: '',
            race: '',
            class: '',
        }
    },
    methods: {
        log(msg) {
            console.log('Log:',msg);
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>

</style>
