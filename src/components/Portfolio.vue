<template>
<div id="portfolio">
    <div class="section-title">
        <h4 class=" dom-green-1-text">Portfolio</h4>
    </div>
    <div class="container">
        <div class="section-header">
            <h5 class=" dom-green-1-text">Project Summary</h5>
        </div>
        <div class="grid">
            <div class="col-2">
                <h5>Wizard Words</h5>
                <img class="portfolio-devices" src="/images/wizard-words/wizard-words-mockup.png" alt="here">
                <button>VISIT 0</button>
                <button>CODE 0</button>
            </div>
            <div class="col-1">
                <h6 class=" dom-green-1-text">- About the Project -</h6>
                <p>
                    This version of the traditional Hangman game is fashioned after the wizarding world of Harry Potter. The game displays what letters you have previously guessed, how many guesses you have left, as well as your wins and losses. The design was inspired by the Marauder's Map and uses jQuery for a number of cool fade-in and fade-out animations.
                </p>
                <h6 class=" dom-green-1-text">- Made Using -</h6>
                <p>
                    HTML, CSS, JavaScript, jQuery, Bootstrap
                </p>
            </div>
            <div class="col-1">
                <img class="portfolio-devices" src="/images/wizard-words/wizard-words-guess.jpg" alt="here">
            </div>
            <div class="col-2 row-2">
                <img class="portfolio-devices" src="/images/wizard-words/wizard-words-win.jpg" alt="here">
            </div>
            <div class="col-1">
                <img class="portfolio-devices" src="/images/wizard-words/wizard-words-lose.jpg" alt="here">
            </div>
        </div>
        <div class="section-header">
            <h5 class=" dom-green-1-text">Select a Project</h5>
        </div>
    </div>
</div>
</template>

<script>
export default {
    name: 'Portfolio'
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>

</style>
