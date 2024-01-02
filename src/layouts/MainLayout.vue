<template>
  <q-layout view="lHh Lpr lFf">
    <q-header>
      <q-toolbar class="text-lightblue">
        <h1 style="color: lightblue; top: 20px; left: 20px">Sudoku_Solver</h1>
        <q-btn flat round dense style="font-size: 2rem" id="menuBtn">
          <q-icon name="menu" />
        </q-btn>
      </q-toolbar>
    </q-header>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
export default {
  name: "MainLayout",
};
</script>

<style>
*,
*::before,
*::after {
  margin: 0;
  padding: 0;
  font-family: "Courier New", Courier, monospace;
}
body {
  background: rgb(0, 0, 20);
}
.q-header {
  background: rgb(0, 0, 20);
  position: absolute;
  top: 0;
  width: 100%;
  border-bottom: 1px solid lightblue;
  padding: 20px;
}
.q-toolbar {
  display: flex;
  justify-content: space-between;
  color: lightblue;
}
</style>
