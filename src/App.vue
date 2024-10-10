<template>
  <div id="app">
    <GridBroad />
  </div>
</template>

<script>
import GridBroad from "./components/GridBroad.vue";
export default {
  components: {
    GridBroad,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>