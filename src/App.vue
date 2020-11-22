<template>
  <div id="app">
    <h1>Turtlemodoro</h1>
    <Stream/>
  </div>
</template>

<script>
import Stream from './components/Stream';

export default {
  name: 'App',
  data: () => ({
    running: false,
    configuration: {
      // Times configuration
      pomodoro: 25,
      shortBreak: 5,
      longBreak: 15,
      // How many intervals between long breaks
      longBreakIntervals: 4
    }
  }),
  components: {
    Stream
  }
}
</script>