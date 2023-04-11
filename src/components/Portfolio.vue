<template>
<div id="portfolio">
    <div class="section-title">
        <h4 class=" text-green">Portfolio</h4>
    </div>
    <div class="container">
        <div class="section-header">
            <h5 class=" text-green">Select a Project</h5>
        </div>
            <!-- @ready="logEvents('ready', $event)"
            @previous="logEvents('previous', $event)"
            @next="logEvents('next', $event)"
            @before-slide="logEvents('before-slide', $event)" -->
        <div class="paint-pics">
            <vueper-slides class="no-shadow"
            ref="projectVueperSlides"
            @slide="logEvents('slide', $event)"
            :touchable="false"
            :infinite="true"
            :visible-slides="5"
            :gap="3"
            :slide-ratio="1 / 5"
            :dragging-distance="20"
            :breakpoints="{
                1000: {
                    visibleSlides: 3,
                    slideRatio: 1 / 3
                },
                600: {
                    visibleSlides: 1,
                    slideRatio: 1,
                    touchable: true,
                    arrows: false,
                } 
            }">
                <vueper-slide v-for="(slide, index) in projects" :key="index"
                class="paint-pic-container"
                :class="{ 'active': slide.isActive}"
                @click="$refs.projectVueperSlides.goToSlide(index)"
                :content="createSlide(slide)"
                >
                
                </vueper-slide>
            </vueper-slides>
        </div>
        <div class="section-header">
            <h5 class=" text-green">Project Summary</h5>
        </div>
        <div class="grid">
            <div class="col-8 s_col-12">
                <h5>{{ currentProject.title }}</h5>
                <img class="portfolio-devices" :src="currentProject.mockup" alt="Mockup">
                <div class="dual-buttons">
                <a target="_blank" :href="currentProject.visit">
                    <button class="btn">Visit<i className="material-icons right">public</i></button>
                </a>
                <a target="_blank" :href="currentProject.code">
                    <button class="btn">Code<i className="material-icons right">code</i></button>
                </a>
                </div>
            </div>
            <div class="col-4 s_col-12">
                <h6 class=" text-green">- About the Project -</h6>
                <p>{{ currentProject.sum }}</p>
                <h6 class=" text-green">- Made Using -</h6>
                <p>{{ currentProject.list.join(' &#8226; ') }}</p>
            </div>
            <div class="col-4 s_col-12">
                <img class="preview-img" alt="Screenshot 1"
                :src="currentProject.img1"
                @click="zoomImage(currentProject.img1)">
            </div>
            <div class="col-8 row-2 s_col-12">
                <img class="preview-img" alt="Screenshot 2"
                :src="currentProject.img2"
                @click="zoomImage(currentProject.img2)">
            </div>
            <div class="col-4 s_col-12">
                <img class="preview-img" alt="Screenshot 3"
                :src="currentProject.img3"
                @click="zoomImage(currentProject.img3)">
            </div>
        </div>
    </div>
    <div v-if="imageModal != ''" class="image-modal"
    @click="zoomImage('')">
        <img :src="imageModal" alt="zoomed image">
    </div>
</div>
</template>

<script>
// In your Vue.js component.
import { VueperSlides, VueperSlide } from 'vueperslides'
import 'vueperslides/dist/vueperslides.css'

export default {
    name: 'Portfolio',
    components: { VueperSlides, VueperSlide },
    data() {
        return {
            currentProject: {},
            imageModal: '',
            slides: [
                {
                title: 'Slide #1',
                content: 'Slide content.'
                },
                {
                title: 'Slide #2',
                content: 'Slide content.'
                },
                {
                title: 'Slide #3',
                content: 'Slide content.'
                },
            ],
            projects: [
                {
                    title: "Meal Planner",
                    mockup: "/images/meal-planner/meal-planner-mock.png",
                    paint: "/images/meal-planner/meal-planner-paint.png",
                    img1: "/images/meal-planner/meal-planner-preview-1.jpg",
                    img2: "/images/meal-planner/meal-planner-preview-2.jpg",
                    img3: "/images/meal-planner/meal-planner-preview-3.jpg",
                    visit: "https://vue-meal-planner-dojeda1.vercel.app/",
                    code: "https://github.com/dojeda1/vue-meal-planner",
                    sum: "Using the Spoonacular API, Meal Planner allows you to look up recipes with optional dietary restrictions and save them to your favorites. You can view recipe cards to see a meal's ingredients and instructions. From the Calendar page, you can choose any recipe from your favorites, add it to a meal period on your weekly calendar, and save the plan for later. Both the favorites and the weekly meal plan are stored using Firebase's Cloud Firestore database.",
                    list: ["HTML", "CSS", "JavaScript", "Vue.js", "Materialize", "Firebase", "Spoonacular API", "Heal Thru Words API"],
                    isActive: true
                },
                {
                    title: "Bug Memory",
                    mockup: "/images/bug-memory/bug-memory-mock.png",
                    paint: "/images/bug-memory/bug-memory-paint.png",
                    img1: "/images/bug-memory/bug-memory-preview-1.jpg",
                    img2: "/images/bug-memory/bug-memory-preview-2.jpg",
                    img3: "/images/bug-memory/bug-memory-preview-3.jpg",
                    visit: "https://bug-memory.vercel.app/",
                    code: "https://github.com/dojeda1/memory-game",
                    sum: "Test your memory by selecting every bug card without choosing the same one twice. Each time one is chosen, the game will shuffle the cards and display them in a random order using REACT. If you select the same bug twice, you lose!",
                    list: ["HTML", "CSS", "JavaScript", "Bootstrap", "React.js", "Node.js"],
                    isActive: false
                },
                {
                    title: "Wizard Words",
                    mockup: "/images/wizard-words/wizard-words-mock.png",
                    paint: "/images/wizard-words/wizard-words-paint.png",
                    img1: "/images/wizard-words/wizard-words-preview-1.jpg",
                    img2: "/images/wizard-words/wizard-words-preview-2.jpg",
                    img3: "/images/wizard-words/wizard-words-preview-3.jpg",
                    visit: "https://dojeda1.github.io/Word-Guess-Game/",
                    code: "https://github.com/dojeda1/Word-Guess-Game",
                    sum: "This version of the traditional Hangman game is fashioned after the wizarding world of Harry Potter. The game displays what letters you have previously guessed, how many guesses you have left, as well as your wins and losses. The design was inspired by the Marauder's Map and uses jQuery for a number of cool fade-in and fade-out animations.",
                    list: ["HTML", "CSS", "JavaScript", "jQuery", "Bootstrap"],
                    isActive: false
                },
                {
                    title: "Sci-fi RPG",
                    mockup: "/images/scifi-rpg/scifi-rpg-mock.png",
                    paint: "/images/scifi-rpg/scifi-rpg-paint.png",
                    img1: "/images/scifi-rpg/scifi-rpg-preview-1.jpg",
                    img2: "/images/scifi-rpg/scifi-rpg-preview-2.jpg",
                    img3: "/images/scifi-rpg/scifi-rpg-preview-3.jpg",
                    visit: "https://dojeda1.github.io/Space-RPG-Game/",
                    code: "https://github.com/dojeda1/Space-RPG-Game",
                    sum: "Choose one of 4 classic Sci-fi characters to play as and try to defeat all of the remaining opponents. Each character has different health, strength, and leveling up stats and you must choose defenders in particular orders to obtain victory.",
                    list: ["HTML", "CSS", "JavaScript", "jQuery", "Bootstrap"],
                    isActive: false
                },
                {
                    title: "Trial of Socrates",
                    mockup: "/images/socrates/socrates-mock.png",
                    paint: "/images/socrates/socrates-paint.png",
                    img1: "/images/socrates/socrates-preview-1.jpg",
                    img2: "/images/socrates/socrates-preview-2.jpg",
                    img3: "/images/socrates/socrates-preview-3.jpg",
                    visit: "https://dojeda1.github.io/Socrates-Game/",
                    code: "https://github.com/dojeda1/Socrates-Game",
                    sum: "This is a text based adventure game inspired by the ancient tale of when Socrates was sentenced to death by a jury of his fellow Athenians. Playing as the philosopher himself, you make branching choices powered by IF/ELSE functions in JavaScript that lead to alternate endings. The goal is to find the historical ending or simply explore alternate timelines. ",
                    list: ["HTML", "CSS", "JavaScript", "jQuery", "Bootstrap"],
                    isActive: false
                },
                {
                    title: "Trivia Game",
                    mockup: "/images/trivia-game/trivia-mock.png",
                    paint: "/images/trivia-game/trivia-paint.png",
                    img1: "/images/trivia-game/trivia-preview-1.jpg",
                    img2: "/images/trivia-game/trivia-preview-2.jpg",
                    img3: "/images/trivia-game/trivia-preview-3.jpg",
                    visit: "https://dojeda1.github.io/TriviaGame/",
                    code: "https://github.com/dojeda1/TriviaGame",
                    sum: "Test your knowledge of both the metric and imperial units of measurement. Each question is timed and will move onto the next if left unanswered. After each question, a fun GIF is briefly displayed according to whether or not you were correct. At the end of the game, the number of right, wrong, and unanswered responses is displayed along side your overall score. Your high scores are also shown at the bottom of the results page.",
                    list: ["HTML", "CSS", "JavaScript", "jQuery", "Bootstrap"],
                    isActive: false
                },
                // {
                //     title: "Fur Butlr",
                //     mockup: "/images/fur-butlr/fur-butlr-mock.png",
                //     paint: "/images/fur-butlr/fur-butlr-paint.png",
                //     img1: "/images/fur-butlr/fur-butlr-preview-1.jpg",
                //     img2: "/images/fur-butlr/fur-butlr-preview-2.jpg",
                //     img3: "/images/fur-butlr/fur-butlr-preview-3.jpg",
                //     visit: "https://fur-butlr-app.herokuapp.com/",
                //     code: "https://github.com/ApexPanda/FurButler",
                //     sum: "A place where pet owners can meet each other and search for pet services like walkers, groomers or sitters. Fur Butlr lets you create a profile, login in, edit your page, and show off your pets.",
                //     list: ["HTML", "CSS", "JavaScript", "jQuery", "Materialize", "MySQL", "Node.js", "Sequelize"],
                //     isActive: false
                // },
                {
                    title: "Book Finder",
                    mockup: "/images/book-finder/book-finder-mock.png",
                    paint: "/images/book-finder/book-finder-paint.png",
                    img1: "/images/book-finder/book-finder-preview-1.jpg",
                    img2: "/images/book-finder/book-finder-preview-2.jpg",
                    img3: "/images/book-finder/book-finder-preview-3.jpg",
                    visit: "https://book-search-dojeda1.vercel.app/",
                    code: "https://github.com/dojeda1/book-search",
                    // sum: "Search through a large database of books with the help of the Google Books API. User inputs the title of a book and results are displayed below. They can then visit the google URL, save the book in a MongoDB database for later, or delete it from saved books.",
                    sum: "Search through a large database of books with the help of the Google Books API. User inputs the title of a book and results are displayed below. They can then visit the google URL using the data provided by the API.",
                    // list: ["HTML", "CSS", "JavaScript", "React.js", "MongoDB", "Mongoose", "Google Books API", "Bootstrap"],
                    list: ["HTML", "CSS", "JavaScript", "React.js", "Bootstrap", "Google Books API"],
                    isActive: false
                }
            ]
        }
    },
    methods: {
        log(msg) {
            console.log('Log:',msg);
        },
        logEvents (eventName, params) {
            console.log(eventName, params)
            console.log('Index:',params.currentSlide.index)
            this.swapProject(params.currentSlide.index)
        },
        swapProject(ind) {
            console.log(ind)
            console.log(this.projects[ind])
            this.currentProject = this.projects[ind];
            this.projects.forEach(project => project.isActive = false)
            this.projects[ind].isActive = true;
        },
        zoomImage(src) {
            console.log('hey');
            this.imageModal = src;
        },
        createSlide(project) {
            return `
            <img class="paint-pic" src="${project.paint}" alt="Painted Preview">
            <p>${project.title}</p>
        `
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
        height: 100%;
        width: 100%;
        transition: all .2s ease-in-out;
        box-shadow: 0 4px 5px 0 rgb(0 0 0 / 14%), 0 1px 10px 0 rgb(0 0 0 / 12%), 0 2px 4px -1px rgb(0 0 0 / 30%);
        cursor: zoom-in;
        object-fit: cover;
    }
    .preview-img:hover {
        opacity: .8;
    }
    .image-modal {
        cursor: zoom-out;
        backdrop-filter: blur(.5rem);
        position: fixed;
        z-index: 4;
        top: 0;
        left: 0;
        background-color: rgba(33, 33, 33, 0.75);
        height: 100%;
        width: 100%;
        padding: 10px;
    }
    .image-modal img{
        object-fit: contain;
        height: 100%;
        width: 100%;
    }
    .portfolio-devices {
        width: 100%;
        margin-bottom: 1rem;
    }
    .paint-pics {
        padding: 0 40px;
        width: 100%;
        overflow: hidden;
    }
    .paint-pic-container {
        cursor: pointer;
        padding: 10px;
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

    /* Vueper */
    .vueperslides__bullet .default {
        background-color: #212121;
        border-color: #8dc63f;
        border-width: 0px;
    }

    .vueperslides__bullet--active .default {
        border-width: 6px;
    }
</style>
