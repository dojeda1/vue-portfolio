<template>
    <template v-if="task == 'bet'">
        <p>How much do you want to bet?</p>
        <p class="quad-buttons">
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn}" @click="handleChangeGold(-10)">-10</button>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn}" @click="handleChangeGold(-5)">-5</button>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn}" @click="handleChangeGold(5)">+5</button>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn}" @click="handleChangeGold(10)">+10</button>
        </p>
        <p class="full-buttons">
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn}" @click="handleBet">Bet ({{this.gold}}g)</button>
        </p>
        <p class="dual-buttons">
            <button class="btn-inv" :class="{ 'disabled' : !playerTurn}" @click="handleLeave"><i class="material-icons left">arrow_back</i>Back</button>
        </p>
    </template>
    <template v-else-if="task == 'play'">
        <div class="word-guess">
            <h5 class="letters">
                <span v-for="(letter,index) in currentWord" :key="index">
                    <template v-if="letter.guessed">{{letter.letter}}</template>
                    <template v-else>_</template>
                </span>
            </h5>
            <!-- <p class="text-blue">{{correctLetters.join(' ')}}</p> -->
            <p class="text-red">{{wrongLetters.join(' ')}}</p>
            <p>Guesses Left: {{guessesLeft}}</p>
        </div>
        <p></p>
        <p>Guess the word.</p>
        <p class="six-buttons">
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn || checkButton('A')}" @click="handleGuess('A')">A</button>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn || checkButton('B')}" @click="handleGuess('B')">B</button>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn || checkButton('C')}" @click="handleGuess('C')">C</button>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn || checkButton('D')}" @click="handleGuess('D')">D</button>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn || checkButton('E')}" @click="handleGuess('E')">E</button>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn || checkButton('F')}" @click="handleGuess('F')">F</button>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn || checkButton('G')}" @click="handleGuess('G')">G</button>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn || checkButton('H')}" @click="handleGuess('H')">H</button>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn || checkButton('I')}" @click="handleGuess('I')">I</button>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn || checkButton('J')}" @click="handleGuess('J')">J</button>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn || checkButton('K')}" @click="handleGuess('K')">K</button>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn || checkButton('L')}" @click="handleGuess('L')">L</button>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn || checkButton('M')}" @click="handleGuess('M')">M</button>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn || checkButton('N')}" @click="handleGuess('N')">N</button>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn || checkButton('O')}" @click="handleGuess('O')">O</button>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn || checkButton('P')}" @click="handleGuess('P')">P</button>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn || checkButton('Q')}" @click="handleGuess('Q')">Q</button>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn || checkButton('R')}" @click="handleGuess('R')">R</button>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn || checkButton('S')}" @click="handleGuess('S')">S</button>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn || checkButton('T')}" @click="handleGuess('T')">T</button>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn || checkButton('U')}" @click="handleGuess('U')">U</button>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn || checkButton('V')}" @click="handleGuess('V')">V</button>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn || checkButton('W')}" @click="handleGuess('W')">W</button>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn || checkButton('X')}" @click="handleGuess('X')">X</button>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn || checkButton('Y')}" @click="handleGuess('Y')">Y</button>
            <button class="btn-blue" :class="{ 'disabled' : !playerTurn || checkButton('Z')}" @click="handleGuess('Z')">Z</button>
        </p>
        <p class="dual-buttons">
            <button class="btn-inv" :class="{ 'disabled' : !playerTurn}" @click="handleGiveUp"><i class="material-icons left">arrow_back</i>Give Up</button>
        </p>
    </template>
</template>

<script>
//JSON Files
import words from './data/words.json';

export default {
    name: 'WordGuess',
    data() {
        return {
            task: 'bet',
            playerTurn: true,
            gold: 10,
            guessesLeft: 0,
            correctLetters: [],
            wrongLetters: [],
            currentWord: []
        }
    },
    methods: {
        log(msg) {
            console.log('Log:',msg);
        },
        handleGuess(guess) {
            console.log('Guess:',guess);
            let isCorrect = false
            this.currentWord.forEach(function(ltr) {
                if (guess == ltr.letter.toUpperCase()) {
                    ltr.guessed = true;
                    isCorrect = true;
                }
            });
            if (isCorrect) {
                if (!this.correctLetters.includes(guess)) {
                    this.correctLetters.push(guess)
                }
            } else {
                if (!this.wrongLetters.includes(guess)) {
                    this.guessesLeft--
                    this.wrongLetters.push(guess)
                }
            }
            this.checkWin();
        },
        handleChangeGold(g) {
            this.gold+=g
            if (this.gold < 5) {
                this.gold = 5;
            }
        },
        handleBet() {
            if (this.$parent.player.gold >= this.gold) {
                this.$parent.message = 'You bet ' + this.gold + 'g.'
                this.task = 'play';
                this.newWord();
            } else {
                this.$parent.message = "You don't have enough gold."
                
            }
        },
        handleLeave() {
            this.$parent.message = "You decided against it."
            this.$parent.messageBox = [];
            this.$parent.changeScene('Tavern');
        },
        handleGiveUp() {
            this.$parent.player.gold-=this.gold;
            this.$parent.message = "You gave up " + this.gold + 'g...'
            this.playerTurn = false;
            this.$parent.player.animation = 'damage';
            this.$parent.note(this.$parent.player,-this.gold);
            this.$parent.currentEncounter.animation = 'buffer';
            this.$parent.note(this.$parent.currentEncounter,this.gold);
            const $this = this;
            setTimeout(function() {
                $this.$parent.player.animation = 'idle';
                $this.$parent.currentEncounter.animation = 'idle';
                $this.task = 'bet';
                $this.playerTurn = true;
            },1200)
        },
        checkButton(letter) {
            if (this.correctLetters.includes(letter) || this.wrongLetters.includes(letter)) {
                return true;
            } else {
                return false;
            }
        },
        checkWin() {
            let passedCheck = true;
            this.currentWord.forEach(function(ltr) {
                if (!ltr.guessed) {
                    passedCheck = false;
                }
            });
            if (passedCheck) {
                this.$parent.player.gold+=this.gold;
                this.$parent.message = 'You won ' + this.gold + 'g!'
                this.playerTurn = false;
                this.$parent.player.animation = 'buffer';
                this.$parent.note(this.$parent.player,this.gold);
                this.$parent.currentEncounter.animation = 'damage';
                this.$parent.note(this.$parent.currentEncounter,-this.gold);
                const $this = this;
                setTimeout(function() {
                    $this.$parent.player.animation = 'idle';
                    $this.$parent.currentEncounter.animation = 'idle';
                    $this.task = 'bet';
                    $this.playerTurn = true;
                }, 1200)
            } else if(this.guessesLeft <= 0) {
                this.$parent.player.gold-=this.gold;
                this.$parent.message = "You lost " + this.gold + 'g...'
                this.playerTurn = false;
                this.$parent.player.animation = 'damage';
                this.$parent.note(this.$parent.player,-this.gold);
                this.$parent.currentEncounter.animation = 'buffer';
                this.$parent.note(this.$parent.currentEncounter,this.gold);
                const $this = this;
                setTimeout(function() {
                    $this.$parent.player.animation = 'idle';
                    $this.$parent.currentEncounter.animation = 'idle';
                    $this.task = 'bet';
                    $this.playerTurn = true;
                },1200)
            }
        },
        newWord() {
            this.correctLetters = [];
            this.wrongLetters = [];
            if (this.gold >= 200) {
                this.guessesLeft = 2
            } else if (this.gold >= 100) {
                this.guessesLeft = 3
            } else if(this.gold >= 50) {
                this.guessesLeft = 4
            } else if(this.gold >= 10) {
                this.guessesLeft = 5
            } else {
                this.guessesLeft = 6
            }
            const wordStr = words[this.$parent.randNum(0,words.length)];
            const wordObj = [];
            for (let i = 0; i < wordStr.length; i++) {
                const isSymbol = !wordStr[i].match(/[a-z]/i);
                wordObj.push({
                    letter: wordStr[i],
                    guessed: isSymbol
                })
            }
            this.currentWord = wordObj;
        }
    },
    created: function() {
        // this.$parent.location = 'Wild';
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
    .letters {
        letter-spacing: 10px;
    }
    .word-guess {
        border: 2px solid gray;
        border-radius: 10px;
        padding: 10px;
        margin-top: 10px;
    }
</style>
