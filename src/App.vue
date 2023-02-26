<template>
  <div class="heading"><h1>Langton's Ant</h1></div>
  <div class="grid-container">
    <Grid :inPlay="inPlay" @stopGame="toggleInPlay" />
  </div>
  <div class="btn-container">
    <button class="start-btn" :class="{ inPlay }" @click="toggleInPlay">
      {{ inPlay ? "Stop" : "Start" }}
    </button>
    <button class="start-btn" @click="restartGame">Restart</button>
  </div>
</template>

<script setup>
import { ref } from "vue";
import Grid from "./components/Grid.vue";
const inPlay = ref(false);

const toggleInPlay = () => {
  inPlay.value = !inPlay.value;
};

const restartGame = () => {
  window.location.reload();
};
</script>

<style scoped>
.heading {
  border-bottom: grey 1px solid;
  display: flex;
  justify-content: center;
  padding-bottom: 2rem;
}

.grid-container {
  display: flex;
  justify-content: center;
  align-content: center;
  margin-top: 5rem;
  margin-bottom: 3rem;
}

.btn-container {
  display: flex;
  justify-content: center;
}

.start-btn {
  width: 10%;
  margin: 10px;
  padding: 1rem 1.5rem;
  background-color: black;
  color: azure;
  font-weight: bold;
  box-shadow: 2px 3px 5px grey;
}

.start-btn:hover {
  cursor: pointer;
  color: black;
  background-color: azure;
}

.inPlay {
  background-color: green !important;
}
</style>
