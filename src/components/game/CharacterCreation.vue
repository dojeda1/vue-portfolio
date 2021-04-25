<template>
    <template v-if="task == 'name'">
        <h5>{{$parent.message}}</h5>
        <form>
            <input class="text-white" :class="{'text-red': name.length > 16}" type="text" v-model="name" placeholder="Killgore">
            <button @click="editName" v-on:submit.prevent="editName(event)">Enter</button>
        </form>
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
            name: '',
            race: '',
            class: '',
            task: 'name'
        }
    },
    methods: {
        log(msg) {
            console.log('Log:',msg);
        },
        editName(event) {
            event.preventDefault()
            if (this.name.length > 16) {
                this.$parent.message = 'A bit long... Perhaps a nickname?'
            } else if (this.name.length) {
                this.$parent.player.name = this.name.trim().charAt(0).toUpperCase() + this.name.slice(1);
                this.task = 'race'
                console.log('player:',this.$parent.player);
            } else {
                this.$parent.message = 'Surely, you must have a name...'
            }
        },
        editRace(race) {
            const player = this.$parent.player;

            let hpMax;
            let mpMax;
            let strength;
            let defense;
            let will;
            let speed;
            let luck;
            switch (race) {
                case "Human":
                    hpMax = 4;
                    mpMax = 4;
                    strength = 1;
                    defense = 1;
                    will = 1;
                    speed = 1;
                    luck = 0;
                    break;
                case "Elf":
                    hpMax = 4;
                    mpMax = 6;
                    strength = 1;
                    defense = 0;
                    will = 2;
                    speed = 1;
                    luck = 1;
                    break;
                case "Dwarf":
                    hpMax = 6;
                    mpMax = 2;
                    strength = 2;
                    defense = 1;
                    will = 0;
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
            player.will += will;
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
            let will;
            let speed;
            let luck;
            switch (cls) {
                case "Warrior":
                    hpMax = 4
                    mpMax = 0
                    strength = 2
                    defense = 2
                    will = 0
                    speed = 0
                    luck = 0
                    this.$parent.addAbility(player.abilities,'Axe Strike',false)
                    this.$parent.addAbility(player.abilities,'Berserk',false)
                    break;
                case "Mage":
                    hpMax = 2
                    mpMax = 6
                    strength = 0
                    defense = 0
                    will = 7
                    speed = 0
                    luck = 1
                    this.$parent.addAbility(player.abilities,'Fireball',false)
                    this.$parent.addAbility(player.abilities,'Heal',false)
                    break;
                case "Rogue":
                    hpMax = 2
                    mpMax = 2
                    strength = 1
                    defense = 0
                    will = 1
                    speed = 2
                    luck = 2
                    this.$parent.addAbility(player.abilities,'Poison Dagger',false)
                    this.$parent.addAbility(player.abilities,'Steal',false)
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
            player.will += will;
            player.speed += speed;
            player.luck += luck;

            this.editSprite(player.race,player.class);
            //Potions
            this.$parent.addItem(player.inventory,this.$parent.items[0][0]);
            this.$parent.addItem(player.inventory,this.$parent.items[0][1]);
            // this.$parent.addItem(player.inventory,this.$parent.items[0][0]);
            // this.$parent.addItem(player.inventory,this.$parent.items[0][1]);
            // this.$parent.addItem(player.inventory,this.$parent.items[1][0]);
            // this.$parent.addItem(player.inventory,this.$parent.items[1][1]);
            // this.$parent.addItem(player.inventory,this.$parent.items[2][0]);
            // this.$parent.addItem(player.inventory,this.$parent.items[2][1]);
            //Status
            // this.$parent.addItem(player.inventory,this.$parent.items[3][0]);
            // this.$parent.addItem(player.inventory,this.$parent.items[3][0]);
            // this.$parent.addItem(player.inventory,this.$parent.items[3][1]);
            // this.$parent.addItem(player.inventory,this.$parent.items[3][1]);
            // this.$parent.addItem(player.inventory,this.$parent.items[3][2]);
            // this.$parent.addItem(player.inventory,this.$parent.items[3][2]);
            //Scrolls
            // this.$parent.addItem(player.inventory,this.$parent.items[4][0]);
            // this.$parent.addItem(player.inventory,this.$parent.items[4][1]);
            // this.$parent.addItem(player.inventory,this.$parent.items[4][2]);
            // this.$parent.addItem(player.inventory,this.$parent.items[4][3]);
            // this.$parent.addItem(player.inventory,this.$parent.items[4][4]);
            // this.$parent.addItem(player.inventory,this.$parent.items[4][5]);
            //Valuables
            // this.$parent.addItem(player.inventory,this.$parent.items[5][0]);
            // this.$parent.addItem(player.inventory,this.$parent.items[5][1]);
            //Elixirs
            // this.$parent.addItem(player.inventory,this.$parent.items[6][0]);
            // this.$parent.addItem(player.inventory,this.$parent.items[6][1]);
            // this.$parent.addItem(player.inventory,this.$parent.items[6][2]);
            // this.$parent.addItem(player.inventory,this.$parent.items[6][3]);
            // this.$parent.addItem(player.inventory,this.$parent.items[6][4]);
            // this.$parent.addItem(player.inventory,this.$parent.items[6][5]);
            // this.$parent.addItem(player.inventory,this.$parent.items[6][6]);
            //Tomes
            // this.$parent.addItem(player.inventory,this.$parent.items[7][0]);
            // this.$parent.addItem(player.inventory,this.$parent.items[7][1]);
            // this.$parent.addItem(player.inventory,this.$parent.items[7][2]);
            // this.$parent.addItem(player.inventory,this.$parent.items[7][3]);
            // this.$parent.addItem(player.inventory,this.$parent.items[7][4]);
            // this.$parent.addItem(player.inventory,this.$parent.items[7][5]);
            //Unique
            // this.$parent.addItem(player.inventory,this.$parent.items[8][0]);
            //Add Stats
            player.xp+=2000
            // player.abilities.push({
            //     name: "Evil Eye",
            //     sprite: '/images/game/evil-eye.png',
            //     cost: 1,
            //     active: true
            // })
            // player.abilities.push({
            //     name: "Steal",
            //     cost: 1,
            //     active: true
            // })
            // player.abilities.push({
            //     name: "Axe Strike",
            //     sprite: '/images/game/strike.png',
            //     cost: 1,
            //     active: true
            // })
            // player.abilities.push({
            //     name: "Bite",
            //     sprite: '/images/game/bite.png',
            //     cost: 1,
            //     active: true
            // })
            // player.abilities.push({
            //     name: "Fireball",
            //     sprite: '/images/game/fireball.png',
            //     cost: 1,
            //     active: true
            // })
            // player.status['Berserk'] = 5;
            // player.status['Burn'] = 5;
            // player.status['Poison'] = 5;
            // player.status['Bleed'] = 5;

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
        this.$parent.resetRegion();
        this.$parent.message = 'Hello Adventurer, what is your name?';
        this.$parent.location = 'Character Creation';
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>

</style>
