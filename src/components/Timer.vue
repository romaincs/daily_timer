<template>
  <div class="timer">
    <div class="title">
      <h1>Timer</h1>
    </div>
    <div class="countdown">
      <span :class="{'timeout': countDown == 0}">{{minutes}}:{{seconds}}</span>
    </div>
    <div class="buttons">
      <input type="button" @click="startStop()" :value="startStopValue" :disabled="countDown == 0"/>
      <input type="button" @click="reset()" value="Reset" />
    </div>
  </div>
</template>

<script>
import * as numbers from '../utils/numbers.js'
import { global } from '../utils/constants.js'

export default {
  name: 'Timer',
  props: {
    //No props
  },
  data() {
    return {
      countDown: global.DEFAULT_CT_VALUE,
      running: false,
      intervalId: 0
    }
  },
  computed: {
    minutes() {
      let minutes = Math.floor(this.countDown / 60)      
      return numbers.padLeadingZero(minutes, 2)
    },
    seconds() {
      let seconds = this.countDown - (this.minutes * 60)
      return numbers.padLeadingZero(seconds, 2)
    },
    startStopValue() {
      if (!this.running) {
        return 'Start'
      } else {
        return 'Stop'
      }
    },
  },
  methods: {
    startStop() {
      let that = this
      if (!this.running) {
        this.running = true
        this.intervalId = setInterval(() => {
          if (that.countDown > 0) {
            that.countDown--
          } else {
            clearInterval()
            that.running = false
          }
        }, 1000)
      } else {
        this.running = false
        clearInterval(this.intervalId)
      }

    },

    reset() {
      clearInterval(this.intervalId)
      this.running = false      
      this.countDown = global.DEFAULT_CT_VALUE
    }
  }
}
</script>

<style>
  .countdown {
    font-size: 46px;
  }

  .buttons input {
    width:65px;
  }

  .buttons input:first-child {
    margin-right:10px;
  }

  .timer{
    display:flex;
    flex-direction: column;
    text-align: center;
  }

  .timeout {
    color: red;
    animation: blinker 1s step-start infinite;
  }

  @keyframes blinker {
    50% {
      opacity: 0;
    }
  }
</style>