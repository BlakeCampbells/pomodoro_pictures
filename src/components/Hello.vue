<template>
  <div class="hello row">
    <div class="col m4"></div>
    <div class="col m4">
      <div id="timer"></div>
    </div>
    <div class="col m4"></div>
  </div>
</template>

<script>
var ProgressBar = require('progressbar.js')
export default {
  name: 'hello',
  data () {
    return {}
  },
  mounted: function () {
    var durationMinutes = 25
    var bar = new ProgressBar.Circle('#timer', {
      color: '#ffffff',
      // This has to be the same size as the maximum width to
      // prevent clipping
      strokeWidth: 4,
      trailWidth: 1,
      easing: 'linear',
      duration: (60000 * durationMinutes),
      text: {
        autoStyleContainer: false
      },
      from: { color: '#1ada5b', width: 1 },
      to: { color: '#000', width: 4 },
      // Set default step function for all animate calls
      step: function (state, circle) {
        circle.path.setAttribute('stroke', state.color)
        circle.path.setAttribute('stroke-width', state.width)

        var secondsPassed = Math.round((circle.value() * 14.4) * 100)
        var secondsLeft = (durationMinutes * 60) - secondsPassed
        var minutes = Math.floor(secondsLeft / 60)
        var seconds = secondsLeft - (minutes * 60)
        if (secondsPassed === 0) {
          circle.setText('')
        } else {
          circle.setText(minutes + ':' + seconds)
        }
      }
    })
    bar.text.style.fontFamily = '"Raleway", Helvetica, sans-serif'
    bar.text.style.fontSize = '2rem'
    bar.animate(1.0)
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
