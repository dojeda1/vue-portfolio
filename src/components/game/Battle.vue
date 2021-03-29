<template>
<h5>You encountered a {{ $parent.currentEnemy.name }}...</h5>
    <p class="dom-green-2-text">
        <i class="material-icons left">adb</i>
        {{ $parent.currentEnemy.name }}
        <span class="white-text"> | </span>
        <span class="">HP: {{ $parent.currentEnemy.hp }}/{{ $parent.currentEnemy.maxHp }}</span>
        <span class="white-text"> | </span>
        <span class="">MP: {{ $parent.currentEnemy.mp }}/{{ $parent.currentEnemy.maxMp }}</span>
    </p>
    <p class="dom-blue-text">
        <i class="material-icons left">person</i>
        {{ $parent.player.name }}
        <span class="white-text"> | </span>
        <span class="dom-blue-text">HP: {{ $parent.player.hp }}/{{ $parent.player.maxHp }}</span>
        <span class="white-text"> | </span>
        <span class="dom-blue-text">MP: {{ $parent.player.mp }}/{{ $parent.player.maxMp }}</span>
        <span class="white-text"> | </span>
        <span class="grey-text">XP: {{ $parent.player.xp }}/{{ $parent.player.nextLevel }}</span>
        <span class="white-text"> | </span>
        <span class="red-text">{{ $parent.player.gold }}g</span>
    </p>
    <div class="message-box">
        <p v-for="(msg, index) in $parent.infoText" :key="index">
            {{ msg }}
        </p>
    </div>
    <p>What next?</p>
    <p>
        <button @click="handleAttack">Attack</button>
    </p>
    <p>
        <button class="disabled">Use Item</button>
    </p>
    <p>
        <button class="disabled"><i className="material-icons left">arrow_back</i>Run</button>
    </p>
</template>

<script>
export default {
    name: 'Battle',
    data() {
        return {
            x: true,
        }
    },
    methods: {
        log(msg) {
            console.log('Log:',msg);
        },
            // Combat Functions
        handleAttack() {
            let player = this.$parent.player;
            let enemy = this.$parent.currentEnemy;
            this.$parent.infoText = [];
            // let $this = this;
            // setTimeout(function() {
            //     $this.attack(player, enemy)
            // }, 500);
            // setTimeout(function() {
                //     $this.enemyTurn(player, enemy)
            // }, 1000);
            this.attack(player, enemy);
            this.enemyTurn(player, enemy);
        },
        enemyTurn(player, enemy) {
            if (enemy.hp <= 0) {
                let regionIndex = this.$parent.region.index;
                player.totalKills++;
                let text = this.$parent.infoText;
                text.push("You killed " + enemy.name + "!")
                text.push("--- RESULTS ---")
                // this.setState({
                //     task: "fight",
                //     step: "results",
                //     message: enemy.name + " defeated.",
                //     infoText: text
                // });
                this.dropGold();
                this.dropLoot(enemy)
                this.gainXp(enemy.xp, player);
                // this.killQuestCheck(enemy.name);
                console.log("total kills: " + player.totalKills);
                if (enemy.type === "endBoss") {
                    const endBosses = this.$parent.endBosses;
                    endBosses[enemy.index].isDead = true;
                    // this.setState({
                    //     endBosses: endBosses
                    // });
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
                    // this.setState({
                        this.$parent.dungeonCount++
                    // }, () => {
                        if (this.$parent.dungeonCount >= this.$parent.region.dungeonGoal) {
                            player.totalDungeons++
                            // this.setState({
                            //     player: player
                            // })
                        }
                    // })
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

                // } else if (this.hasItem(enemy.inventory, "Health Potion") && enemy.hp < enemy.maxHp * 0.25) {
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
                // this.setState({
                //     step: "select move"
                // }, () => this.attack(enemy, player));
                this.attack(enemy, player);
            }
        },
        // selectSpecial(event) {
        //     const specialCost = event.target.getAttribute("data-cost");
        //     const specialName = event.target.value;
        //     console.log("Special: " + specialName + " " + specialCost);
        //     if (specialName === "Heal" && this.$parent.player.hp >= this.$parent.player.maxHp) {
        //         this.setState({
        //             message: "You are already at full health."
        //         });
        //     } else if (specialName === "Steal" && !this.$parent.currentEnemy.inventory.length) {
        //         this.setState({
        //             message: "There is nothing to steal."
        //         });
        //     } else {
        //         this.special(this.$parent.player, this.$parent.currentEnemy, specialName, specialCost);
        //         this.enemyTurn(this.$parent.player, this.$parent.currentEnemy);
        //     }
        // },
        attack(attacker, defender) {
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
            } else if (criticalCheck >= luckCheck) {
                damage = attacker.strength + berserkNum - defense;
                if (damage < 1) {
                    damage = 1;
                }
                attackMessage = attacker.name + " did " + damage + " damage.";
            } else {
                damage = attacker.strength + Math.floor(attacker.strength * 0.25) + berserkNum;
                attackMessage = "Critical hit! " + attacker.name + " did " + damage + " damage.";
            }
            defender.hp -= damage;
            this.$parent.infoText.push(attackMessage);
            console.log(attackMessage);
            // this.atkText(attacker, attackMessage);
            // attacker.berserkCheck();
        },
        // special (attacker, defender, name, cost) {
        //     let attackMessage;
        //     let damage;
        //     let defense = defender.defense;
        //     let criticalCheck = this.$parent.randNum(1, 100);
        //     let missCheck = this.$parent.randNum(1, 100);
        //     let luckCheck = (attacker.luck - defender.luck) + 10;
        //     if (luckCheck > 95) {
        //         luckCheck = 95;
        //     } else if (luckCheck < 5) {
        //         luckCheck = 5;
        //     };
        //     let speedCheck = (defender.speed - attacker.speed + 2);
        //     if (speedCheck > 95) {
        //         speedCheck = 95;
        //     } else if (speedCheck < 3) {
        //         speedCheck = 3;
        //     };

        //     switch (name) {
        //         case "Axe Strike":
        //             console.log("rand/CritCheck: " + criticalCheck + "/" + luckCheck);
        //             console.log("rand/MissCheck: " + missCheck + "/" + speedCheck);
        //             if (missCheck < speedCheck) {
        //                 attackMessage = "Axe missed...";
        //             } else if (criticalCheck >= luckCheck) {
        //                 defense = Math.floor(defense / 4);
        //                 damage = attacker.strength + Math.floor(attacker.strength * 0.25) - defense;
        //                 if (damage < 1) {
        //                     damage = 1
        //                 }
        //                 attackMessage = "Axe did " + damage + " damage.";
        //             } else {
        //                 damage = attacker.strength + Math.floor(attacker.strength * 0.5);
        //                 attackMessage = "Critical hit! Axe did " + damage + " damage.";
        //             }
        //             defender.hp -= damage;
        //             attacker.mp -= cost;
        //             this.atkText(attacker, attackMessage);
        //             break;

        //         case "Berserk":
        //             attacker.mp -= cost;
        //             attacker.isBerserk = true;
        //             attacker.berserkCount = 0;
        //             this.atkText(attacker, attacker.name + " is now Berserk!");
        //             // attacker.berserkCheck();
        //             break;

        //         case "Fireball":
        //             console.log("rand/CritCheck: " + criticalCheck + "/" + luckCheck);
        //             console.log("rand/MissCheck: " + missCheck + "/" + speedCheck);
        //             if (missCheck < speedCheck) {
        //                 attackMessage = "Fire missed...";
        //             } else if (criticalCheck >= luckCheck) {
        //                 defense = Math.floor(defense / 4);
        //                 damage = attacker.mana + Math.floor(attacker.mana * 0.25) - defense;
        //                 if (damage < 1) {
        //                     damage = 1
        //                 }
        //                 attackMessage = "Fire did " + damage + " damage.";
        //             } else {
        //                 damage = attacker.mana + Math.floor(attacker.mana * 0.5);
        //                 attackMessage = "Critical hit! Fire did " + damage + " damage.";
        //             }
        //             defender.hp -= damage;
        //             attacker.mp -= cost;
        //             this.atkText(attacker, attackMessage);

        //             // attacker.berserkCheck();
        //             break;

        //         case "Heal":
        //             luckCheck = attacker.luck + 8;
        //             if (luckCheck > 95) {
        //                 luckCheck = 95;
        //             } else if (luckCheck < 5) {
        //                 luckCheck = 5;
        //             }
        //             console.log("rand/CritCheck: " + criticalCheck + "/" + luckCheck);
        //             if (criticalCheck >= luckCheck) {
        //                 damage = Math.floor(attacker.maxHp * 0.75);
        //                 attackMessage = attacker.name + " recovered " + damage + " HP.";
        //             } else {
        //                 damage = attacker.maxHp;
        //                 attackMessage = "Wow! " + attacker.name + " recovered max HP.";
        //             }
        //             attacker.hp += damage;
        //             if (attacker.hp > attacker.maxHp) {
        //                 attacker.hp = attacker.maxHp;
        //             }
        //             attacker.mp -= cost;
        //             this.atkText(attacker, attackMessage);

        //             // attacker.berserkCheck();
        //             break;

        //         case "Dagger Slash":
        //             console.log("rand/CritCheck: " + criticalCheck + "/" + luckCheck);
        //             console.log("rand/MissCheck: " + missCheck + "/" + speedCheck);
        //             if (missCheck < speedCheck) {
        //                 attackMessage = "Dagger missed...";
        //             } else if (criticalCheck >= luckCheck) {
        //                 defense = Math.floor(defense / 4);
        //                 damage = attacker.strength + Math.floor(attacker.strength * 0.25) - defense;
        //                 if (damage < 1) {
        //                     damage = 1
        //                 }
        //                 attackMessage = "Dagger did " + damage + " damage.";
        //             } else {
        //                 damage = attacker.mana + Math.floor(attacker.strength * 0.5);
        //                 attackMessage = "Critical hit! Dagger did " + damage + " damage.";
        //             }
        //             defender.hp -= damage;
        //             attacker.mp -= cost;
        //             this.atkText(attacker, attackMessage);

        //             // attacker.berserkCheck();
        //             break;

        //         case "Steal":
        //             speedCheck = (attacker.speed - defender.speed) + 60;
        //             if (speedCheck > 95) {
        //                 speedCheck = 95;
        //             } else if (speedCheck < 5) {
        //                 speedCheck = 5;
        //             }
        //             console.log("rand/CritCheck: " + criticalCheck + "/" + speedCheck);
        //             if (criticalCheck <= speedCheck) {
        //                 const itemNum = this.$parent.randNum(0, defender.inventory.length);
        //                 const item = defender.inventory[itemNum];
        //                 this.transferItem(defender.inventory, attacker.inventory, item);
        //                 this.atkText(attacker, attacker.name + " stole " + this.$parent.anA(item.name) + " " + item.name + ".");
        //             } else {
        //                 this.atkText(attacker, attacker.name + " failed to steal anything.");
        //             }

        //             attacker.mp -= cost;
        //             // attacker.berserkCheck();
        //             break;

        //         default:
        //         // code block
        //     };
        // },
        gameOverCheck() {
            if (this.$parent.player.hp <= 0) {
                let text = this.$parent.infoText;
                text.push(this.$parent.currentEnemy.name + " killed you.");
                text.push("--- RESULTS ---");
                text.push("Monsters Killed: " + this.$parent.player.totalKills);
                text.push("Gold Collected: " + this.$parent.player.totalGold);
                text.push("Quests Completed: " + this.$parent.player.totalQuests);
                text.push("Dungeons Completed: " + this.$parent.player.totalDungeons);
                this.setState({
                    message: "Game over.",
                    step: "game over",
                    infoText: text
                });
            }
        },
        selectReset() {
            alert('RESET');
            // this.changePlayStates("Title Screen", "new or load");
            // this.setState({
            //     player: playerDefault,
            //     message: "Start again?"
            // });
        },
        gainXp(xpNum, player) {
            player.xp += xpNum;
            let text = this.$parent.infoText;
            text.push("You gained " + xpNum + " XP.");
            // this.setState({
            //     player: player,
            //     xpResult: xpNum,
            //     infoText: text
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
                player.maxHp += 5;
                player.hp = player.maxHp;
                player.maxMp += 2;
                player.mp = player.maxMp;

                let text = this.$parent.infoText;
                text.push("You are now lv. " + player.level + "!!!");
                if (player.level === 2) {
                    text.push("You learned " + player.special1 + "!!!!!");
                } else if (player.level === 6) {
                    text.push("You learned " + player.special2 + "!!!!!");
                }

                // this.setState({
                //     player: player,
                //     infoText: text
                // });
                this.levelUpCheck(player)
            }
        },
        selectRun() {
            const player = this.$parent.player
            const lostGold = this.$parent.randNum(0, Math.floor(player.gold / 2));
            player.gold -= lostGold;
            const lostHp = this.$parent.randNum(0, 3);
            player.hp -= lostHp;
            this.gameOverCheck();
            this.setState({
                task: "select where",
                step: null,
                movingForward: false,
                message: "You lost " + lostGold + " gold and " + lostHp + " HP.",
            });

        },
        dropGold() {
            const amount = this.$parent.randNum(2, this.$parent.currentEnemy.gold);
            let text = this.$parent.infoText;
            text.push(this.$parent.currentEnemy.name + " dropped " + amount + " gold.")
            let player = this.$parent.player;
            player.gold += amount;
            player.totalGold += amount;
            // this.setState({
            //     player: player,
            //     infoText: text,
            //     goldResult: amount
            // });
        },
        dropLoot(enemy) {
            if (enemy.name === "Mimic") {
                let text = this.$parent.infoText;
                enemy.inventory.forEach(item => {
                    this.transferItem(enemy.inventory, this.$parent.player.inventory, item)
                    text.push(enemy.name + " dropped " + this.$parent.anA(item.name) + " " + item.name + ".");
                });
                // this.setState({
                //     infoText: text
                // })
            } else {
                const lootCheck = this.$parent.randNum(1, 5);
                if (lootCheck === 1) {
                    if (enemy.inventory.length) {
                        const itemNum = this.$parent.randNum(0, enemy.inventory.length);
                        const item = enemy.inventory[itemNum];
                        this.$parent.transferItem(enemy.inventory, this.$parent.player.inventory, item)
                        let text = this.$parent.infoText;
                        text.push(enemy.name + " dropped " + this.$parent.anA(item.name) + " " + item.name + ".");
                        // this.setState({
                        //     infoText: text
                        // })
                    } else {
                        console.log("enemy has no items")
                    }
                }
            }
        }
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
