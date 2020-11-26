<template>
  <div class="timer">
    <h2 class="timer__phase">{{ this._formatPhase(this.phase) }}</h2>
    <h1 class="timer__clock">{{ this.format(this.timer) }}</h1>
    <div class="timer__intervals">
        <div :class="'interval ' + _intervalClass(i)" v-for="(_, i) in new Array(this.configuration.longBreakIntervals)" :key="i">
        </div>
    </div>
    <button @click="this.pause">Pause</button>
    <button @click="this.stop">Stop</button>
  </div>
</template>
<script>
export default {
  name: "Timer",
  props: ["timer", "stop", "pause", "format", "phase", "intervals", "configuration"],
  methods: {
    _formatPhase: function (phase) {
        return phase.replace(/([A-Z])/g, ' $1').trim().toUpperCase();
    },
    _intervalClass: function (index) {
        return index == this.intervals ? "active" : "not";
    }
  },
};
</script>