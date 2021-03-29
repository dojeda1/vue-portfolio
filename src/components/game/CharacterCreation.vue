<template>
    <template v-if="task == 'name'">
        <h5>Hello Adventurer, what is your name?</h5>
        <input type="text" v-model="name" placeholder="Killgore">
        <p>
            <button @click="editName">Enter</button>
        </p>
    </template>
    <template v-if="task == 'race'">
        <h5>What is your race?</h5>
        <p>
            <button @click="editRace('Human')">Human</button>
        </p>
        <p>
            <button @click="editRace('Elf')">Elf</button>
        </p>
        <p>
            <button @click="editRace('Dwarf')">Dwarf</button>
        </p>
    </template>
    <template v-if="task == 'class'">
        <h5>What is your class?</h5>
        <p>
            <button @click="editClass('Warrior')">Warrior</button>
        </p>
        <p>
            <button @click="editClass('Mage')">Mage</button>
        </p>
        <p>
            <button @click="editClass('Rogue')">Rogue</button>
        </p>
    </template>
</template>

<script>
export default {
    name: 'CharacterCreation',
    data() {
        return {
            name: 'Thomas',
            race: '',
            class: '',
            task: 'name'
        }
    },
    methods: {
        log(msg) {
            console.log('Log:',msg);
        },
        editName() {
            if (this.name.length) {
                this.$parent.player.name = this.name.trim();
                this.task = 'race'
                console.log('player:',this.$parent.player);
            } else {
                alert('Must contain at least 1 character')
            }
        },
        editRace(race) {
            let maxHp;
            let maxMp;
            let strength;
            let defense;
            let mana;
            let speed;
            let luck;
            switch (race) {
                case "Human":
                    maxHp = 4;
                    maxMp = 4;
                    strength = 1;
                    defense = 1;
                    mana = 1;
                    speed = 1;
                    luck = 0;
                    break;
                case "Elf":
                    maxHp = 4;
                    maxMp = 6;
                    strength = 0;
                    defense = 0;
                    mana = 2;
                    speed = 1;
                    luck = 1;
                    break;
                case "Dwarf":
                    maxHp = 6;
                    maxMp = 2;
                    strength = 2;
                    defense = 1;
                    mana = 0;
                    speed = 0;
                    luck = 0;
                    break;
                default: console.log("error");
            }
            this.$parent.player.race = race;
            this.$parent.player.maxHp += maxHp;
            this.$parent.player.hp += maxHp;
            this.$parent.player.maxMp += maxMp;
            this.$parent.player.mp += maxMp;
            this.$parent.player.strength += strength;
            this.$parent.player.defense += defense;
            this.$parent.player.mana += mana;
            this.$parent.player.speed += speed;
            this.$parent.player.luck += luck;

            this.task = 'class'
            console.log('player:',this.$parent.player);
        },
        editClass(cls) {
            let maxHp;
            let maxMp;
            let strength;
            let defense;
            let mana;
            let speed;
            let luck;
            let special1
            let special2
            let special1Cost
            let special2Cost
            switch (cls) {
                case "Warrior":
                    maxHp = 4
                    maxMp = 0
                    strength = 2
                    defense = 2
                    mana = 0
                    speed = 0
                    luck = 0
                    special1 = "Axe Strike"
                    special2 = "Berserk"
                    special1Cost = 6
                    special2Cost = 8
                    break;
                case "Mage":
                    maxHp = 2
                    maxMp = 6
                    strength = 0
                    defense = 0
                    mana = 7
                    speed = 0
                    luck = 1
                    special1 = "Fireball"
                    special2 = "Heal"
                    special1Cost = 6
                    special2Cost = 10
                    break;
                case "Rogue":
                    maxHp = 2
                    maxMp = 2
                    strength = 1
                    defense = 0
                    mana = 1
                    speed = 2
                    luck = 2
                    special1 = "Dagger Slash"
                    special2 = "Steal"
                    special1Cost = 6
                    special2Cost = 5
                    break;
                default: console.log("error");
            }
            this.$parent.player.class = cls;
            this.$parent.player.maxHp += maxHp;
            this.$parent.player.hp += maxHp;
            this.$parent.player.maxMp += maxMp;
            this.$parent.player.mp += maxMp;
            this.$parent.player.strength += strength;
            this.$parent.player.defense += defense;
            this.$parent.player.mana += mana;
            this.$parent.player.speed += speed;
            this.$parent.player.luck += luck;
            this.$parent.player.special1 = special1;
            this.$parent.player.special2 = special2;
            this.$parent.player.special1Cost = special1Cost;
            this.$parent.player.special2Cost = special2Cost;

            this.$parent.changeScene('Wild');
            this.$parent.message = 'Your adventure begins...';
            console.log('player:',this.$parent.player);
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>

</style>
