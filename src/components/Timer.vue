<template>
  <div>
      <h2>{{ this.phase }}</h2>
      <h1>{{ this._formatTime(timer) }}</h1>
      <button @click="stop">STOP</button>
  </div>
</template>
<script>
export default {
  name: "Timer",
  props: ["timer", "stop", "phase", "intervals"],
  methods: {
      _formatTime: function (time) {
          return Math.floor(time / 60).toString().padStart(2, 0) + ":" +
                 Math.floor(time % 60).toString().padStart(2, 0);
      }
  }
};
</script>