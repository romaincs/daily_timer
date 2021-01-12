<template>
  <div>
    <h1>Timer</h1>
    <span>{{minutes}}:{{seconds}}</span>
    <input type="button" @click="start()" value="Start" :disabled="running" />
    <input type="button" @click="stop()" value="Stop" :disabled="!running" />
  </div>
</template>

<script>
export default {
  name: 'Timer',
  props: {
    //No props
  },
  data() {
    return {
      countDown:65, 
      running: false,
      intervalId: 0
    }
  },
  computed: {
    minutes() {
      let minutes = Math.floor(this.countDown / 60)
      return minutes
    },
    seconds() {
      let seconds = this.countDown - (this.minutes*60)
      return seconds
    },
  },
  methods: {
    start() {
      let that = this
      this.running = true
      this.intervalId = setInterval(() => {
        if(that.countDown > 0) {
          that.countDown--
        }
        else {
          clearInterval()
          that.running = false
        }
      }, 1000)
    },

    stop() {
      this.running = false
      clearInterval(this.intervalId)
    }
  }
}
</script>