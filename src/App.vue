<template>
  <div id="app">
    <FrameVoto/>
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import FrameVoto from './components/FrameVoto.vue'

export default {
  name: 'App',
  components: {
    FrameVoto
  }
}
</script>

<style>
html {
  margin: 0 !important;
  padding: 0 !important;
}
body {
  margin: 0 !important;
  padding: 0 !important;
}
#app {
  height: 100vh;
  padding: 0px;
  margin: 0px;
}
</style>
