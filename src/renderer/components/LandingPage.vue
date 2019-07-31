<template>
  <div id="wrapper">
    <Nav tab="Time"></Nav>
    <div class="e-box">
      <div class="item">
        <progress class="e-progress primary" :value="progress" max="100" style="height:2px;"></progress>
      </div>
    </div>
  </div>
</template>

<script>
  import Nav from './Nav'
  export default {
    name: 'landing-page',
    components: {
      Nav
    },
    data () {
      return {
        progress: 100
      }
    },
    created () {
      setInterval(() => {
        this.progress--
        if (this.progress <= 0) {
          this.progress = 100
        }
      }, 1000)
    },
    methods: {
      open (link) {
        this.$electron.shell.openExternal(link)
      }
    }
  }
</script>

<style>
  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }


  #wrapper {
    background:
      radial-gradient(
        ellipse at top left,
        rgba(255, 255, 255, 1) 40%,
        rgba(229, 229, 229, .9) 100%
      );
    height: 100vh;
    width: 100vw;
    padding: 1rem;
  }
  .e-progress {
    transition: .2s all;
  }
</style>
