<template>
  <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
  <Nav @gameToggled="handleGameToggled"/>
  <div id="main-content">
    <Hero/>
    <Portfolio/>
    <About/>
    <Contact/>
    <Footer/>
  <template v-if="playingGame == true">
    <Game/>
  </template>
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import Nav from './components/Nav.vue'
import Hero from './components/Hero.vue'
import Portfolio from './components/Portfolio.vue'
import About from './components/About.vue'
import Contact from './components/Contact.vue'
import Footer from './components/Footer.vue'
import Game from './components/game/Game.vue'

export default {
  name: 'App',
  components: {
    // HelloWorld
    Nav,
    Hero,
    Portfolio,
    About,
    Contact,
    Footer,
    Game
  },
  data() {
    return {
      code: '< / >',
      playingGame: false,
      questComplete: false,
      menu: false,
      gameStarted: false,
    }
  },
  methods: {
    log(msg) {
        console.log('Log:',msg);
    },
    handleGameToggled() {
      if (this.playingGame) {
        this.playingGame = false;
        history.replaceState(null,null,window.location.pathname)
      } else {
        this.playingGame = true;
        history.replaceState('game','','?game=true')
      }
      console.log(this.playingGame)
      if (!this.playingGame) {
        this.gameStarted = false;
      }
      console.log(this.playingGame)
    }
  },
  created: function() {
    let url = window.location.href.split('?');
    if (url.length == 2)
    {
      let vars = url[1].split('&');
      let getVars = {};
      let tmp = '';
      const $this = this;
      vars.forEach(function(v){
        tmp = v.split('=');
        if(tmp.length == 2) {
          getVars[tmp[0]] = tmp[1];
          if (tmp[0] == 'game' && tmp[1] == 'true') {
            console.log("sup yo")
            $this.handleGameToggled();
          }
        }
      });
    }
  }
}
</script>

<style>
/* HTML RESET */
html, body, div, span, applet, object, iframe,
h1, h2, h3, h4, h5, h6, p, blockquote, pre,
a, abbr, acronym, address, big, cite, code,
del, dfn, em, img, ins, kbd, q, s, samp,
small, strike, strong, sub, sup, tt, var,
b, u, i, center,
dl, dt, dd, ol, ul, li,
fieldset, form, label, legend,
table, caption, tbody, tfoot, thead, tr, th, td,
article, aside, canvas, details, embed, 
figure, figcaption, footer, header, hgroup, 
menu, nav, output, ruby, section, summary,
time, mark, audio, video {
	margin: 0;
	padding: 0;
	border: 0;
	font-size: 100%;
	font: inherit;
	vertical-align: baseline;
  box-sizing: border-box;
}
/* HTML5 display-role reset for older browsers */
article, aside, details, figcaption, figure, 
footer, header, hgroup, menu, nav, section {
	display: block;
}
body {
	line-height: 1;
}
ol, ul {
	list-style: none;
}
blockquote, q {
	quotes: none;
}
blockquote:before, blockquote:after,
q:before, q:after {
	content: '';
	content: none;
}
table {
	border-collapse: collapse;
	border-spacing: 0;
}
</style>
