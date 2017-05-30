<template>
  <div class="hello">
    <div class="row">
      <div class="col m8"></div>
      <div class="col m4">
        <span v-if="show.start">
          <a v-on:click="start()" class="btn-floating btn-large waves-effect waves-light blue lighten-1">
            <i class="material-icons">play_arrow</i>
          </a>
        </span>
        <span v-if="show.play">
          <a v-on:click="play()" class="btn-floating btn-large waves-effect waves-light blue lighten-1">
            <i class="material-icons">play_arrow</i>
          </a>
        </span>
        <span v-if="show.stop">
          <a v-on:click="stop()" class="btn-floating btn-large waves-effect waves-light blue lighten-1">
            <i class="material-icons">stop</i>
          </a>
        </span>
        <span v-if="show.replay">
          <a v-on:click="reset()" class="btn-floating btn-large waves-effect waves-light blue lighten-3">
            <i class="material-icons">loop</i>
          </a>
        </span>
      </div>
    </div>
    <div class="row">
      <div class="col m4"></div>
      <div class="col m4">
        <div id="timer"></div>
      </div>
      <div class="col m4"></div>
    </div>
  </div>
</template>

<script>
var ProgressBar = require('progressbar.js')
export default {
  name: 'hello',
  data () {
    return {
      show: {
        start: true,
        stop: false,
        play: false,
        replay: false
      },
      durationMinutes: 25,
      circleChart: {}
    }
  },
  methods: {
    start: function () {
      var self = this
      this.show.start = false
      this.show.stop = true
      this.show.replay = true
      this.circleChart = new ProgressBar.Circle('#timer', {
        color: '#ffffff',
        // This has to be the same size as the maximum width to
        // prevent clipping
        strokeWidth: 4,
        trailWidth: 1,
        easing: 'linear',
        duration: (60000 * self.durationMinutes),
        text: {
          autoStyleContainer: false
        },
        from: { color: '#1ada5b', width: 1 },
        to: { color: '#000', width: 4 },
        // Set default step function for all animate calls
        step: function (state, circle) {
          circle.path.setAttribute('stroke', state.color)
          circle.path.setAttribute('stroke-width', state.width)
          // Percentage of progress on circle
          var secondsPassed = 0
          var secondsLeft = 0
          var minutes = 0
          var seconds = 0
          if (circle && circle._progressPath) {
            secondsPassed = Math.round((circle._progressPath._opts.duration * circle.value()) / 1000)
            secondsLeft = (self.durationMinutes * 60) - secondsPassed
            minutes = Math.floor(secondsLeft / 60)
            seconds = secondsLeft - (minutes * 60)
          }
          if (secondsPassed === 0) {
            circle.setText('')
          } else {
            circle.setText(('0' + minutes).slice(-2) + ':' + ('0' + seconds).slice(-2))
          }
        }
      })
      this.circleChart.text.style.fontFamily = '"Raleway", Helvetica, sans-serif'
      this.circleChart.text.style.fontSize = '2rem'
      this.circleChart.animate(1.0)
    },
    stop: function () {
      var self = this
      self.show.play = true
      self.show.stop = false
      self.circleChart.stop()
    },
    play: function () {
      var self = this
      self.show.play = false
      self.show.stop = true
      self.circleChart.animate(1.0)
    },
    reset: function () {
      var self = this
      self.show.stop = true
      self.show.play = false
      self.circleChart.destroy()
      self.start()
    }
  },
  mounted: function () {
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
body {
  min-height: 100%;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  background: url('../assets/washoe_valley.jpg') no-repeat center center fixed;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  -o-background-size: cover;
  background-size: cover;
}

#timer {
  margin: 20px;
  width: 100%;
  height: 100%;
  position: relative;
}
h1, h2 {
  font-weight: normal;
}
</style>
