<template>
  <div id="app">
    <LocalWeather msg="Welcome to Your Vue.js App" />
  </div>
</template>

<script>
import LocalWeather from "./components/LocalWeather.vue";

export default {
  name: "app",
  components: {
    LocalWeather
  }
};
</script>

<style lang="scss">
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: white;
  background: black;
  min-height: 100vh;
  overflow: hidden;
}
</style>
