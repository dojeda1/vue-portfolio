<template>
<div id="portfolio">
    <div class="section-title">
        <h4 class=" dom-green-1-text">Portfolio</h4>
    </div>
    <div class="container">
        <div class="section-header">
            <h5 class=" dom-green-1-text">Select a Project</h5>
        </div>
        <div class="paint-pics">
            <div class="paint-pic-container"
            :class="{ 'active': project.isActive}"
            @click="swapProject(index)"
            v-for="(project, index) in projects"
            :key="index">
                <img class="paint-pic" :src="project.paint" alt="Painted Preview">
                <p>{{ project.title }}</p>
            </div>
        </div>
        <div class="section-header">
            <h5 class=" dom-green-1-text">Project Summary</h5>
        </div>
        <div class="grid">
            <div class="col-8">
                <h5>{{ currentProject.title }}</h5>
                <img class="portfolio-devices" :src="currentProject.mockup" alt="Mockup">
                <a :href="currentProject.visit">
                    <button>VISIT 0</button>
                </a>
                <a :href="currentProject.code">
                    <button>CODE 0</button>
                </a>
            </div>
            <div class="col-4">
                <h6 class=" dom-green-1-text">- About the Project -</h6>
                <p>{{ currentProject.sum }}</p>
                <h6 class=" dom-green-1-text">- Made Using -</h6>
                <p>{{ currentProject.list.join(', ') }}</p>
            </div>
            <div class="col-4">
                <img class="preview-img" alt="Screenshot 1"
                :class="{'zoomed': isZoomed[0]}"
                :src="currentProject.img1"
                @click="zoomImage(0)">
            </div>
            <div class="col-8 row-2">
                <img class="preview-img" alt="Screenshot 2"
                :class="{'zoomed': isZoomed[1]}"
                :src="currentProject.img2"
                @click="zoomImage(1)">
            </div>
            <div class="col-4">
                <img class="preview-img" alt="Screenshot 3"
                :class="{'zoomed': isZoomed[2]}"
                :src="currentProject.img3"
                @click="zoomImage(2)">
            </div>
        </div>
    </div>
</div>
</template>

<script>
export default {
    name: 'Portfolio',
    data() {
        return {
        currentProject: {},
        isZoomed: [
            false,
            false,
            false
        ],
        projects: [
            {
                title: "Meal Planner",
                mockup: "/images/meal-planner/meal-planner-mockup.png",
                paint: "/images/meal-planner/meal-planner-paint.png",
                img1: "/images/meal-planner/meal-planner-search.jpg",
                img2: "/images/meal-planner/meal-planner-fav.jpg",
                img3: "/images/meal-planner/meal-planner-cal.jpg",
                visit: "https://stephanfalcon.github.io/PlusUltraProject/",
                code: "https://github.com/stephanfalcon/PlusUltraProject",
                sum: "Using the Spoonacular API, Meal Planner allows you to look up recipes with optional dietary restrictions and save them to your favorites. You can view recipe cards to see a meal's ingredients and instructions. From the Calendar page, you can choose any recipe from your favorites, add it to a meal period on your weekly calendar, and save the plan for later. Both the favorites and the weekly meal plan are stored using Firebase's realtime database.",
                list: ["HTML", "CSS", "JavaScript", "jQuery", "Materialize", "Firebase", "Spoonacular API", "Heal Thru Words API"],
                isActive: true
            },
            {
                title: "Wizard Words",
                mockup: "/images/wizard-words/wizard-words-mockup.png",
                paint: "/images/wizard-words/wizard-words-paint.png",
                img1: "/images/wizard-words/wizard-words-guess.jpg",
                img2: "/images/wizard-words/wizard-words-lose.jpg",
                img3: "/images/wizard-words/wizard-words-win.jpg",
                visit: "https://dojeda1.github.io/Word-Guess-Game/",
                code: "https://github.com/dojeda1/Word-Guess-Game",
                sum: "This version of the traditional Hangman game is fashioned after the wizarding world of Harry Potter. The game displays what letters you have previously guessed, how many guesses you have left, as well as your wins and losses. The design was inspired by the Marauder's Map and uses jQuery for a number of cool fade-in and fade-out animations.",
                list: ["HTML", "CSS", "JavaScript", "jQuery", "Bootstrap"],
                isActive: false
            },
            {
                title: "Sci-fi RPG",
                mockup: "/images/scifi-rpg/scifi-rpg-mockup.png",
                paint: "/images/scifi-rpg/scifi-rpg-paint.png",
                img1: "/images/scifi-rpg/scifi-rpg-lose.jpg",
                img2: "/images/scifi-rpg/scifi-rpg-win.jpg",
                img3: "/images/scifi-rpg/scifi-rpg-fight.jpg",
                visit: "https://dojeda1.github.io/Space-RPG-Game/",
                code: "https://github.com/dojeda1/Space-RPG-Game",
                sum: "Choose one of 4 classic Sci-fi characters to play as and try to defeat all of the remaining opponents. Each character has different health, strength, and leveling up stats and you must choose defenders in particular orders to obtain victory.",
                list: ["HTML", "CSS", "JavaScript", "jQuery", "Bootstrap"],
                isActive: false
            },
            {
                title: "Trial of Socrates",
                mockup: "/images/socrates/socrates-mockup.png",
                paint: "/images/socrates/socrates-paint.png",
                img1: "/images/socrates/socrates-prompt.jpg",
                img2: "/images/socrates/socrates-results.jpg",
                img3: "/images/socrates/socrates-choices.jpg",
                visit: "https://dojeda1.github.io/Socrates-Game/",
                code: "https://github.com/dojeda1/Socrates-Game",
                sum: "This is a text based adventure game inspired by the ancient tale of when Socrates was sentenced to death by a jury of his fellow Athenians. Playing as the philosopher himself, you make branching choices powered by IF/ELSE functions in JavaScript that lead to alternate endings. The goal is to find the historical ending or simply explore alternate timelines. ",
                list: ["HTML", "CSS", "JavaScript", "jQuery", "Bootstrap"],
                isActive: false
            },
            {
                title: "Trivia Game",
                mockup: "/images/trivia-game/trivia-mockup.png",
                paint: "/images/trivia-game/trivia-paint.png",
                img1: "/images/trivia-game/trivia-results.jpg",
                img2: "/images/trivia-game/trivia-start.jpg",
                img3: "/images/trivia-game/trivia-results.jpg",
                visit: "https://dojeda1.github.io/TriviaGame/",
                code: "https://github.com/dojeda1/TriviaGame",
                sum: "Test your knowledge of both the metric and imperial units of measurement. Each question is timed and will move onto the next if left unanswered. After each question, a fun GIF is briefly displayed according to whether or not you were correct. At the end of the game, the number of right, wrong, and unanswered responses is displayed along side your overall score. Your high scores are also shown at the bottom of the results page.",
                list: ["HTML", "CSS", "JavaScript", "jQuery", "Bootstrap"],
                isActive: false
            },
            {
                title: "Fur Butlr",
                mockup: "/images/fur-butlr/fur-butlr-mockup.png",
                paint: "/images/fur-butlr/fur-butlr-paint.png",
                img1: "/images/fur-butlr/fur-butlr-results.jpg",
                img2: "/images/fur-butlr/fur-butlr-update.jpg",
                img3: "/images/fur-butlr/fur-butlr-profile.jpg",
                visit: "https://fur-butlr-app.herokuapp.com/",
                code: "https://github.com/ApexPanda/FurButler",
                sum: "A place where pet owners can meet each other and search for pet services like walkers, groomers or sitters. Fur Butlr lets you create a profile, login in, edit your page, and show off your pets.",
                list: ["HTML", "CSS", "JavaScript", "jQuery", "Materialize", "MySQL", "Node.js", "Sequelize"],
                isActive: false
            },
            {
                title: "Bug Memory",
                mockup: "/images/bug-memory/bug-memory-mockup.png",
                paint: "/images/bug-memory/bug-memory-paint.png",
                img1: "/images/bug-memory/bug-memory-win.jpg",
                img2: "/images/bug-memory/bug-memory-lose.jpg",
                img3: "/images/bug-memory/bug-memory-cards.jpg",
                visit: "https://memory-game-react-click.herokuapp.com/",
                code: "https://github.com/dojeda1/memory-game",
                sum: "Test your memory by selecting every bug card without choosing the same one twice. Each time one is chosen, the game will shuffle the cards and display them in a random order using REACT. If you select the same bug twice, you lose!",
                list: ["HTML", "CSS", "JavaScript", "Bootstrap", "React.js", "Node.js"],
                isActive: false
            },
            {
                title: "Book Finder",
                mockup: "/images/book-finder/book-finder-mockup.png",
                paint: "/images/book-finder/book-finder-paint.png",
                img1: "/images/book-finder/book-finder-home.jpg",
                img2: "/images/book-finder/book-finder-results.jpg",
                img3: "/images/book-finder/book-finder-saved.jpg",
                visit: "https://google-books-mern.herokuapp.com/",
                code: "https://github.com/dojeda1/book-search",
                sum: "Search through a large database of books with the help of the Google Books API. User inputs the title of a book and results are displayed below. They can then visit the google URL, save the book in a MongoDB database for later, or delete it from saved books.",
                list: ["HTML", "CSS", "JavaScript", "React.js", "MongoDB", "Mongoose", "Google Books API"],
                isActive: false
            }
        ]
        }
    },
    methods: {
        log(msg) {
            console.log('Log:',msg);
        },
        swapProject(ind) {
            console.log(ind)
            console.log(this.projects[ind])
            this.currentProject = this.projects[ind];
            this.projects.forEach(project => project.isActive = false)
            this.projects[ind].isActive = true;
        },
        zoomImage(ind) {
            console.log('hey');
            this.isZoomed[ind] = !this.isZoomed[ind];
        }
    },
    created: function() {
        this.swapProject(0);
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
    .preview-img {
        width: 100%;
        transition: all .2s ease-in-out;
        box-shadow: 0 4px 5px 0 rgb(0 0 0 / 14%), 0 1px 10px 0 rgb(0 0 0 / 12%), 0 2px 4px -1px rgb(0 0 0 / 30%);
        cursor: zoom-in;
    }
    .preview-img:hover {
        opacity: .8;
    }
    .preview-img.zoomed {
        position: fixed;
        top: 0;
        left: 0;
        cursor: zoom-out;
    }
    .preview-img.zoomed:hover {
        opacity: 1;
    }
    .portfolio-devices {
    width: 100%;
    }
    .paint-pics {
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;
        justify-content: center;
    }
    .paint-pic-container {
        width: 200px;
        text-align: center;
        padding: 10px;
        cursor: pointer;
    }
    .paint-pic {
        width: 100%;
        transition: all .2s ease-in-out;
    }
    .paint-pic-container:hover .paint-pic{
        transform: translateY(-5px);
        transform: scale(1.1);
    }
    .paint-pic-container.active {
        color: #8dc63f;
    }
</style>
