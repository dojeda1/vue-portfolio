<template>
    <!-- TOP NAVBAR -->
    <!-- <div id="top-nav-container" class="navbar-fixed">
        <nav id="top-nav" class="nav-wrapper navbar-fixed grey darken-4">

            <div class="container">
                <a href="#!" class="sidenav-trigger left" data-target="side-modal">
                    <i class="material-icons">menu</i>
                </a>

                <a href="/" class="brand-logo">
                    <img src="DLogo" id="logo-top" class="left valign-wrapper" alt="D Logo" />
                </a>
            </div>
        </nav>
    </div> -->

    <!-- Modal side navbar -->
    <!-- <ul id="side-modal" class="sidenav center-align font1">
        <li>
            <a href="#portfolio" class="white-text">Portfolio</a>
        </li>
        <li>
            <a href="#about" class="white-text">About</a>
        </li>
        <li>
            <a href="#contact" class="white-text">Contact</a>
        </li>
        <li>
            <a href="resume" target="blank" class="white-text">Resume</a>
        </li>
        <li>
            <a class="lime-text" onClick="CLICK">CODE</a>
        </li>
    </ul> -->
    <!-- End NavBar -->

    <!-- LEFT MAIN SIDEBAR	 -->
    <ul id="nav">
        <div>
            <a id="logo-side-a" href="">
                <img id="logo-side" src="DLogo" alt="D Logo" />
            </a>
        </div>
        <div class="table-of-contents">
          <a href="#portfolio" class="white-text">Portfolio</a>
          <a href="#about" class="white-text">About</a>
          <a href="#contact" class="white-text">Contact</a>
          <a href="resume" target="blank" class="white-text">Resume</a>
          <a class="grey-text" onClick="CLICK">CODE</a>
        </div>
    </ul>
</template>

<script>
export default {
  name: 'Nav',
  props: {
    msg: String
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>

</style>
