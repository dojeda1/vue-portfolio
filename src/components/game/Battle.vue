<template>
    <template v-if="task == 'battle'">
        <p>What next?</p>
        <p>
            <button class="btn-blue" @click="handleAttack" :class="{ 'disabled' : !playerTurn}">Attack</button>
        </p>
        <!-- <template v-if="$parent.player.special1.active">
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
        </template> -->
        <template v-for="(special, index) in $parent.player.specials" :key="index">
            <template v-if="special.active">
                <p>
                    <button class="btn-blue" @click="handleSpecial(special)"
                    :class="{ 'disabled' : !playerTurn || special.cost > $parent.player.mp}">
                    {{special.name}} - {{special.cost}} mp</button>
                </p>
            </template>
        </template>
        <p>
            <button class="btn-blue" @click="handleUseItem" :class="{ 'disabled' : !playerTurn}">Use Item</button>
        </p>
        <p>
            <button class="btn-inv" @click="handleRun" :class="{ 'disabled' : !playerTurn}"><i className="material-icons left">arrow_back</i>Run</button>
        </p>
    </template>
    <template v-else-if="task == 'use item'">
        <p>{{$parent.infoText}}</p>
        <div class="items" @mouseleave="$parent.infoText = 'Select an Item'">
            <button class="btn-blue"
            v-for="(item, index) in $parent.player.inventory"
            :key="index"
                :class="{ 'disabled' : !playerTurn}"
                @mouseover="$parent.infoText = item.info"
                @click="handleAttemptItem(item,index)">{{ item.name }} x {{item.qty}}</button>
        </div>
        <p>
            <button class="btn-inv" @click="handleBack"><i class="material-icons left">arrow_back</i>Back</button>
        </p>
    </template>
    <template v-else-if="task == 'next'">
        <p>
            <button class="btn-inv" @click="handleNext">Next</button>
        </p>
    </template>
    <template v-else-if="task == 'end'">
        <p>
            <button class="btn-inv" @click="handleEnd">End</button>
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
            if (this.$parent.location == 'Wild') {
                this.$parent.changeScene('Wild');
            } else if (this.$parent.location == 'Dungeon') {
                if (this.$parent.dungeonCount >= this.$parent.region.dungeonGoal + 1) {
                    this.$parent.message = 'Dungeon Completed.'
                    this.$parent.changeScene('Wild');
                } else {
                    this.$parent.changeScene('Dungeon');
                }
            }
        },
        handleBack() {
            this.task = 'battle';
        },
        handleUseItem() {
            if (this.$parent.player.inventory.length) {
                this.$parent.infoText = 'Select an item'
            } else {
                this.$parent.infoText = 'Inventory empty'
            }
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
                    if (this.$parent.dungeonCount >= this.$parent.region.dungeonGoal + 1) {
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
        handleSpecial($special) {
            const cost = $special.cost
            const name = $special.name;
            console.log("Special: " + name + " " + cost);
            if (name === "Heal" && this.$parent.player.hp >= this.$parent.player.hpMax) {
                this.$parent.message = "You are already at full health."
            } else if (name === "Steal" && !this.$parent.currentEnemy.inventory.length) {
                this.$parent.message = "There is nothing to steal."
            } else if (name === "Berserk" && this.$parent.player.status.includes('Berserk')) {
                this.$parent.message = "You are already Berserk."
            } else {
                this.playerTurn = false;
                let player = this.$parent.player;
                let enemy = this.$parent.currentEnemy;
                this.$parent.messageBox = [];
                let $this = this;
                // $this.attack(player, enemy)
                $this.special(player, enemy, $special);
                setTimeout(function() {
                    $this.enemyTurn(player, enemy);
                }, 1200);
                //
            }
        },
        attack(attacker, defender) {
            attacker.animation = 'attack'
            let berserkNum = 0;
            let berserkMessage;
            if (attacker.status.includes('Berserk')) {
                const berserk = this.$parent.returnSpecial(attacker.specials,'Berserk')
                berserk.count--
                berserkNum = Math.ceil(attacker.strength / 2);
                if (berserk.count <= 0) {
                    attacker.status.splice(attacker.status.indexOf('Berserk'), 1);
                    console.log(" - Berserk has run out. - \n");
                    berserkMessage = '- Berserk has run out. -'
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
                console.log('S:',attacker.strength,'B:',berserkNum,'-D:',defense,"=",damage)
                if (damage < 1) {
                    damage = 1;
                }
                attackMessage = attacker.name + " did " + damage + " damage.";
            } else {
                setTimeout(function() {
                    defender.animation = 'damage'
                },300);
                damage = attacker.strength + Math.ceil(attacker.strength * 0.25) + berserkNum;
                console.log('S:',attacker.strength,'X:',Math.ceil(attacker.strength * 0.25),'B:',berserkNum,"=",damage)
                attackMessage = "Critical hit! " + attacker.name + " did " + damage + " damage.";
            }
            defender.hp -= damage;
            this.$parent.messageBox.push(attackMessage);
            berserkMessage ? this.$parent.messageBox.push(berserkMessage) : null;
            console.log(attackMessage);
            // this.atkText(attacker, attackMessage);
            // attacker.berserkCheck();
        },
        special (attacker, defender, $special) {
            const cost = $special.cost
            const name = $special.name;
            let attackMessage;
            let damage = 0;
            let defense = defender.defense;
            let criticalCheck = this.$parent.randNum(1, 100);
            let missCheck = this.$parent.randNum(1, 100);
            let luckCheck = (attacker.luck - defender.luck) + 10;
            let special;
            let berserkNum = 0;
            let berserkMessage;
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
                    if (attacker.status.includes('Berserk')) {
                        const berserk = this.$parent.returnSpecial(attacker.specials,'Berserk')
                        berserk.count--
                        berserkNum = Math.ceil(attacker.strength / 2);
                        if (berserk.count <= 0) {
                            attacker.status.splice(attacker.status.indexOf('Berserk'), 1);
                            console.log(" - Berserk has run out. - \n");
                            berserkMessage = '- Berserk has run out. -'
                        }
                    }
                    if (missCheck < speedCheck) {
                        setTimeout(function() {
                            defender.animation = 'dodge'
                        },300);
                        attackMessage = attacker.name + "'s Axe missed...";
                    } else if (criticalCheck >= luckCheck) {
                        setTimeout(function() {
                            defender.animation = 'damage'
                        },300);
                        defense = Math.floor(defense / 4);
                        damage = attacker.strength + Math.ceil(attacker.strength * 0.25) + berserkNum - defense;
                        console.log('S:',attacker.strength,'X:',Math.ceil(attacker.strength * 0.25),'B:',berserkNum,'-D:',defense,"=",damage)
                        if (damage < 1) {
                            damage = 1;
                        }
                        attackMessage = attacker.name + "'s Axe did " + damage + " damage.";
                    } else {
                        setTimeout(function() {
                            defender.animation = 'damage'
                        },300);
                        damage = attacker.strength + Math.ceil(attacker.strength * 0.5) + berserkNum;
                        console.log('S:',attacker.strength,'X:',Math.ceil(attacker.strength * 0.5),'B:',berserkNum,"=",damage)
                        attackMessage = "Critical hit! " + attacker.name + "'s Axe did " + damage + " damage.";
                    }
                    defender.hp -= damage;
                    attacker.mp -= cost;
                    console.log('message in a bottle:',berserkMessage)
                    this.$parent.messageBox.push(attackMessage);
                    berserkMessage ? this.$parent.messageBox.push(berserkMessage) : null;
                    console.log(attackMessage);
                    break;

                case "Berserk":
                    attacker.animation = 'buffer'
                    attacker.mp -= cost;
                    special = this.$parent.returnSpecial(attacker.specials,'Berserk')
                    console.log('special:',special);
                    special.count = 3;
                    attacker.status.push('Berserk')
                    this.$parent.messageBox.push(attacker.name + " is now Berserk!");
                    break;

                case "Fireball":
                    attacker.animation = 'attack'
                    console.log("rand/CritCheck: " + criticalCheck + "/" + luckCheck);
                    console.log("rand/MissCheck: " + missCheck + "/" + speedCheck);
                    if (attacker.status.includes('Berserk')) {
                        const berserk = this.$parent.returnSpecial(attacker.specials,'Berserk')
                        berserk.count--
                        berserkNum = Math.ceil(attacker.strength / 2);
                        if (berserk.count <= 0) {
                            attacker.status.splice(attacker.status.indexOf('Berserk'), 1);
                            console.log(" - Berserk has run out. - \n");
                            berserkMessage = '- Berserk has run out. -'
                        }
                    }
                    if (missCheck < speedCheck) {
                        setTimeout(function() {
                            defender.animation = 'dodge'
                        },300);
                        attackMessage = attacker.name + "'s Fire missed...";
                    } else if (criticalCheck >= luckCheck) {
                        setTimeout(function() {
                            defender.animation = 'damage'
                        },300);
                        defense = Math.floor(defense / 4);
                        damage = attacker.mana + Math.ceil(attacker.mana * 0.25) + berserkNum - defense;
                        console.log('S:',attacker.mana,'X:',Math.ceil(attacker.mana * 0.25),'B:',berserkNum,'-D:',defense,"=",damage)
                        if (damage < 1) {
                            damage = 1;
                        }
                        attackMessage = attacker.name + "'s Fire did " + damage + " damage.";
                    } else {
                        setTimeout(function() {
                            defender.animation = 'damage'
                        },300);
                        damage = attacker.mana + Math.ceil(attacker.mana * 0.5) + berserkNum;
                        console.log('S:',attacker.mana,'X:',Math.ceil(attacker.mana * 0.25),'B:',berserkNum,'-D:',defense,"=",damage)
                        attackMessage = "Critical hit! " + attacker.name + "'s Fire did " + damage + " damage.";
                    }
                    defender.hp -= damage;
                    attacker.mp -= cost;
                    this.$parent.messageBox.push(attackMessage);
                    berserkMessage ? this.$parent.messageBox.push(berserkMessage) : null;
                    console.log(attackMessage);
                    break;

                case "Heal":
                    attacker.animation = 'buffer'
                    luckCheck = attacker.luck + 8;
                    if (luckCheck > 95) {
                        luckCheck = 95;
                    } else if (luckCheck < 5) {
                        luckCheck = 5;
                    }
                    console.log("rand/CritCheck: " + criticalCheck + "/" + luckCheck);
                    if (criticalCheck >= luckCheck) {
                        damage = Math.ceil(attacker.hpMax * 0.75);
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
                    this.$parent.messageBox.push(attackMessage);
                    console.log(attackMessage);
                    break;

                case "Dagger Slash":
                    attacker.animation = 'attack'
                    console.log("rand/CritCheck: " + criticalCheck + "/" + luckCheck);
                    console.log("rand/MissCheck: " + missCheck + "/" + speedCheck);
                    if (attacker.status.includes('Berserk')) {
                        const berserk = this.$parent.returnSpecial(attacker.specials,'Berserk')
                        berserk.count--
                        berserkNum = Math.ceil(attacker.strength / 2);
                        if (berserk.count <= 0) {
                            attacker.status.splice(attacker.status.indexOf('Berserk'), 1);
                            console.log(" - Berserk has run out. - \n");
                            berserkMessage = '- Berserk has run out. -'
                        }
                    }
                    if (missCheck < speedCheck) {
                        setTimeout(function() {
                            defender.animation = 'dodge'
                        },300);
                        attackMessage = attacker.name + "'s Dagger missed...";
                    } else if (criticalCheck >= luckCheck) {
                        setTimeout(function() {
                            defender.animation = 'damage'
                        },300);
                        defense = Math.floor(defense / 4);
                        damage = attacker.strength + Math.ceil(attacker.strength * 0.25) + berserkNum - defense;
                        console.log('S:',attacker.strength,'X:',Math.ceil(attacker.strength * 0.25),'B:',berserkNum,'-D:',defense,"=",damage)
                        if (damage < 1) {
                            damage = 1;
                        }
                        attackMessage = attacker.name + "'s Dagger did " + damage + " damage.";
                    } else {
                        setTimeout(function() {
                            defender.animation = 'damage'
                        },300);
                        damage = attacker.strength + Math.ceil(attacker.strength * 0.5) + berserkNum;
                        attackMessage = "Critical hit! " + attacker.name + "'s Dagger did " + damage + " damage.";
                    }
                    defender.hp -= damage;
                    attacker.mp -= cost;
                    this.$parent.messageBox.push(attackMessage);
                    berserkMessage ? this.$parent.messageBox.push(berserkMessage) : null;
                    console.log(attackMessage);
                    break;

                case "Steal":
                    attacker.animation = 'attack'
                    speedCheck = (attacker.speed - defender.speed) + 60;
                    if (speedCheck > 95) {
                        speedCheck = 95;
                    } else if (speedCheck < 5) {
                        speedCheck = 5;
                    }
                    console.log("rand/CritCheck: " + criticalCheck + "/" + speedCheck);
                    if (criticalCheck <= speedCheck) {
                        setTimeout(function() {
                            defender.animation = 'damage'
                        },300);
                        const itemNum = this.$parent.randNum(0, defender.inventory.length);
                        const item = defender.inventory[itemNum];
                        this.$parent.transferItem(defender.inventory, attacker.inventory, item);
                        attackMessage = attacker.name + " stole " + this.$parent.anA(item.name) + " " + item.name + "."
                        this.$parent.messageBox.push(attackMessage);
                        console.log(attackMessage);
                    } else {
                        setTimeout(function() {
                            defender.animation = 'dodge'
                        },300);
                        attackMessage = attacker.name + " failed to steal anything."
                        this.$parent.messageBox.push(attackMessage);
                        console.log(attackMessage);
                    }

                    attacker.mp -= cost;
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
                this.$parent.message = 'Game Over.';
                this.task = 'end';
            }
            console.log('player:',this.$parent.player);
        },
        handleEnd() {
            this.$parent.changeScene('TitleScreen');
        },
        gainXp(xpNum, player) {
            player.xp += xpNum;
            let text = this.$parent.messageBox;
            text.push("You gained " + xpNum + " XP.");
            this.levelUpCheck(this.$parent.player);
        },
        levelUpCheck(player) {
            if (player.xp >= player.nextLevel) {
                player.level++;
                player.nextLevel += player.level * 30;
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
                text.push("You are now lv. " + player.level + "!");
                if (player.level == 2) {
                    player.specials[0].active = true;
                    text.push("You learned " + player.specials[0].name + "!!!");
                } else if (player.level == 3) {
                    player.specials[1].active = true;
                    text.push("You learned " + player.specials[1].name + "!!!");
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
            if (this.$parent.location == 'Wild') {
                this.$parent.changeScene('Wild');
            } else if (this.$parent.location == 'Dungeon') {
                this.$parent.changeScene('Dungeon');
            }
            this.gameOverCheck();
            this.$parent.message = "You lost " + lostGold + " gold and " + lostHp + " HP.";
            this.$parent.messageBox = []
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
            let text = this.$parent.messageBox;
            if (enemy.name === "Mimic") {
                if (enemy.inventory.length) {
                    enemy.inventory.forEach(item => {
                        this.$parent.transferItem(enemy.inventory, this.$parent.player.inventory, item)
                        text.push(enemy.name + " dropped " + this.$parent.anA(item.name) + " " + item.name + ".");
                    });
                } else {
                    console.log("enemy has no items")
                }
            } else {
                const lootCheck = this.$parent.randNum(1, 5);
                if (lootCheck === 1) {
                    if (enemy.inventory.length) {
                        const itemNum = this.$parent.randNum(0, enemy.inventory.length);
                        const item = enemy.inventory[itemNum];
                        this.$parent.transferItem(enemy.inventory, this.$parent.player.inventory, item)
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

</style>
