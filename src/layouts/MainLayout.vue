<template>
  <q-layout view="lHh Lpr lFf">
    <q-header>
      <q-toolbar class="text-lightblue">
        <h1 style="color: lightblue; top: 20px; left: 20px">Sudoku_Saga</h1>
        <q-btn flat round dense style="font-size: 2rem" id="menuBtn">
          <q-icon name="menu" id="menu" />
        </q-btn>
        <nav>
          <ul>
            <router-link to="/play" class="nav-links">ranked</router-link>
          </ul>
          <ul>
            <router-link to="/" class="nav-links">solver</router-link>
          </ul>
          <ul>
            <router-link to="/" class="nav-links">contact</router-link>
          </ul>
        </nav>
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

nav {
  display: flex;
  gap: 100px;
}

.nav-links {
  font-size: 3rem;
  text-decoration: none;
  color: lightblue;
  position: relative;
}

.nav-links::after {
  content: "";
  position: absolute;
  left: 0;
  bottom: -2px;
  height: 2px;
  width: 100%;
  transform: scaleX(0);
  transition: transform 0.3s;
  background: lightblue;
}

.nav-links:hover::after {
  transform: scaleX(1);
}

@media screen and (min-width: 768px) {
  #menuBtn {
    display: none;
  }
}
</style>
