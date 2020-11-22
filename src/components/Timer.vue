<template>
  <div class="timer">
    <h2 class="timer__phase">{{ this._formatPhase(this.phase) }}</h2>
    <h1 class="timer__clock">{{ this._formatTime(this.timer) }}</h1>
    <button @click="this.stop">STOP</button>
  </div>
</template>
<script>
export default {
  name: "Timer",
  props: ["timer", "stop", "phase", "intervals"],
  methods: {
    _formatTime: function (time) {
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
    _formatPhase: function (phase) {
        return phase.toUpperCase();
    },
  },
};
</script>