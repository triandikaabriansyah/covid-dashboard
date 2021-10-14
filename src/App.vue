<template>
  <div id="app">
    <v-app color="primary" dark>
      <Covid />
    </v-app>
  </div>
</template>

<script>
import Covid from "./components/Covid.vue";

export default {
  name: "App",
  components: {
    Covid,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #fff;
  margin-top: 60px;
  background-color: black;
}
body {
  background-color: black;
}
</style>
