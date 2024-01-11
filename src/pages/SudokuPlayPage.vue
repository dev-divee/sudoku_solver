<template>
  <q-page>
    <sudoku-board />
  </q-page>
</template>

<script>
import SudokuBoard from "src/components/SudokuBoard.vue";

export default {
  components: {
    "sudoku-board": SudokuBoard,
  },
};
</script>

<style></style>
