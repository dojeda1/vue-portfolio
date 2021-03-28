<template>
<div id="about">
    <div class="section-title">
        <h4 class=" dom-green-2-text">About</h4>
    </div>
    <div class="container">
        <div class="grid">
            <div class="col-8 s_col-12">
                <div class="section-header">
                    <h5 class=" dom-green-2-text">Summary</h5>
                </div>
                <p>
                    I am a web developer who enjoys coding on both front-end and back-end. Experience producing web applications with HTML, CSS, and JavaScript and creating visual elements with Photoshop and Illustrator. I have a passion for making programs that are visually appealing and fun to interact with. I enjoy solving problems creatively and I am always determined to try again and again until the vision becomes reality. I believe clear communication is key to completing any project, and I work well in a team or going at it alone. I know that the right code can solve any problem.
                </p>
            </div>
            <div class="col-4 s_col-12">
                <div class="section-header">
                    <h5 class=" dom-green-2-text">My Tool Belt</h5>
                </div>
                <div class="tool-logos">
                    <img class="tool-logo" src="/images/web-portfolio/tool-logos/html-logo.png" alt="HTML Logo"
                        :class="{'rotate': isRotated[0]}"
                        @click="rotate(0)">
                    <img class="tool-logo" src="/images/web-portfolio/tool-logos/css-logo.png" alt="CSS Logo"
                        :class="{'rotate': isRotated[1]}"
                        @click="rotate(1)">
                    <img class="tool-logo" src="/images/web-portfolio/tool-logos/js-logo.png" alt="JS Logo"
                        :class="{'rotate': isRotated[2]}"
                        @click="rotate(2)">
                    <img class="tool-logo" src="/images/web-portfolio/tool-logos/jquery-logo.png" alt="jQuery Logo"
                        :class="{'rotate': isRotated[3]}"
                        @click="rotate(3)">
                    <img class="tool-logo" src="/images/web-portfolio/tool-logos/react-logo.png" alt="React Logo"
                        :class="{'rotate': isRotated[4]}"
                        @click="rotate(4)">
                    <img class="tool-logo" src="/images/web-portfolio/tool-logos/node-logo.png" alt="Node Logo"
                        :class="{'rotate': isRotated[5]}"
                        @click="rotate(5)">
                    <img class="tool-logo" src="/images/web-portfolio/tool-logos/mongodb-logo.png" alt="MongoDB Logo"
                        :class="{'rotate': isRotated[6]}"
                        @click="rotate(6)">
                    <img class="tool-logo" src="/images/web-portfolio/tool-logos/mysql-logo.png" alt="mySQL Logo"
                        :class="{'rotate': isRotated[7]}"
                        @click="rotate(7)">
                    <img class="tool-logo" src="/images/web-portfolio/tool-logos/bootstrap-logo.png" alt="Bootstrap Logo"
                        :class="{'rotate': isRotated[8]}"
                        @click="rotate(8)">
                    <img class="tool-logo" src="/images/web-portfolio/tool-logos/materialize-logo.png" alt="Materialize Logo"
                        :class="{'rotate': isRotated[9]}"
                        @click="rotate(9)">
                    <img class="tool-logo" src="/images/web-portfolio/tool-logos/ps-logo.png" alt="Photoshop Logo"
                        :class="{'rotate': isRotated[10]}"
                        @click="rotate(10)">
                    <img class="tool-logo" src="/images/web-portfolio/tool-logos/illustrator-logo.png" alt="Illustrator Logo"
                        :class="{'rotate': isRotated[11]}"
                        @click="rotate(11)">
                </div>
            </div>
        </div>
        <div class="section-header">
            <h5 class=" dom-green-2-text">My Interests</h5>
        </div>
        <div class="interests grid">
            <div class="col-3 s_col-12">
                <img class="interest-icon" src="/images/web-portfolio/tree-logo.png" alt="CSS Logo"
                :class="{'rotate': isRotated[12]}"
                @click="rotate(12)">
                <p>
                    Nature is incredible. The trees, rocks, and rivers give us everything we need for life. Being outdoors, taking in the fresh air and looking at all the wonder this world has to offer can always give me sense of peace and purpose.
                </p>
            </div>
            <div class="col-3 s_col-12">
                <img class="interest-icon" src="/images/web-portfolio/space-logo.png" alt="CSS Logo"
                :class="{'rotate': isRotated[13]}"
                @click="rotate(13)">
                <p>
                    The vastness of the universe forever leaves me in awe. I love learning about the planets and stars, and how they all mesh together to form the cosmos. There is so much we don't yet know, and that's part of what makes it so exciting.
                </p>
            </div>
            <div class="col-3 s_col-12">
                <img class="interest-icon" src="/images/web-portfolio/art-logo.png" alt="CSS Logo"
                :class="{'rotate': isRotated[14]}"
                @click="rotate(14)">
                <p>
                    Art has always been one of my great loves. I enjoy creating characters, concept art, and logo designs using both traditional and digital medium. Many hours have been spent huddled over my desk working on my next piece.
                </p>
            </div>
            <div class="col-3 s_col-12">
                <img class="interest-icon" src="/images/web-portfolio/game-logo.png" alt="CSS Logo"
                :class="{'rotate': isRotated[15]}"
                @click="rotate(15)">
                <p>
                    Playing or creating games is always a good time. It's so much fun to go on adventures, explore new worlds, or reach that new high score. My favorite games include Gears of War, Final Fantasy, and Super Smash Bros.
                </p>
            </div>
        </div>
    </div>
</div>
</template>

<script>
export default {
    name: 'About',
    data() {
        return {
            title: 'My App is cooler than yours',
            isRotated: [
                false,
                false,
                false,
                false,
                false,
                false,
                false,
                false,
                false,
                false,
                false,
                false,
                false,
                false,
                false,
                false
            ],
        }
    },
    methods: {
        log(msg) {
            console.log('Log:',msg);
        },
        rotate(ind) {
            var $this = this;
            if (!this.isRotated[ind]) {
                this.isRotated[ind] = true;
                setTimeout(function() {
                    $this.isRotated[ind] = false;
                },1000);
            }
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
    .tool-logos {
        display: flex;
        flex-wrap: wrap;
        justify-content: space-between;
        align-content: center;
    }
    .tool-logo {
        /* width: 50px; */
        width: 25%;
        padding: 10px;
        transition: all 1s ease-in-out;
    }
    .tool-logo:hover {
        transform: scale(1.1);
        transition: all .1s ease-in-out;
    }
    .interests {
        text-align: center;
    }
    .interest-icon {
        width: 80px;
        transition: all 1s ease-in-out;
        margin-top: 10px;
        margin-bottom: 15px;
    }
    .interest-icon:hover {
        transform: scale(1.1);
        transition: all .1s ease-in-out;
    }

    @media only screen and (max-width: 600px) {
        .interests {
            text-align: left;
        }
        .interest-icon {
            float: left;
            margin-top: 5px;
            margin-bottom: 0px;
            margin-right: 5px
        }
    }

</style>
