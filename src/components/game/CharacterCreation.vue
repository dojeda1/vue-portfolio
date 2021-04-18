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
            const player = this.$parent.player;

            let hpMax;
            let mpMax;
            let strength;
            let defense;
            let mana;
            let speed;
            let luck;
            switch (race) {
                case "Human":
                    hpMax = 4;
                    mpMax = 4;
                    strength = 1;
                    defense = 1;
                    mana = 1;
                    speed = 1;
                    luck = 0;
                    break;
                case "Elf":
                    hpMax = 4;
                    mpMax = 6;
                    strength = 1;
                    defense = 0;
                    mana = 2;
                    speed = 1;
                    luck = 1;
                    break;
                case "Dwarf":
                    hpMax = 6;
                    mpMax = 2;
                    strength = 2;
                    defense = 1;
                    mana = 0;
                    speed = 0;
                    luck = 0;
                    break;
                default: console.log("error");
            }
            player.race = race;
            player.hpMax += hpMax;
            player.hp += hpMax;
            player.mpMax += mpMax;
            player.mp += mpMax;
            player.strength += strength;
            player.defense += defense;
            player.mana += mana;
            player.speed += speed;
            player.luck += luck;

            this.task = 'class'
            console.log('player:',player);
        },
        editClass(cls) {
            const player = this.$parent.player;

            let hpMax;
            let mpMax;
            let strength;
            let defense;
            let mana;
            let speed;
            let luck;
            switch (cls) {
                case "Warrior":
                    hpMax = 4
                    mpMax = 0
                    strength = 2
                    defense = 2
                    mana = 0
                    speed = 0
                    luck = 0
                    player.specials.push({
                        name: "Axe Strike",
                        cost: 6,
                        active: false
                    })
                    player.specials.push({
                        name: "Berserk",
                        cost: 8,
                        active: false,
                    })
                    break;
                case "Mage":
                    hpMax = 2
                    mpMax = 6
                    strength = 0
                    defense = 0
                    mana = 7
                    speed = 0
                    luck = 1
                    player.specials.push({
                        name: "Fireball",
                        cost: 6,
                        active: false,
                    })
                    player.specials.push({
                        name: "Heal",
                        cost: 10,
                        active: false,
                    })
                    break;
                case "Rogue":
                    hpMax = 2
                    mpMax = 2
                    strength = 1
                    defense = 0
                    mana = 1
                    speed = 2
                    luck = 2
                    player.specials.push({
                        name: "Dagger Slash",
                        cost: 6,
                        active: false,
                    })
                    player.specials.push({
                        name: "Steal",
                        cost: 5,
                        active: false
                    })
                    break;
                default: console.log("error");
            }

            player.class = cls;
            player.hpMax += hpMax;
            player.hp += hpMax;
            player.mpMax += mpMax;
            player.mp += mpMax;
            player.strength += strength;
            player.defense += defense;
            player.mana += mana;
            player.speed += speed;
            player.luck += luck;

            this.editSprite(player.race,player.class);
            this.$parent.addItem(player.inventory,this.$parent.items[0][0]);
            this.$parent.addItem(player.inventory,this.$parent.items[0][0]);
            this.$parent.addItem(player.inventory,this.$parent.items[0][1]);
            this.$parent.addItem(player.inventory,this.$parent.items[0][1]);
            this.$parent.addItem(player.inventory,this.$parent.items[1][0]);
            this.$parent.addItem(player.inventory,this.$parent.items[1][0]);
            this.$parent.addItem(player.inventory,this.$parent.items[1][1]);
            this.$parent.addItem(player.inventory,this.$parent.items[1][1]);
            this.$parent.addItem(player.inventory,this.$parent.items[2][0]);
            this.$parent.addItem(player.inventory,this.$parent.items[2][0]);
            this.$parent.addItem(player.inventory,this.$parent.items[2][1]);
            this.$parent.addItem(player.inventory,this.$parent.items[2][1]);
            this.$parent.addItem(player.inventory,this.$parent.items[3][0]);
            this.$parent.addItem(player.inventory,this.$parent.items[3][0]);
            this.$parent.addItem(player.inventory,this.$parent.items[3][1]);
            this.$parent.addItem(player.inventory,this.$parent.items[3][1]);
            this.$parent.addItem(player.inventory,this.$parent.items[3][2]);
            this.$parent.addItem(player.inventory,this.$parent.items[3][2]);
            this.$parent.addItem(player.inventory,this.$parent.items[4][0]);
            this.$parent.addItem(player.inventory,this.$parent.items[4][1]);
            this.$parent.addItem(player.inventory,this.$parent.items[4][2]);
            this.$parent.addItem(player.inventory,this.$parent.items[4][3]);
            player.animation = 'idle';

            console.log('player:',player);
            this.setUpGame()
        },
        editSprite(race,cls) {
            // this.$parent.sprite = $this.playerSprites[race + cls];
            if (race == 'Human') {
                switch (cls) {
                    case "Warrior":
                        this.$parent.player.sprite = '/images/game/human-warrior.png'
                        break;
                    case "Mage":
                        this.$parent.player.sprite = '/images/game/human-mage.png'
                        break;
                    case "Rogue":
                        this.$parent.player.sprite = '/images/game/human-rogue.png'
                        break;
                    default: console.log("error");
                }
            } else if (race == 'Elf') {
                switch (cls) {
                    case "Warrior":
                        this.$parent.player.sprite = '/images/game/elf-warrior.png'
                        break;
                    case "Mage":
                        this.$parent.player.sprite = '/images/game/elf-mage.png'
                        break;
                    case "Rogue":
                        this.$parent.player.sprite = '/images/game/elf-rogue.png'
                        break;
                    default: console.log("error");
                }
            } else if (race == 'Dwarf') {
                switch (cls) {
                    case "Warrior":
                        this.$parent.player.sprite = '/images/game/dwarf-warrior.png'
                        break;
                    case "Mage":
                        this.$parent.player.sprite = '/images/game/dwarf-mage.png'
                        break;
                    case "Rogue":
                        this.$parent.player.sprite = '/images/game/dwarf-rogue.png'
                        break;
                    default: console.log("error");
                }
            }
        },
        setUpGame() {
            this.$parent.changeScene('Wild');
            this.$parent.message = 'Your adventure begins...';
            this.$parent.messageBox = [];
        }
    },
    created: function() {
        this.$parent.resetPlayer();
        this.$parent.resetBosses();
        this.$parent.resetRegion();
        this.$parent.location = 'Character Creation';
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>

</style>
