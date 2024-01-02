<template>
  <main>
    <div id="sudokuBoard">
      <div
        v-for="(row, rowIndex) in board"
        :key="rowIndex"
        style="display: flex"
        :class="{ 'sudoku-row-divider': rowIndex % 3 === 2 }"
      >
        <div
          v-for="(tile, colIndex) in row"
          :key="colIndex"
          :class="{ 'sudoku-cell-divider': colIndex % 3 === 2 }"
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
    };
  },
  methods: {
    async fetchSudoku() {
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
    },
    solveSudoku() {
      const emptyCell = this.findEmptyCell();

      // If there are no empty cells, the Sudoku puzzle is solved
      if (!emptyCell) {
        return true;
      }

      const [row, col] = emptyCell;

      for (let num = 1; num <= 9; num++) {
        if (this.isValidMove(row, col, num)) {
          // Place the number if it's a valid move
          this.board[row][col] = num;

          // Recursively attempt to solve the rest of the puzzle
          if (this.solveSudoku()) {
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
  border: 3px solid transparent;
  transition: border 0.3s;

  -moz-appearance: textfield;
  appearance: none;
}

input:focus {
  border: 3px solid orange;
}

input::-webkit-inner-spin-button,
input::-webkit-outer-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

.sudoku-row-divider {
  margin-bottom: 10px; /* Adjust the size of the gap between the 3x3 fields vertically */
}

.sudoku-cell-divider {
  margin-right: 10px; /* Adjust the size of the gap between the 3x3 fields horizontally */
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
}
</style>
