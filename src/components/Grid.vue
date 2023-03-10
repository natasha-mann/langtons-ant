<template>
  <table id="gameboard" class="wrapper">
    <tbody>
      <tr v-for="(row, rowIndex) in rowArray" class="row" :key="rowIndex">
        <td
          v-for="(col, colIndex) in colArray"
          class="square"
          :data-coordinates="`${row},${col}`"
          :key="colIndex"
          :class="[
            {
              northAnt: isCurrentSquare(row, col) && antDirection === 0,
            },
            {
              southAnt: isCurrentSquare(row, col) && antDirection === 180,
            },
            {
              westAnt: isCurrentSquare(row, col) && antDirection === 270,
            },

            { antSquare: isCurrentSquare(row, col) },
            {
              blackSquare: blackSquares.some(
                (obj) => obj.row === row && obj.col === col
              ),
            },
            {
              whiteSquare: !blackSquares.some(
                (obj) => obj.row === row && obj.col === col
              ),
            },
          ]"
        ></td>
      </tr>
    </tbody>
  </table>
</template>

<script setup>
import { computed, onUpdated, ref } from "vue";

const emit = defineEmits(["endGame"]);

const props = defineProps({ inPlay: Boolean });

const rows = ref(5);
const columns = ref(5);
const antPosition = ref({ row: 2, col: 2 });
const antDirection = ref(90);
const blackSquares = ref([]);

const isCurrentSquare = (row, col) =>
  antPosition.value.row === row && antPosition.value.col === col;

const rowArray = computed(() => {
  return Array.from(Array(rows.value).keys());
});

const colArray = computed(() => {
  return Array.from(Array(columns.value).keys());
});

onUpdated(() => {
  if (props.inPlay) {
    setTimeout(moveAnt, 250);
  }
});

const extendGrid = () => {
  if (rows.value >= 16 || columns.value >= 16) {
    emit("endGame");
    return;
  }
  rows.value += 2;
  columns.value += 2;
  antPosition.value.row += 1;
  antPosition.value.col += 1;
  const newBlackSquares = blackSquares.value.map(({ row, col }) => ({
    row: row + 1,
    col: col + 1,
  }));
  blackSquares.value = newBlackSquares;
};

const moveAnt = () => {
  const currentSquare = document.querySelector(
    `[data-coordinates="${antPosition.value.row},${antPosition.value.col}"]`
  );

  if (currentSquare.classList.contains("whiteSquare")) {
    blackSquares.value.push({
      row: antPosition.value.row,
      col: antPosition.value.col,
    });

    antDirection.value === 0
      ? (antDirection.value = 270)
      : (antDirection.value -= 90);
  } else {
    const newBlackSquares = blackSquares.value.filter((obj) => {
      if (
        obj.row === antPosition.value.row &&
        obj.col === antPosition.value.col
      ) {
        return false;
      }
      return true;
    });

    blackSquares.value = newBlackSquares;

    antDirection.value === 270
      ? (antDirection.value = 0)
      : (antDirection.value += 90);
  }

  switch (antDirection.value) {
    case 0:
      if (antPosition.value.row === 0) {
        extendGrid();
      }
      antPosition.value.row -= 1;
      break;
    case 90:
      if (antPosition.value.col === columns.value - 1) {
        extendGrid();
      }
      antPosition.value.col += 1;
      break;
    case 180:
      if (antPosition.value.row === rows.value - 1) {
        extendGrid();
      }
      antPosition.value.row += 1;
      break;
    case 270:
      if (antPosition.value.col === 0) {
        extendGrid();
      }
      antPosition.value.col -= 1;
      break;
  }
};
</script>

<style scoped>
.wrapper {
  border-spacing: 0px;
  border: 1px solid black;
}

.square {
  width: 45px;
  height: 45px;
  min-width: 30px;
  min-height: 30px;
  margin: 0;
  padding: 0;
  border: 1px solid black;
}

.whiteSquare {
  background-color: white;
}

.blackSquare {
  background-color: black;
}

.antSquare {
  background-image: url("../assets/images/ant.svg");
  background-size: contain;
  background-position: center;
  background-repeat: no-repeat;
}

.southAnt {
  transform: rotate(0.25turn);
}

.westAnt {
  transform: rotate(0.5turn);
}

.northAnt {
  transform: rotate(0.75turn);
}
</style>
