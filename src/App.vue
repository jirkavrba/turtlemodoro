<template>
  <div id="app" :class="'phase__' + phase">
    <h1>Turtlemodoro</h1>
    <Stream :phase="this.phase" />
    <Timer
      v-if="this.running"
      :timer="this.timer"
      :phase="this.phase"
      :format="this.formatTime"
      :intervals="this.intervals"
      :stop="this.stop"
    />
    <Configuration
      v-else
      :configuration="this.configuration"
      :start="this.start"
    />
  </div>
</template>

<script>
import "./assets/style.css";

import Timer from "./components/Timer";
import Stream from "./components/Stream";
import Configuration from "./components/Configuration";

export default {
  name: "App",
  data: () => ({
    running: false,
    timer: 0,
    phase: "none",
    intervals: 0, // How many short breaks was there since last long break
    configuration: {
      // Times configuration
      pomodoro: 25,
      shortBreak: 5,
      longBreak: 15,
      // How many intervals between long breaks
      longBreakIntervals: 4,
    },

    _interval: null,
  }),
  methods: {
    start: function () {
      this.running = true;
      this.phase = "pomodoro";
      this.timer = this.configuration.pomodoro * 60;
      this.intervals = 0;

      this._interval = window.setInterval(this.tick, 1000);
    },
    stop: function () {
      this.running = false;
      this.phase = "stop";

      window.clearInterval(this._interval);
    },

    tick: function () {
      this.timer--;
      
      document.title = this.formatTime(this.timer) + " | Turtlemodoro" 

      // TODO: Refactor this shit
      if (this.timer == 0) {
        if (this.phase == "pomodoro") {
          this.intervals++;

          if (this.intervals == this.configuration.longBreakIntervals) {
            this.timer = this.configuration.longBreak * 60;
            this.phase = "longBreak";
            this.intervals = 0;
          } else {
            this.timer = this.configuration.shortBreak * 60;
            this.phase = "shortBreak";
          }
        } else {
          this.phase = "pomodoro";
          this.timer = this.configuration.pomodoro * 60;
        }
      }
    },
    formatTime: function (time) {
      return (
        Math.floor(time / 60)
          .toString()
          .padStart(2, 0) +
        ":" +
        Math.floor(time % 60)
          .toString()
          .padStart(2, 0)
      );
    },
  },
  components: {
    Stream,
    Timer,
    Configuration,
  },
};
</script>