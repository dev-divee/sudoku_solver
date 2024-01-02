<template>
  <q-page class="flex flex-center">
    <sudoku-main />
  </q-page>
</template>

<script>
import { defineComponent } from "vue";
import SudokuMain from "src/components/SudokuMain.vue";

export default defineComponent({
  name: "IndexPage",
  components: {
    "sudoku-main": SudokuMain,
  },
});
</script>
