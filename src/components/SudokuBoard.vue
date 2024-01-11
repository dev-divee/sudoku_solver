<template>
  <main>
    <div class="q-pa-md q-gutter-sm">
      <q-dialog v-model="showPopup" persistent>
        <q-card id="ratingDialog">
          <q-card-section class="row column">
            <p>Please take a few seconds and rate our website!</p>
            <q-rating
              v-model="ratingModel"
              max="5"
              size="3em"
              color="red"
              color-selected="red-9"
              icon="favorite_border"
              icon-selected="favorite"
              icon-half="favorite"
            />
          </q-card-section>

          <q-card-actions align="right">
            <q-btn flat label="No, thanks" v-close-popup />
            <q-btn flat label="Submit" v-close-popup />
          </q-card-actions>
        </q-card>
      </q-dialog>
    </div>

    <div v-if="loading" class="loading-overlay"></div>
    <div id="sudokuBoard" :class="{ greenBorder: win }">
      <div
        v-for="(row, rowIndex) in board"
        :key="rowIndex"
        style="display: flex"
        :class="{ 'sudoku-row-divider': (rowIndex + 1) % 3 === 0 }"
      >
        <div
          v-for="(tile, colIndex) in row"
          :key="colIndex"
          :class="{ 'sudoku-cell-divider': (colIndex + 1) % 3 === 0 }"
        >
          <input
            type="number"
            v-model="board[rowIndex][colIndex]"
            @change="logBoard"
            min="1"
            max="9"
          />
        </div>
      </div>
      <q-spinner-clock
        v-if="loading"
        color="orange"
        size="15rem"
        id="spinner"
      />
    </div>
    <div class="buttons">
      <button id="solveSudoku" @click="solveSudoku">Solve</button>
      <button id="getSudoku" @click="fetchSudoku">Get Sudoku</button>
    </div>
  </main>
</template>

<script>
export default {
  data() {
    return {
      board: [
        [null, null, null, null, null, null, null, null, null],
        [null, null, null, null, null, null, null, null, null],
        [null, null, null, null, null, null, null, null, null],
        [null, null, null, null, null, null, null, null, null],
        [null, null, null, null, null, null, null, null, null],
        [null, null, null, null, null, null, null, null, null],
        [null, null, null, null, null, null, null, null, null],
        [null, null, null, null, null, null, null, null, null],
        [null, null, null, null, null, null, null, null, null],
      ],
      win: false,
      loading: false,
      showPopup: false,
      ratingModel: 5,
    };
  },
  methods: {
    async fetchSudoku() {
      this.loading = true;
      const data = await fetch(
        `https://sudoku-api.vercel.app/api/dosuku?query={newboard(limit:1){grids{value,solution,difficulty},results,message}}`
      );
      const result = await data.json();

      console.log(result);

      const originalSudoku = result.newboard.grids[0].value;
      console.log(originalSudoku);
      const modifiedSudoku = originalSudoku.map((row) => {
        return row.map((tile) => (tile === 0 ? null : tile));
      });
      console.log(modifiedSudoku);
      this.board = modifiedSudoku;
      this.loading = false;
      this.win = false;
    },
    solveSudoku() {
      const emptyCell = this.findEmptyCell();

      // If there are no empty cells, the Sudoku puzzle is solved
      if (!emptyCell) {
        console.log("winner winner chicken dinner");
        return true;
      }

      const [row, col] = emptyCell;

      for (let num = 1; num <= 9; num++) {
        if (this.isValidMove(row, col, num)) {
          // Place the number if it's a valid move
          this.board[row][col] = num;

          // Recursively attempt to solve the rest of the puzzle
          if (this.solveSudoku()) {
            this.win = true;
            return true;
          }

          // If placing the number leads to an invalid solution, backtrack
          this.board[row][col] = null;
        }
      }

      // No valid number found, trigger backtracking
      return false;
    },
    findEmptyCell() {
      for (let row = 0; row < 9; row++) {
        for (let col = 0; col < 9; col++) {
          if (this.board[row][col] === null) {
            return [row, col];
          }
        }
      }
      return null; // No empty cell found, puzzle is solved
    },
    isValidMove(row, col, num) {
      // Check if the number is not in the same row or column
      for (let i = 0; i < 9; i++) {
        if (this.board[row][i] === num || this.board[i][col] === num) {
          return false;
        }
      }

      // Check if the number is not in the same 3x3 subgrid
      const startRow = Math.floor(row / 3) * 3;
      const startCol = Math.floor(col / 3) * 3;
      for (let i = 0; i < 3; i++) {
        for (let j = 0; j < 3; j++) {
          if (this.board[startRow + i][startCol + j] === num) {
            return false;
          }
        }
      }

      // The move is valid
      return true;
    },
    arraysEqual(arr1, arr2) {
      if (arr1.length !== arr2.length) {
        return false;
      }

      for (let i = 0; i < arr1.length; i++) {
        if (arr1[i] !== arr2[i]) {
          return false;
        }
      }

      return true;
    },
  },
  mounted() {
    setTimeout(() => {
      this.showPopup = true;
    }, 2000);
  },
};
</script>

<style scoped>
main {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 50px;
}

input {
  width: 100px;
  height: 100px;
  background: white;
  color: rgb(0, 0, 20);
  border: none;
  margin: 3px;
  font-size: 2.5rem;
  font-weight: bold;
  text-align: center;
  outline: none;
  border: 5px solid transparent;
  transition: border 0.3s ease-in-out;

  -moz-appearance: textfield;
  appearance: none;
}

input:focus {
  border: 5px solid orange;
}

input::-webkit-inner-spin-button,
input::-webkit-outer-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

#sudokuBoard {
  z-index: 1;
  position: relative;
  border: 5px solid white;
  padding: 20px;
}

#sudokuBoard.greenBorder {
  border: 5px solid lightgreen;
}

.sudoku-row-divider {
  margin-bottom: 10px; /* Adjust the size of the gap between the 3x3 fields vertically */
}

.sudoku-row-divider:nth-child(9) {
  margin: 0;
}

.sudoku-cell-divider {
  margin-right: 10px; /* Adjust the size of the gap between the 3x3 fields horizontally */
}

.sudoku-cell-divider:nth-child(9) {
  margin: 0;
}

button {
  margin-top: 50px;
  cursor: pointer;
}

.buttons {
  display: flex;
  gap: 100px;
}
.buttons button {
  font-size: 2rem;
  border: 3px solid lightblue;
  border-radius: 10px;
  padding: 5px 20px;
  background: rgb(0, 0, 20);
  color: lightblue;
  font-size: 3rem;
  font-weight: bold;
  transition: background 0.3s, color 0.3s;
}

.buttons button:hover {
  /* border: 3px solid lightgreen; */
  background: lightblue;
  color: rgb(0, 0, 20);
}

#spinner {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 3;
}

#ratingDialog {
  background: rgb(0, 0, 20);
  border: 3px solid lightblue;
  color: lightblue;
  padding: 20px;
  border-radius: 15px;
}

#ratingDialog p {
  font-size: 1.5rem;
  margin-bottom: 30px;
}

#ratingDialog button {
  background: rgb(0, 0, 20);
  color: lightblue;
  font-size: 1.2rem;
  padding: 0 20px;
}
.loading-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(255, 165, 0, 0.1);
  z-index: 2;
}
</style>
