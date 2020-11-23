<template>
  <div class="configuration">
    <h1 class="timer__phase">Configuration</h1>

    <div class="configuration__slider">
      <label
        >Pomodoro time:
        <b
          >{{ configuration.pomodoro.toString().padStart(2, "0") }}:00</b
        ></label
      ><br />
      <input
        type="range"
        v-model="configuration.pomodoro"
        min="1"
        max="60"
        step="1"
      />
    </div>

    <div class="configuration__slider">
      <label
        >Short break time:
        <b
          >{{ configuration.shortBreak.toString().padStart(2, "0") }}:00</b
        ></label
      ><br />
      <input
        type="range"
        v-model="configuration.shortBreak"
        min="1"
        max="60"
        step="1"
      />
    </div>

    <div class="configuration__slider">
      <label
        >Long break time:
        <b
          >{{ configuration.longBreak.toString().padStart(2, "0") }}:00</b
        ></label
      ><br />
      <input
        type="range"
        v-model="configuration.longBreak"
        min="1"
        max="60"
        step="1"
      />
    </div>

    <div class="configuration__slider">
      <label
        >Long break intervals:
        <b>{{ configuration.longBreakIntervals }}</b></label
      ><br />
      <input
        type="range"
        v-model="configuration.longBreakIntervals"
        min="2"
        max="10"
        step="1"
      />
    </div>

    <button @click="this.start">START</button>
  </div>
</template>
<script>
export default {
  name: "Configuration",
  props: ["configuration", "start"],
};
</script>