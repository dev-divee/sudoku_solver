<template>
  <q-page class="flex flex-center column">
    <h1>Sudoku_Saga</h1>
    <button id="playNow">
      <router-link to="/play" class="link">Play Now</router-link>
    </button>
  </q-page>
</template>

<script></script>

<style scoped>
h1 {
  color: lightblue;
  font-size: 10rem;
}

.link {
  color: lightblue;
  text-decoration: none;
  transition: 0.3s;
}

.link:hover {
  color: rgb(0, 0, 20);
}

#playNow {
  cursor: pointer;
  margin-top: 150px;
  color: lightblue;
  border: 3px solid lightblue;
  border-radius: 15px;
  background: none;
  font-size: 5rem;
  padding: 0 30px;
  animation: pulse 1s infinite;
  transition: 0.3s;
}

#playNow:hover {
  background: lightblue;
  color: rgb(0, 0, 20);
}

@keyframes pulse {
  0% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.05);
  }
  100% {
    transform: scale(1);
  }
}
</style>
