<template>
  <h1 class="title">"{{ title }}"</h1>
  <MineSweep />
</template>

<script>
import MineSweep from "./components/MineSweep.vue";
export default {
  name: "App",
  components: { MineSweep },
  data() {
    return {
      title: "MINE SWEEPER",
    };
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

.title {
  color: #2c3e50;
}
</style>
