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
      <div class="nav-link text-gray" @click="gameToggled">{{code}}</div>
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
  },
  methods: {
    log(msg) {
      console.log('Log:',msg);
    },
    gameToggled() {
      console.log('gameTogged!');
      this.$emit('gameToggled');
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
  #nav {
      background-color: #212121;
      color: white;
      position: fixed;
      top: 0;
      left: 0;
      width: 100px;
      height: 100%;
      text-align: center;
      z-index: 2;
  }
  .nav-link {
      color: white;
      display: block;
      padding: 15px 5px;
      text-decoration: none;
      cursor: pointer;
  }

  #nav .nav-link:hover {
      color: #27aae1;
      border-right: #27aae1 5px solid;
  }

  #nav .nav-logo {
      /* width: 100%; */
  }

  #nav .nav-logo img {
      padding: 20px;
      width: 100%;
  }

  @media screen and (max-width: 600px) {
    #nav {
      /* background-color: gray; */
      width: 100%;
      height: 60px;
      text-align: left;
      display: flex;
      flex-direction: row;
      justify-content: space-between;
      align-items: center;
    }

    #nav .nav-logo {
      display: none;
    }

    #nav .nav-logo img {
      padding: 10px;
      height: 100%;
      width: auto;
    }

    #nav .nav-link {
      display: inline-block;
    }
    #nav .nav-link:hover {
        color: #27aae1;
        border-bottom: #27aae1 5px solid;
        border-right: none;
    }
  }

</style>
