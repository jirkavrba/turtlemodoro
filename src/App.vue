<template>
  <div id="app" :class="'phase__' + phase">
    <div :class="'content' + (this.paused ? ' content--paused' : '')">
      <h1>Turtlemodoro</h1>
      <Stream :phase="this.phase" />
      <Timer
        v-if="this.running"
        :timer="this.timer"
        :phase="this.phase"
        :format="this.formatTime"
        :configuration="this.configuration"
        :intervals="this.intervals"
        :pause="this.pause"
        :stop="this.stop"
      />
      <div class="configuration" v-if="this.running">
        <div class="configuration__slider">
          <label
            >Volume <b>{{ this.volume }}</b
            >%</label
          >
          <br />
          <input type="range" v-model="volume" min="0" max="100" step="1" />
        </div>
      </div>
      <Configuration
        v-else
        :configuration="this.configuration"
        :start="this.start"
      />
    </div>
    <div class="paused" v-if="this.paused">
      <h1>Paused</h1>
      <button @click="this.pause">Resume</button>
    </div>
  </div>
</template>

<script>
import "./assets/style.css";
import pomodoro from "./assets/sounds/pomodoro.mp3";
import rickroll from "./assets/sounds/rickroll.mp3";

import { Howl, Howler } from "howler";
import Timer from "./components/Timer";
import Stream from "./components/Stream";
import Configuration from "./components/Configuration";

export default {
  name: "App",
  data: () => ({
    running: false,
    paused: false,
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
    volume: 100,

    _interval: null,
    _sounds: {},
  }),
  created: function () {
    this._sounds = {
      pomodoro: new Howl({ src: [pomodoro] }),
      shortBreak: new Howl({ src: [pomodoro] }),
      longBreak: new Howl({ src: [rickroll] }),
    };
  },
  methods: {
    start: function () {
      this.running = true;
      this.phase = "pomodoro";
      this.timer = this.configuration.pomodoro * 60;
      this.intervals = 0;

      this._interval = window.setInterval(this.tick, 1000);
      this.changeIcon();
    },
    pause: function () {
      this.paused = !this.paused;
    },
    stop: function () {
      this.running = false;
      this.phase = "stop";
      this.timer = 0;

      window.clearInterval(this._interval);
      this.changeIcon();
    },

    tick: function () {
      if (this.paused) return;

      this.timer--;

      document.title = this.formatTime(this.timer) + " | Turtlemodoro";

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

        this.playPhaseChangeSound();
        this.changeIcon();
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
    playPhaseChangeSound() {
      Howler.volume(this.volume / 100);
      this._sounds[this.phase].play();
    },
    changeIcon() {
      const icon = "icon" + (this.running ? "-" + this.phase : "") + ".png";

      const favicon = document.getElementById("favicon");
      favicon.href = icon;
    },
  },
  components: {
    Stream,
    Timer,
    Configuration,
  },
};
</script>