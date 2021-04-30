<template>
    <div id="nav">
      <a class="nav-logo" href="">
          <!-- <img id="logo-side" src="@/assets/img/art-logo.png" alt="D Logo" /> -->
          <img src="/images/web-portfolio/D_logo_final.png" alt="D Logo" />
      </a>
      <template v-if="!$parent.playingGame">
        <a href="#portfolio" class="nav-link">Portfolio</a>
        <a href="#about" class="nav-link">About</a>
        <a href="#contact" class="nav-link">Contact</a>
        <a href="/files/dro_resume.pdf" target="blank" class="nav-link">Resume</a>
        <div class="nav-link text-gray" @click="gameToggled">{{code}}</div>
      </template>
      <template v-else>
        <div class="nav-link" :class="{'disabled' : !$parent.gameStarted, 'selected' : $parent.menu == 'Save'}" @click="saveToggled">Save</div>
        <div class="nav-link" :class="{'disabled' : !$parent.gameStarted, 'selected' : $parent.menu == 'Quests', 'notification': $parent.questComplete}" @click="questsToggled">Quests</div>
        <div class="nav-link" :class="{'disabled' : !$parent.gameStarted, 'selected' : $parent.menu == 'Stats'}" @click="statsToggled">Stats</div>
        <div class="nav-link text-gray" :class="{'selected' : $parent.menu == 'Quit'}" @click="quitToggled">{{code}}</div>
      </template>
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
    },
    saveToggled() {
      if (this.$parent.menu != 'Save') {
          this.$parent.menu = 'Save';
      } else {
          this.$parent.menu = false
      }
      console.log(this.menu)
    },
    questsToggled() {
      if (this.$parent.menu != 'Quests') {
          this.$parent.menu = 'Quests';
      } else {
          this.$parent.menu = false
      }
      console.log(this.$parent.menu)
    },
    statsToggled() {
      if (this.$parent.menu != 'Stats') {
          this.$parent.menu = 'Stats';
      } else {
          this.$parent.menu = false
      }
      console.log(this.$parent.menu)
    },
    quitToggled() {
      if (this.$parent.menu != 'Quit') {
          this.$parent.menu = 'Quit';
      } else {
          this.$parent.menu = false
      }
      console.log(this.$parent.menu)
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
      z-index: 3;
  }
  .nav-link {
      color: white;
      display: block;
      padding: 15px 5px;
      text-decoration: none;
      cursor: pointer;
  }
  .nav-link.disabled {
      color: #454545;
      /* background-color: gray; */
      pointer-events: none;
  }

  #nav .nav-link:hover {
      color: #27aae1;
      border-right: #27aae1 5px solid;
  }

  #nav .nav-link.selected {
      color: #8dc63f;
      border-right: #8dc63f 5px solid;
  }

  .notification::after {
    display: inline-block;
    content: '';
    margin-left: 5px;
    margin-right: -15px;
    width: 10px;
    height: 10px;
    border-radius: 50%;
    background-color: gold;
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
      justify-content: space-evenly;
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
    #nav .nav-link.selected {
      color: #8dc63f;
      border-bottom: #8dc63f 5px solid;
      border-right: none;
    }
  }

</style>
