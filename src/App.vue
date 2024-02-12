<template>
  <h1 class="title">Doomsday Trainer</h1>
  <time :datetime="dateString" class="date">{{ dateString }}</time>
  <table class="stats">
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
  <div class="options">
    <button
      v-for="(weekday, index) in weekdays"
      :key="index"
      @click="guess(index)"
      :class="{ inactive: wrongGuesses.includes(index) }"
      class="btn-secondary"
    >
      {{ weekday }}
    </button>
  </div>
  <button
    class="btn-primary"
    :class="{ inactive: status !== 'Correct!' }"
    @click="nextDate()"
  >
    Next Date
  </button>
  <footer>
    <h2 class="subtitle">How does this work?</h2>
    <p>
      Check out Numberphiles
      <a
        href="https://www.youtube.com/watch?v=z2x3SSBVGJU&ab_channel=Numberphile"
        target="_blank"
        >"The Doomsday Algorithm" on Youtube</a
      >
      . With some basic math and some practice you'll be able to tell the
      weekday of any Date given to you
    </p>
  </footer>
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

setInterval(() => {
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
  --text: #020f0d;
  --background: #f5fdfc;
  --primary: #25dec4;
  --secondary: #90a1ee;
  --accent: #6d52e4;
}

#app {
  margin: 0 auto;
  max-width: 400px;
  display: block;
  padding: 0 1rem;
}

body {
  font-family: Arial, Helvetica, sans-serif;
  text-align: center;
  font-size: 16px;
  line-height: 1.2;
  color: var(--text);
  background-color: var(--background);
  margin: 0;
}

.title {
  font-size: 1.5rem;
  color: var(--text);
  padding: 1rem 0;
  margin-top: 0;
  border-bottom: 1px solid;
}

.date {
  font-weight: bold;
  font-size: 2rem;
  color: var(--accent);
  margin: 2rem auto 1rem;
  display: inline-block;
}

.stats {
  width: 100%;
  border: 1px solid;
  margin-bottom: 3rem;
}

td:not(:first-child),
th:not(:first-child) {
  border-left: 1px solid;
}
th {
  border-bottom: 1px solid;
}

.options {
  display: flex;
  flex-direction: row;
  gap: 1rem;
  flex-wrap: wrap;
  margin-bottom: 3rem;
}

button {
  display: block;
  margin: auto;
  padding: 0.75rem 1rem;
  font-size: 1.5rem;
  border: none;
  border-radius: 0.25rem;
  cursor: pointer;
  transition: all 0.2s;
  box-shadow: 4px 4px 5px rgb(0 0 0 / 30%);
}

button.inactive {
  background-color: #999999;
  cursor: not-allowed;
  box-shadow: none;
  color: var(--text);
}

.btn-secondary {
  border: 1px solid black;
  color: var(--accent);
  transition: box-shadow 0.2s;
}

.options button {
  flex-basis: calc(50% - 0.5rem); /* because of flex gap */
}

.options button:first-child {
  flex-basis: 55%; /* so sunday is on top */
}

.btn-primary {
  background-color: var(--accent);
  color: var(--background);
}

footer {
  margin-top: 4rem;
}
</style>
