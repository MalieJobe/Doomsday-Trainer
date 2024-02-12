<template>
  <h1>Doomsday Trainer</h1>
  <time :datetime="dateString"
    ><strong>{{ dateString }}</strong></time
  >
  <table style="text-align: left; width: 100%">
    <tr>
      <th scope="col">Stopwatch</th>
      <th scope="col">Best Guess</th>
      <th scope="col">Streak</th>
      <th scope="col">Status</th>
    </tr>

    <tr>
      <td>
        {{
          stopwatch < 60
            ? stopwatch
            : Math.floor(stopwatch / 60) + "m " + (stopwatch % 60)
        }}s
      </td>
      <td>
        {{
          bestGuessTime < 60
            ? bestGuessTime
            : Math.floor(bestGuessTime / 60) + "m " + (bestGuessTime % 60)
        }}s
      </td>
      <td>{{ streak }}</td>
      <td>{{ status }}</td>
    </tr>
  </table>
  <div class="weekday_selection">
    <button
      v-for="(weekday, index) in weekdays"
      :key="index"
      @click="guess(index)"
      :class="{ inactive: wrongGuesses.includes(index) }"
    >
      {{ weekday }}
    </button>
  </div>
  <button
    id="next"
    :class="{ inactive: status !== 'Correct!' }"
    @click="nextDate()"
  >
    Next Date
  </button>
  <h2>How does this work?</h2>
  <p>
    Check out Numberphiles
    <a
      href="https://www.youtube.com/watch?v=z2x3SSBVGJU&ab_channel=Numberphile"
      target="_blank"
      >The Doomsday Algorithm</a
    >
    on Youtube. With some basic math and some practice you'll be able to tell
    the weekday of any Date given to you
  </p>
</template>

<script setup lang="ts">
import { computed, reactive, ref } from "vue";

const status = ref<"" | "Correct!" | "Incorrect!">("");
const streak = ref<number>(0);
const stopwatch = ref<number>(58);
const bestGuessTime = ref<number>(0);
const date = ref(getRandomDate(1900, 2100));
const dateString = computed(() => {
  return date.value.toLocaleString("en-us", {
    month: "short",
    year: "numeric",
    day: "numeric",
  });
});

const stopWatchInterval = setInterval(() => {
  stopwatch.value += 1;
}, 1000);

const wrongGuesses = reactive<Array<number>>([]);

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
  if (date.value.getDay() === weekday) {
    status.value = "Correct!";
    if (stopwatch.value > bestGuessTime.value)
      bestGuessTime.value = stopwatch.value;
  } else {
    wrongGuesses.push(weekday);
    status.value = "Incorrect!";
  }
}

function nextDate() {
  date.value = getRandomDate(1900, 2100);
  wrongGuesses.splice(0);
  status.value = "";
  stopwatch.value = 0;
}

function getRandomDate(startYear: number, endYear: number) {
  const startTimestamp = new Date(`${startYear}-01-01`).getTime();
  const endTimestamp = new Date(`${endYear}-12-31`).getTime();
  const randomTimestamp = Math.floor(
    Math.random() * (endTimestamp - startTimestamp + 1) + startTimestamp
  );
  return new Date(randomTimestamp);
}
</script>

<style>
:root {
  --text: #240e03;
  --background: #ffebe0;
  --primary: #ee5719;
  --secondary: #c0f471;
  --accent: #7df258;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 60px auto 0;
  max-width: 400px;
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
  flex-direction: row;
  columns: 2;
  gap: 0.5rem;
  flex-wrap: wrap;
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
  flex-basis: 48%;
}

.weekday_selection button.inactive {
  background-color: grey;
  cursor: not-allowed;
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
