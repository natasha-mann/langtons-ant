<template>
  <div class="heading"><h1>Langton's Ant</h1></div>
  <div class="btn-container">
    <button class="start-btn" :class="{ inPlay }" @click="toggleInPlay">
      {{ inPlay ? "Pause" : "Start" }}
    </button>
    <button class="start-btn" @click="restartGame">Restart</button>
  </div>
  <div class="grid-container">
    <Grid :inPlay="inPlay" @endGame="() => (gameOver = true)" />
    <div v-if="gameOver" class="game-over">
      <h3 class="game-over-text">
        Oh no! The grid is too big! Hit restart to play again.
      </h3>
      <button class="restart-btn" @click="restartGame">RESTART!</button>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import Grid from "./components/Grid.vue";
const inPlay = ref(false);

const toggleInPlay = () => {
  inPlay.value = !inPlay.value;
};

const gameOver = ref(false);

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

.btn-container {
  display: flex;
  justify-content: center;
  margin-top: 1rem;
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

.start-btn:hover,
.restart-btn:hover {
  cursor: pointer;
  color: black;
  background-color: azure;
}

.inPlay {
  background-color: green !important;
}

.grid-container {
  display: flex;
  justify-content: center;
  align-content: center;
  margin-top: 2rem;
  margin-bottom: 3rem;
}

.game-over {
  position: absolute;
  background-color: red;
  height: 50%;
  width: 50%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.game-over-text {
  font-weight: bold;
  margin-bottom: 2rem;
}

.restart-btn {
  width: 30%;
  margin: 10px;
  padding: 1rem 1.5rem;
  background-color: black;
  color: azure;
  font-weight: bold;
  box-shadow: 2px 3px 5px grey;
}
</style>
