<template>
  <div class="timer">
    <h2 class="timer__phase">{{ this._formatPhase(this.phase) }}</h2>
    <h1 class="timer__clock">{{ this.format(this.timer) }}</h1>
    <button @click="this.stop">STOP</button>
  </div>
</template>
<script>
export default {
  name: "Timer",
  props: ["timer", "stop", "format", "phase", "intervals"],
  methods: {
    _formatPhase: function (phase) {
        return phase.toUpperCase();
    },
  },
};
</script>