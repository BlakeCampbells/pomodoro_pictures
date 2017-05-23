<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <div id="timer"></div>
  </div>
</template>

<script>
var ProgressBar = require('progressbar.js')
export default {
  name: 'hello',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  },
  mounted: function () {
    var bar = new ProgressBar.Circle('#timer', {
      color: '#aaa',
      // This has to be the same size as the maximum width to
      // prevent clipping
      strokeWidth: 4,
      trailWidth: 1,
      easing: 'linear',
      duration: (60000 * 25),
      text: {
        autoStyleContainer: false
      },
      from: { color: '#aaa', width: 1 },
      to: { color: '#333', width: 4 },
      // Set default step function for all animate calls
      step: function (state, circle) {
        circle.path.setAttribute('stroke', state.color)
        circle.path.setAttribute('stroke-width', state.width)

        var secondsPassed = Math.round((circle.value() * 14.4) * 100)
        var secondsLeft = 1500 - secondsPassed
        var minutes = Math.floor(secondsLeft / 60)
        var seconds = secondsLeft - minutes * 60
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
<style scoped>
#timer {
  margin: 20px;
  width: 200px;
  height: 200px;
  position: relative;
}
h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}
</style>
