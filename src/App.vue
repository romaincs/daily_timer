<template>
  <div id="app">
    <Timer />
  </div>
</template>

<script>
import Timer from './components/Timer.vue'

export default {
  name: 'App',
  components: {
    Timer
  }
}
</script>

<style>
  body {    
    font-family: 'Courier New', Courier, monospace;
    background-color:#252423;
    color:rgba(255,255,255, 0.95);
  }
  h1 {
    margin: 0px;
    padding: 0px;
  }
</style>