<template>
    <div id="nav">
      <a class="nav-logo" href="">
          <!-- <img id="logo-side" src="@/assets/img/art-logo.png" alt="D Logo" /> -->
          <img src="/images/web-portfolio/D_logo_final.png" alt="D Logo" />
      </a>
      <a href="#portfolio" class="nav-link">Portfolio</a>
      <a href="#about" class="nav-link">About</a>
      <a href="#contact" class="nav-link">Contact</a>
      <a href="/files/dro_resume.pdf" target="blank" class="nav-link">Resume</a>
      <div class="nav-link game-link" onClick="CLICK">{{code}}</div>
    </div>
</template>

<script>
export default {
  name: 'Nav',
  props: {
    msg: String
  },
  data() {
    return {
      code: '< / >'
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>

</style>
