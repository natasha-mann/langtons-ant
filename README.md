# 🏆 Antics 🐜

The task this week is to create a simulation of Langton's Ant. Langton's Ant is a cellular automaton that moves around a two-dimensional grid of black and white cells. The ant can face one of four different directions - north, south, east or west. At each step of the simulation the ant will decide where to move - if the ant is currently on a black cell then it turns 90 degrees to the right and moves forward one cell. Similarly, if the ant is on a white cell then it turns 90 degrees to the left and moves forward one cell. Whenever the ant leaves a cell, it inverts the colour of that cell. You're free to choose the starting size and colour of your grid, as well as the starting direction and position of the ant.

### Overview

- This challenge was completed in Vue.js.

- The start size of the grid is 5 x 5 -> this can be amended by changing the ref values for rows and columns on lines 37 and 38 of Grid.vue.

- The ant is set to move every 250ms -> this can be amended by changing the value given to `setTimeout` on line 66 of Grid.vue.

- There are buttons which allow you to manually start and pause the simulation at any point. If the simulation is left to run, it will automatically end once the grid gets to a size of 16x16.

### To run the simulation

```sh
npm run dev
```

## Rewards:

5️⃣ Points are awarded for a working simulation of Langton's Ant with a simple user interface to display the grid and the ant's movement.

3️⃣ Further points are awarded for creating your simulation in a unique choice of language (or a unique frontend JavaScript framework).

2️⃣ Further points are awarded for expanding the grid by one cell in all directions if the ant attempts to move outside of the grid.

### Example

![img](./src/assets/langant.gif)
