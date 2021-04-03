<template>
    <div class="event-display">
        <div class="text-blue" :class="{'text-gray': $parent.player.hp <= 0}">
            <img class="game-sprite"
            :class="{ 'idle': $parent.player.animation == 'idle',
            'walk': $parent.player.animation == 'walk' ,
            'dodge': $parent.player.animation == 'dodge' ,
            'die-left': $parent.player.animation == 'die' ,
            'damage': $parent.player.animation == 'damage' ,
            'attack-right': $parent.player.animation == 'attack' }"
            :src="$parent.player.sprite"
            :alt="$parent.player.name">
            <p>
                {{ $parent.player.name }} | 
                <span 
                :class="{'text-red': $parent.player.hp < $parent.player.hpMax/2,
                'text-gray': $parent.player.hp <= 0}">
                HP: {{ $parent.player.hp }}/{{ $parent.player.hpMax }}</span> |  
                <span 
                :class="{'text-red': $parent.player.mp < $parent.player.mpMax/2,
                'text-gray': $parent.player.hp <= 0}">
                MP: {{ $parent.player.mp }}/{{ $parent.player.mpMax }}</span>
            </p>
            <p>
                <span >XP: {{ $parent.player.xp }}/{{ $parent.player.nextLevel }}</span> | 
                <span :class="{'text-red': $parent.player.gold < 10}">{{ $parent.player.gold }}g</span>
            </p>
        </div>
        <div class="text-green" :class="{'text-gray': $parent.currentEnemy.hp <= 0}">
            <img class="game-sprite"
            :class="{ 'idle': $parent.currentEnemy.animation == 'idle',
            'walk': $parent.currentEnemy.animation == 'walk' ,
            'dodge': $parent.currentEnemy.animation == 'dodge' ,
            'die-right': $parent.currentEnemy.animation == 'die' ,
            'damage': $parent.currentEnemy.animation == 'damage' ,
            'attack-left': $parent.currentEnemy.animation == 'attack' }"
            :src="$parent.currentEnemy.sprite"
            :alt="$parent.currentEnemy.name">
            <p>
                {{ $parent.currentEnemy.name }} | 
                <span 
                :class="{'text-red': $parent.currentEnemy.hp < $parent.currentEnemy.hpMax/2,
                'text-gray': $parent.currentEnemy.hp <= 0}">
                HP: {{ $parent.currentEnemy.hp }}/{{ $parent.currentEnemy.hpMax }}</span> | 
                <span 
                :class="{'text-red': $parent.currentEnemy.mp < $parent.currentEnemy.mpMax/2,
                'text-gray': $parent.currentEnemy.hp <= 0}">MP: {{ $parent.currentEnemy.mp }}/{{ $parent.currentEnemy.mpMax }}</span>
            </p>
        </div>
    </div>
    <div class="message-box">
        <h5>{{ $parent.message }}</h5>
        <p v-for="(msg, index) in $parent.messageBox" :key="index">
            {{ msg }}
        </p>
    </div>
    <template v-if="task == 'battle'">
        <p>What next?</p>
        <p>
            <button class="btn-blue" @click="handleAttack" :class="{ 'disabled' : !playerTurn}">Attack</button>
        </p>
        <template v-if="$parent.player.special1.active">
            <p>
                <button class="btn-blue" @click="handleSpecial($parent.player.special1)"
                :class="{ 'disabled' : !playerTurn || $parent.player.special1.cost > $parent.player.mp}">
                {{$parent.player.special1.name}} - {{$parent.player.special1.cost}} mp</button>
            </p>
        </template>
        <template v-if="$parent.player.special2.active">
            <p>
                <button class="btn-blue" @click="handleSpecial($parent.player.special2)"
                :class="{ 'disabled' : !playerTurn || $parent.player.special2.cost > $parent.player.mp}">
                {{$parent.player.special2.name}} - {{$parent.player.special1.cost}} mp</button>
            </p>
        </template>
        <p>
            <button class="btn-blue" @click="handleUseItem" :class="{ 'disabled' : !playerTurn}">Use Item</button>
        </p>
        <p>
            <button class="btn-blue" @click="handleRun" :class="{ 'disabled' : !playerTurn}"><i className="material-icons left">arrow_back</i>Run</button>
        </p>
    </template>
    <template v-else-if="task == 'use item'">
        <p>{{this.$parent.infoText}}</p>
        <button class="btn-blue"
        v-for="(item, index) in $parent.player.inventory"
        :key="index"
            :class="{ 'disabled' : !playerTurn}"
            @mouseover="$parent.infoText = item.info"
            @click="handleAttemptItem(item,index)">{{ item.name }} x {{item.qty}}</button>
        <p>
            <button class="btn-blue" @click="handleBack"><i class="material-icons left">arrow_back</i>Back</button>
        </p>
    </template>
    <template v-else-if="task == 'next'">
        <p>
            <button class="btn-blue" @click="handleNext">Next</button>
        </p>
    </template>
    <template v-else-if="task == 'end'">
        <p>
            <button class="btn-blue" @click="handleEnd">End</button>
        </p>
    </template>
</template>

<script>
export default {
    name: 'Battle',
    data() {
        return {
            playerTurn: true,
            task: 'battle'
        }
    },
    methods: {
        log(msg) {
            console.log('Log:',msg);
        },
        // Combat Functions
        handleAttack() {
            this.playerTurn = false;
            let player = this.$parent.player;
            let enemy = this.$parent.currentEnemy;
            this.$parent.messageBox = [];
            let $this = this;
            $this.attack(player, enemy)
            setTimeout(function() {
                $this.enemyTurn(player, enemy);
            }, 1200);
        },
        handleNext() {
            this.$parent.messageBox = [];
            this.$parent.changeScene('Wild');
        },
        handleBack() {
            this.task = 'battle';
        },
        handleUseItem() {
            this.infoText = 'Select an item'
            this.task = 'use item';
        },
        handleAttemptItem(item,index) {
            let $this = this;
            let player = this.$parent.player;
            let enemy = this.$parent.currentEnemy;
            this.$parent.handleAttemptItem(item,index,
                function() {
                    $this.handleBack();
                    $this.playerTurn = false;
                    setTimeout(function() {
                        $this.enemyTurn(player, enemy);
                    }, 1200)
                }
            );
        },
        enemyTurn(player, enemy) {
            console.log('BIOTCH')
            if (enemy.hp <= 0) {
                let regionIndex = this.$parent.region.index;
                player.totalKills++;
                let text = this.$parent.messageBox;
                text.push("You killed " + enemy.name + "!")
                text.push("--- RESULTS ---")
                this.dropGold();
                this.dropLoot(enemy)
                this.gainXp(enemy.xp, player);
                // this.killQuestCheck(enemy.name);
                this.$parent.message = this.$parent.currentEnemy.name + ' defeated.';
                this.task = 'next';
                console.log("total kills: " + player.totalKills);
                if (enemy.type === "endBoss") {
                    const endBosses = this.$parent.endBosses;
                    endBosses[enemy.index].isDead = true;
                } else if (enemy.type === "boss") {
                    let bossArray = [];
                    switch (regionIndex) {
                        case 1:
                            bossArray = this.$parent.bosses1;
                            break;
                        case 2:
                            bossArray = this.$parent.bosses2;
                            break;
                        case 3:
                            bossArray = this.$parent.bosses3;
                            break;

                        default:
                        // code block
                    }
                    bossArray.splice(enemy.index, 1);
                }
                if (this.$parent.location === "Dungeon") {
                    this.$parent.dungeonCount++
                    if (this.$parent.dungeonCount >= this.$parent.region.dungeonGoal) {
                        player.totalDungeons++
                    }
                }
                // if (this.$parent.location === "Castle") {
                //     this.setState({
                //         castleCount: this.$parent.castleCount + 1
                //     }, () => {
                //         if (this.$parent.castleCount > this.$parent.region.dungeonGoal) {
                //             player.totalDungeons++
                //             this.setState({
                //                 player: player
                //             })
                //         }
                //     })
                // }

                // enemy can use health potion

                // } else if (this.hasItem(enemy.inventory, "Health Potion") && enemy.hp < enemy.hpMax * 0.25) {
                //     let thisIndex;
                //     enemy.inventory.forEach((element, index) => {
                //         if ("Health Potion" === element.name) {
                //             thisIndex = index;
                //         };
                //     });
                //     this.setState({
                //         step: "select move"
                //     }, () => this.activateItem(enemy, player, "Health Potion", thisIndex));

            } else {
                this.attack(enemy, player);
            }
            var $this = this;
            setTimeout(function() {
                if (player.hp > 0) {
                    player.animation = 'idle'
                } else {
                    player.animation = 'die';
                }
                if (enemy.hp > 0) {
                    enemy.animation = 'idle'
                } else {
                    enemy.animation = 'die';
                }
                $this.playerTurn = true;
            },600)
            this.gameOverCheck();
        },
        handleSpecial(special) {
            const cost = special.cost
            const name = special.name;
            console.log("Special: " + name + " " + cost);
            if (name === "Heal" && this.$parent.player.hp >= this.$parent.player.hpMax) {
                this.$parent.message = "You are already at full health."
            } else if (name === "Steal" && !this.$parent.currentEnemy.inventory.length) {
                this.$parent.message = "There is nothing to steal."
            } else {
                this.playerTurn = false;
                let player = this.$parent.player;
                let enemy = this.$parent.currentEnemy;
                this.$parent.messageBox = [];
                let $this = this;
                // $this.attack(player, enemy)
                $this.special(player, enemy, name, cost);
                setTimeout(function() {
                    $this.enemyTurn(player, enemy);
                }, 1200);
                //
            }
        },
        attack(attacker, defender) {
            attacker.animation = 'attack'
            let berserkNum = 0;
            if (attacker.isBerserk) {
                attacker.berserkCount++
                berserkNum = Math.floor(attacker.strength / 2)
            }

            if (attacker.isBerserk) {
                attacker.berserkCount++;
                // console.log(attacker.berserkCount);
                if (attacker.berserkCount > 2) {
                    attacker.berserkCount = 0;
                    attacker.isBerserk = false;
                    attacker.strength = attacker.berserkAtkHold
                    console.log(" - Berserk has run out. - \n")
                }
            }

            console.log(attacker.name + " attacked " + defender.name)
            let attackMessage;
            let damage = 0;
            let defense = Math.floor(defender.defense / 3);
            let criticalCheck = this.$parent.randNum(1, 100);
            let missCheck = this.$parent.randNum(1, 100);
            let luckCheck = (attacker.luck - defender.luck) + 10;
            if (luckCheck > 95) {
                luckCheck = 95;
            } else if (luckCheck < 5) {
                luckCheck = 5;
            }
            let speedCheck = (defender.speed - attacker.speed + 5);
            if (speedCheck > 95) {
                speedCheck = 95;
            } else if (speedCheck < 5) {
                speedCheck = 5;
            }
            console.log("rand/CritCheck: " + criticalCheck + "/" + luckCheck);
            console.log("rand/MissCheck: " + missCheck + "/" + speedCheck);
            if (missCheck < speedCheck) {
                attackMessage = attacker.name + " missed...";
                setTimeout(function() {
                    defender.animation = 'dodge'
                },300);
            } else if (criticalCheck >= luckCheck) {
                setTimeout(function() {
                    defender.animation = 'damage'
                },300);
                damage = attacker.strength + berserkNum - defense;
                if (damage < 1) {
                    damage = 1;
                }
                attackMessage = attacker.name + " did " + damage + " damage.";
            } else {
                setTimeout(function() {
                    defender.animation = 'damage'
                },300);
                damage = attacker.strength + Math.floor(attacker.strength * 0.25) + berserkNum;
                attackMessage = "Critical hit! " + attacker.name + " did " + damage + " damage.";
            }
            defender.hp -= damage;
            this.$parent.messageBox.push(attackMessage);
            console.log(attackMessage);
            // this.atkText(attacker, attackMessage);
            // attacker.berserkCheck();
        },
        special (attacker, defender, name, cost) {
            let attackMessage;
            let damage = 0;
            let defense = defender.defense;
            let criticalCheck = this.$parent.randNum(1, 100);
            let missCheck = this.$parent.randNum(1, 100);
            let luckCheck = (attacker.luck - defender.luck) + 10;
            if (luckCheck > 95) {
                luckCheck = 95;
            } else if (luckCheck < 5) {
                luckCheck = 5;
            }
            let speedCheck = (defender.speed - attacker.speed + 2);
            if (speedCheck > 95) {
                speedCheck = 95;
            } else if (speedCheck < 3) {
                speedCheck = 3;
            }

            switch (name) {
                case "Axe Strike":
                    attacker.animation = 'attack'
                    console.log("rand/CritCheck: " + criticalCheck + "/" + luckCheck);
                    console.log("rand/MissCheck: " + missCheck + "/" + speedCheck);
                    if (missCheck < speedCheck) {
                        attackMessage = attacker.name + "'s Axe missed...";
                        setTimeout(function() {
                            defender.animation = 'dodge'
                        },300);
                    } else if (criticalCheck >= luckCheck) {
                        setTimeout(function() {
                            defender.animation = 'damage'
                        },300);
                        defense = Math.floor(defense / 4);
                        // damage = attacker.strength + berserkNum - defense;
                        damage = attacker.strength + Math.floor(attacker.strength * 0.25) - defense;
                        if (damage < 1) {
                            damage = 1;
                        }
                        attackMessage = attacker.name + "'s Axe did " + damage + " damage.";
                    } else {
                        setTimeout(function() {
                            defender.animation = 'damage'
                        },300);
                        damage = attacker.strength + Math.floor(attacker.strength * 0.5);
                        attackMessage = "Critical hit! " + attacker.name + "'s Axe did " + damage + " damage.";
                    }
                    defender.hp -= damage;
                    attacker.mp -= cost;
                    this.$parent.messageBox.push(attackMessage);
                    console.log(attackMessage);
                    break;

                case "Berserk":
                    attacker.mp -= cost;
                    attacker.isBerserk = true;
                    attacker.berserkCount = 0;
                    // this.atkText(attacker, attacker.name + " is now Berserk!");
                    // attacker.berserkCheck();
                    break;

                case "Fireball":
                    attacker.animation = 'attack'
                    console.log("rand/CritCheck: " + criticalCheck + "/" + luckCheck);
                    console.log("rand/MissCheck: " + missCheck + "/" + speedCheck);
                    if (missCheck < speedCheck) {
                        attackMessage = attacker.name + "'s Fire missed...";
                        setTimeout(function() {
                            defender.animation = 'dodge'
                        },300);
                    } else if (criticalCheck >= luckCheck) {
                        setTimeout(function() {
                            defender.animation = 'damage'
                        },300);
                        defense = Math.floor(defense / 4);
                        damage = attacker.mana + Math.floor(attacker.mana * 0.25) - defense;
                        if (damage < 1) {
                            damage = 1;
                        }
                        attackMessage = attacker.name + "'s Fire did " + damage + " damage.";
                    } else {
                        setTimeout(function() {
                            defender.animation = 'damage'
                        },300);
                        damage = attacker.mana + Math.floor(attacker.mana * 0.5);
                        attackMessage = "Critical hit! " + attacker.name + "'s Fire did " + damage + " damage.";
                    }
                    defender.hp -= damage;
                    attacker.mp -= cost;
                    this.$parent.messageBox.push(attackMessage);
                    console.log(attackMessage);
                    // attacker.berserkCheck();
                    break;

                case "Heal":
                    luckCheck = attacker.luck + 8;
                    if (luckCheck > 95) {
                        luckCheck = 95;
                    } else if (luckCheck < 5) {
                        luckCheck = 5;
                    }
                    console.log("rand/CritCheck: " + criticalCheck + "/" + luckCheck);
                    if (criticalCheck >= luckCheck) {
                        damage = Math.floor(attacker.hpMax * 0.75);
                        attackMessage = attacker.name + " recovered " + damage + " HP.";
                    } else {
                        damage = attacker.hpMax;
                        attackMessage = "Wow! " + attacker.name + " recovered max HP.";
                    }
                    attacker.hp += damage;
                    if (attacker.hp > attacker.hpMax) {
                        attacker.hp = attacker.hpMax;
                    }
                    attacker.mp -= cost;
                    // this.atkText(attacker, attackMessage);

                    // attacker.berserkCheck();
                    break;

                case "Dagger Slash":
                    console.log("rand/CritCheck: " + criticalCheck + "/" + luckCheck);
                    console.log("rand/MissCheck: " + missCheck + "/" + speedCheck);
                    attacker.animation = 'attack'
                    console.log("rand/CritCheck: " + criticalCheck + "/" + luckCheck);
                    console.log("rand/MissCheck: " + missCheck + "/" + speedCheck);
                    if (missCheck < speedCheck) {
                        attackMessage = attacker.name + "'s Dagger missed...";
                        setTimeout(function() {
                            defender.animation = 'dodge'
                        },300);
                    } else if (criticalCheck >= luckCheck) {
                        setTimeout(function() {
                            defender.animation = 'damage'
                        },300);
                        defense = Math.floor(defense / 4);
                        // damage = attacker.strength + berserkNum - defense;
                        damage = attacker.strength + Math.floor(attacker.strength * 0.25) - defense;
                        if (damage < 1) {
                            damage = 1;
                        }
                        attackMessage = attacker.name + "'s Dagger did " + damage + " damage.";
                    } else {
                        setTimeout(function() {
                            defender.animation = 'damage'
                        },300);
                        damage = attacker.strength + Math.floor(attacker.strength * 0.5);
                        attackMessage = "Critical hit! " + attacker.name + "'s Dagger did " + damage + " damage.";
                    }
                    defender.hp -= damage;
                    attacker.mp -= cost;
                    this.$parent.messageBox.push(attackMessage);
                    console.log(attackMessage);

                    // attacker.berserkCheck();
                    break;

                case "Steal":
                    speedCheck = (attacker.speed - defender.speed) + 60;
                    if (speedCheck > 95) {
                        speedCheck = 95;
                    } else if (speedCheck < 5) {
                        speedCheck = 5;
                    }
                    console.log("rand/CritCheck: " + criticalCheck + "/" + speedCheck);
                    if (criticalCheck <= speedCheck) {
                        const itemNum = this.$parent.randNum(0, defender.inventory.length);
                        const item = defender.inventory[itemNum];
                        this.transferItem(defender.inventory, attacker.inventory, item);
                        // this.atkText(attacker, attacker.name + " stole " + this.$parent.anA(item.name) + " " + item.name + ".");
                    } else {
                        // this.atkText(attacker, attacker.name + " failed to steal anything.");
                    }

                    attacker.mp -= cost;
                    // attacker.berserkCheck();
                    break;

                default:
                this.$parent.message = "ERRor"
                // code block
            }
        },
        gameOverCheck() {
            if (this.$parent.player.hp <= 0) {
                let text = this.$parent.messageBox;
                text.push(this.$parent.currentEnemy.name + " killed you.");
                text.push("--- RESULTS ---");
                text.push("Monsters Killed: " + this.$parent.player.totalKills);
                text.push("Gold Collected: " + this.$parent.player.totalGold);
                text.push("Quests Completed: " + this.$parent.player.totalQuests);
                text.push("Dungeons Completed: " + this.$parent.player.totalDungeons);
                // this.$parent.scene="GameOver";
                this.$parent.message = 'Game Over.';
                this.task = 'end';
            }
            console.log('player:',this.$parent.player);
        },
        handleEnd() {
            this.$parent.changeScene('TitleScreen');
            // this.changePlayStates("Title Screen", "new or load");
            // this.setState({
            //     player: playerDefault,
            //     message: "Start again?"
            // });
        },
        gainXp(xpNum, player) {
            player.xp += xpNum;
            let text = this.$parent.messageBox;
            text.push("You gained " + xpNum + " XP.");
            // this.setState({
            //     player: player,
            //     xpResult: xpNum,
            //     messageBox: text
            // }, () => this.levelUpCheck(this.$parent.player))
            this.levelUpCheck(this.$parent.player);
        },
        levelUpCheck(player) {
            if (player.xp >= player.nextLevel) {
                player.level++;
                player.nextLevel += player.level * 50;
                player.strength += 2;
                player.defense += 1;
                player.speed += 1;
                player.mana += 2;
                player.luck += 1;
                player.hpMax += 5;
                player.hp = player.hpMax;
                player.mpMax += 2;
                player.mp = player.mpMax;

                let text = this.$parent.messageBox;
                text.push("You are now lv. " + player.level + "!!!");
                if (player.level === 2) {
                    player.special1.active = true;
                    text.push("You learned " + player.special1.name + "!!!!!");
                } else if (player.level === 4) {
                    player.special2.active = true;
                    text.push("You learned " + player.special2.name + "!!!!!");
                }
                this.levelUpCheck(player)
            }
        },
        handleRun() {
            const player = this.$parent.player
            const lostGold = this.$parent.randNum(0, Math.floor(player.gold / 2));
            player.gold -= lostGold;
            const lostHp = this.$parent.randNum(0, 3);
            player.hp -= lostHp;
            this.$parent.changeScene('Wild');
            this.gameOverCheck();
            this.$parent.message = "You lost " + lostGold + " gold and " + lostHp + " HP.";
            // this.setState({
            //     task: "select where",
            //     step: null,
            //     movingForward: false,
            //     message: "You lost " + lostGold + " gold and " + lostHp + " HP.",
            // });

        },
        dropGold() {
            const amount = this.$parent.randNum(2, this.$parent.currentEnemy.gold);
            let text = this.$parent.messageBox;
            text.push(this.$parent.currentEnemy.name + " dropped " + amount + " gold.")
            let player = this.$parent.player;
            player.gold += amount;
            player.totalGold += amount;
        },
        dropLoot(enemy) {
            if (enemy.name === "Mimic") {
                let text = this.$parent.messageBox;
                enemy.inventory.forEach(item => {
                    this.transferItem(enemy.inventory, this.$parent.player.inventory, item)
                    text.push(enemy.name + " dropped " + this.$parent.anA(item.name) + " " + item.name + ".");
                });
            } else {
                const lootCheck = this.$parent.randNum(1, 5);
                if (lootCheck === 1) {
                    if (enemy.inventory.length) {
                        const itemNum = this.$parent.randNum(0, enemy.inventory.length);
                        const item = enemy.inventory[itemNum];
                        this.$parent.transferItem(enemy.inventory, this.$parent.player.inventory, item)
                        let text = this.$parent.messageBox;
                        text.push(enemy.name + " dropped " + this.$parent.anA(item.name) + " " + item.name + ".");
                    } else {
                        console.log("enemy has no items")
                    }
                }
            }
        }
    },
    created: function() {
        this.$parent.messageBox = [];
        this.$parent.player.animation = 'idle'
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
    .message-box {
        border: 2px solid gray;
        padding: 10px;
        min-height: 100px;
    }
</style>
