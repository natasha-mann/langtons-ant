<template>
  <table id="gameboard" class="wrapper">
    <tbody>
      <tr v-for="(row, rowIndex) in rowArray" class="row" :key="rowIndex">
        <td
          v-for="(col, colIndex) in colArray"
          class="square"
          :data-coordinates="`${row},${col}`"
          :data-row="row"
          :data-col="col"
          :key="colIndex"
          :class="[
            antDirectionClass,
            { antSquare: antPosition.row === row && antPosition.col === col },
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

const rows = ref(6);
const columns = ref(5);
const antPosition = ref({ row: 2, col: 2 });
const antDirection = ref(90);
const blackSquares = ref([]);

const antDirectionClass = computed(() => {
  switch (antDirection.value) {
    case 0:
      return "northAnt";
    case 90:
      return "eastAnt";
    case 180:
      return "southAnt";
    case 270:
      return "westAnt";
  }
});

const rowArray = computed(() => {
  return Array.from(Array(rows.value).keys());
});

const colArray = computed(() => {
  return Array.from(Array(columns.value).keys());
});

const moveAnt = () => {
  if (props.inPlay) {
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

    const extendGrid = () => {
      if (rows.value >= 16 || columns.value >= 15) {
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
  }
};

onUpdated(() => {
  if (props.inPlay) {
    setTimeout(moveAnt, 200);
  }
});
</script>

<style scoped>
.wrapper {
  border-spacing: 0px;
  border: 1px solid black;
}

.square {
  width: 70px;
  height: 70px;
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
  background-image: url("../assets/images/ant-black.svg");
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
