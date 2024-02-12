<template>
  <h1>Doomsday Trainer</h1>
  <time :datetime="date.toString()">{{
    date.toLocaleString("en-us", {
      month: "short",
      year: "numeric",
      day: "numeric",
    })
  }}</time>
  <div class="weekday_selection">
    <button
      v-for="(weekday, index) in weekdays"
      :key="index"
      @click="guess(index)"
    >
      {{ weekday }}
    </button>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";

const weekdays = [
  "Sunday",
  "Monday",
  "Tuesday",
  "Wednesday",
  "Thursday",
  "Friday",
  "Saturday",
];

function guess(weekday: number) {
  console.log(weekdays[weekday]);
}

export default defineComponent({
  setup() {
    const date = ref(new Date());

    return {
      date,
      weekdays,
      guess,
    };
  },
  name: "App",
});
</script>

<style lang="scss">
:root {
  --text: #130206;
  --background: #fef6f8;
  --primary: #ec1a54;
  --secondary: #87f5dc;
  --accent: #5872f1;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 60px auto 0;
  max-width: 500px;
  display: block;
  text-align: center;
}

body {
  font-size: 16px;
  line-height: 1.2;
  color: var(--text);
  background-color: var(--background);
}

.weekday_selection {
  display: flex;
  flex-direction: column;
  columns: 2;
  gap: 0.5rem;
}

.weekday_selection button {
  display: block;
  margin: auto;
  padding: 0.5rem 1rem;
  font-size: 1.5rem;
  background-color: var(--accent);
  color: var(--background);
  border: none;
  border-radius: 0.25rem;
  cursor: pointer;
  transition: box-shadow 0.2s;
}

.weekday_selection button:hover {
  box-shadow: rgb(0, 0, 0, 0.7) 3px 3px 8px;
}

time {
  font-size: 2rem;
  color: var(--primary);
  margin-bottom: 2rem;
  display: inline-block;
}
</style>
