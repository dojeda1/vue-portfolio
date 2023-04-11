<template>
<div id="footer">
    <h5 class="text-blue">Live, Learn, Create</h5>
    <div>
        <a href="https://www.linkedin.com/in/dominic-ojeda/" target="blank">
            <img class="footer-icon" src="/images/web-portfolio/linkedin-logo.png" alt="Linkedin Logo" />
        </a>
        <a href="https://github.com/dojeda1" target="blank">
            <img class="footer-icon" src="/images/web-portfolio/github-logo.png" alt="GitHub Logo" />
        </a>
        <a href="/files/dominic-ojeda_resume.pdf" target="blank">
            <img class="footer-icon" src="/images/web-portfolio/resume-logo.png" alt="Resume Logo" />
        </a>
    </div>
    <p class="text-blue">Copyright © 2023 - Dominic Ojeda</p>
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
            const $this = this;
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
    #footer {
        padding: 15px;
        background-color: #212121;
        text-align: center;
    }
    .footer-icon {
        width: 30px;
        margin: 0 25px;
        /* transition: all .2s ease-in-out; */
    }
</style>
