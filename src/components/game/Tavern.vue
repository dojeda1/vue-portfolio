<template>
    <template v-if="task == 'tavern'">
        <p>What Next?</p>
        <p>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn}" @click="handleMead">Buy a Mead</button>
        </p>
        <p>
            <button class="btn-blue disabled" :class="{ 'disabled' : !playerTurn}" @click="handleSell">Play a Game</button>
        </p>
        <p>
            <button class="btn-inv" :class="{ 'disabled' : !playerTurn}" @click="handleLeave"><i class="material-icons left">arrow_back</i>Leave</button>
        </p>
    </template>
    <template v-else-if="task == 'mead'">
        <p>Purchase mead?</p>
        <p>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn}" @click="handleYesMead">Yes</button>
        </p>
        <p>
            <button class="btn-inv" :class="{ 'disabled' : !playerTurn}" @click="handleNoMead"><i class="material-icons left">arrow_back</i>No</button>
        </p>
    </template>
</template>

<script>
export default {
    name: 'Tavern',
    data() {
        return {
            task: 'tavern',
            playerTurn: true
        }
    },
    methods: {
        log(msg) {
            console.log('Log:',msg);
        },
        handleMead() {
            const player = this.$parent.player
            // if (player.hp < player.hpMax || player.mp < player.mpMax) {
                const cost = 10 + (player.level - 2) * 2;
                this.$parent.message = 'It costs ' + cost + 'g for a mead.'
                this.task = 'mead';
            // } else {
            //     this.$parent.message = 'You are already at full Health and Mana.'
            // }
        },
        handleYesMead() {
            let player = this.$parent.player;
            const cost = 8;
            const $this = this
            if (player.gold >= cost) {
                player.gold = player.gold - cost;
                player.hp += 10;
                player.mp += 5;
                if (player.hp > player.hpMax) {
                    player.hp = player.hpMax;
                }
                if (player.mp > player.mpMax) {
                    player.mp = player.mpMax;
                }

                let meadNum = this.$parent.meadCount++;
                console.log('meadCount',this.$parent.meadCount)
                if (meadNum < 3) {
                    this.$parent.message = "It's Refreshing."
                    player.animation = 'buffer'
                    this.playerTurn = false
                    setTimeout(function() {
                        player.animation = 'idle'
                        $this.playerTurn = true
                        $this.task = 'tavern'
                    },600) 
                } else if (meadNum < 4) {
                    this.$parent.message = "You are starting to feel drunk..."
                    player.animation = 'buffer'
                    this.playerTurn = false
                    setTimeout(function() {
                        player.animation = 'idle'
                        $this.playerTurn = true
                        $this.task = 'tavern'
                    },600)
                } else if (meadNum < 5) {
                    this.$parent.message = "Maaaysssbe wwuuun meerrrrr..."
                    player.animation = 'buffer'
                    this.playerTurn = false
                    setTimeout(function() {
                        player.animation = 'idle'
                        $this.playerTurn = true
                        $this.task = 'tavern'
                    },600)
                } else {
                    player.animation = 'die'
                    this.playerTurn = false
                    $this.$parent.message = "..."
                    setTimeout(function() {
                        player.animation = 'idle'
                        $this.playerTurn = true
                        $this.$parent.message = "You blacked out, waking hours later inside a dungeon."
                        $this.$parent.dungeonCount = 0;
                        $this.$parent.changeScene('Dungeon');
                        player.hp = 3
                        player.mp = 0
                    },3000)
                }
            } else {
                this.$parent.message = "You can't afford it."
                this.task = 'tavern'
            }
        },
        handleNoMead() {
            this.$parent.message = "You decided against it."
            this.task = 'tavern'
        },
        handleLeave() {
            this.$parent.message = "You left the Tavern."
            this.$parent.changeScene('Town');
        },
        handleBack() {
            this.task = 'shop';
        }
    },
    created: function() {
        // this.$parent.location = 'Wild';
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>

</style>
