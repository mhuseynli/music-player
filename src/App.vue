<template>
  <div id="app">
    <header>
      <h1>Music Player</h1>
    </header>
    <main>
      <Player />
    </main>
  </div>
</template>

<script>
import Player from "./components/Player.vue";
export default {
  name: "App",
  components: {
    Player,
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Roboto:ital,wght@0,100;0,300;0,400;0,500;0,700;0,900;1,100;1,300;1,400;1,500;1,700;1,900&display=swap");
* {
  font-family: "Roboto", sans-serif;
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  -webkit-tap-highlight-color: transparent;
}
header {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 15px;
  background-color: #212121;
  color: #fff;
}
body {
  background-color: #363636;
}
main {
  padding: 20px;
}
header h1 {
  font-weight: 100;
}
</style>
