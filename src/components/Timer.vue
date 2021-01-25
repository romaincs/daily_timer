<template>
  <div class="timer">
    <div class="title">
      <h1>Timer</h1>
    </div>
    <form class="container">
      <div class="countdown">
        <div>
          <span :class="{'timeout': countDown == 0}">{{minutes}}:{{seconds}}</span>
        </div>
        <div class="buttons">
          <input type="button" @click="startStop()" :value="startStopValue" :disabled="countDown == 0" />
          <input type="button" @click="reset()" value="Reset" />
        </div>
      </div>
      <div class="config">
        <h3>Default</h3>
        <div>
          <div v-for="configValue in configValues" :key="configValue">
            <input type="radio" name="config" :id="'config-' + configValue" @click="setStartCountDownValue(configValue)" :checked="isConfigValueChecked(configValue)">
            <label :for="'config-' + configValue">
              <span>{{(configValue / 60)}} min</span>
            </label>
          </div> 
          <div>
            <input type="radio" name="config" id="config-custom" @click="setStartCountDownValue('Custom')" :checked="isConfigValueChecked('Custom')">
            <label for="config-custom">
              <span>Custom</span>
            </label>
            <br />
            <input type="number" name="config-custom-value" id="config-custom-value" v-model="customConfigValue" :disabled="!this.isConfigValueChecked('Custom')">
          </div>         
        </div>
      </div>
    </form>

  </div>
</template>

<script>
  import * as numbers from '../utils/numbers.js'
  import {
    global
  } from '../utils/constants.js'

  export default {
    name: 'Timer',
    props: {
      //No props
    },


    data() {
      return {
        countDown: null,
        startCountDownValue: null,
        configValues: global.CONFIG_VALUES,
        customConfigValue: null,
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
      }
    },


    watch: {
      customConfigValue: function(value) {
        this.setStartCountDownValue(value)
      },

      startCountDownValue: function(value) {
        if(!this.running) {
          if(this.isConfigValueChecked('Custom')) {
            this.countDown = this.customConfigValue
          } else {
            this.countDown = value
          }
        }
      }
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
        this.countDown = this.startCountDownValue
      },

      setStartCountDownValue(selectedConfig) {        
        this.startCountDownValue = (selectedConfig != 'Custom') ? selectedConfig : this.customConfigValue
      },

      isConfigValueChecked(configValue) {
        if(configValue == 'Custom') {
          return this.startCountDownValue == this.customConfigValue
        } else {
          return this.startCountDownValue == configValue
        }
      }
    },

    mounted: function() {
      this.startCountDownValue = global.DEFAULT_CT_VALUE
      if(!this.configValues.includes(global.DEFAULT_CT_VALUE)) {
        this.customConfigValue = global.DEFAULT_CT_VALUE
      } 
    }
  }
</script>

<style>
  .countdown {
    font-size: 46px;
  }

  .buttons input {
    width: 65px;
  }

  .buttons input:first-child {
    margin-right: 10px;
  }

  .timer {
    display: flex;
    flex-direction: column;
    text-align: center;
  }

  .timeout {
    color: red;
    animation: blinker 1s step-start infinite;
  }

  #custom-value {
    width:40px;
    margin-left: 22px;;
  }

  @keyframes blinker {
    50% {
      opacity: 0;
    }
  }
</style>