<template>
    <div id="hero" style="background-image: url('/images/web-portfolio/trees2.jpg')">
        <img class="hero-logo" src="/images/web-portfolio/d-logo.png" alt="D Logo" />
        <h1 class="white-text font2">DOMINIC OJEDA</h1>
        <h5 class="font1">Full Stack Web Developer</h5>
        <div class="hero-icons">
            <a href="https://www.linkedin.com/in/dominic-ojeda/" target="blank">
                <img class="hero-icon" src="/images/web-portfolio/linkedin-logo.png" alt="Linkedin Logo" />
            </a>
            <a href="https://github.com/dojeda1" target="blank">
                <img class="hero-icon" src="/images/web-portfolio/github-logo.png" alt="GitHub Logo" />
            </a>
            <a href="/files/dro_resume.pdf" target="blank">
                <img class="hero-icon" src="/images/web-portfolio/resume-logo.png" alt="Resume Logo" />
            </a>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Hero'
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
    #hero {
        height: 100%;
        min-height: 500px;
        padding: 20px;
        background-color: #2bb673;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        color: white;
        text-align: center;
        background-position: center;
        background-size: cover;
    }

    .hero-logo {
        width: 200px;
        margin: 0 25px 1rem 0;
    }

    .hero-icons {
        margin-top: 1rem;
    }

    .hero-icon {
        width: 60px;
        margin: 0 25px;
        transition: all .2s ease-in-out;
    }
    .hero-icon:hover {
        transform: translateY(-5px) scale(1.2);
    }
</style>
