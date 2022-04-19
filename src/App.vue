<template>
  <h1 class="title">"{{ title }}"</h1>
  <MineSweep v-if="renderComponent" @reload="forceRerender" :number="number" />
</template>

<script>
import MineSweep from "./components/MineSweep.vue";
export default {
  name: "App",

  components: { MineSweep },
  data() {
    return {
      title: "MINE SWEEPER",
      renderComponent: true,
      number: 5,
    };
  },
  methods: {
    forceRerender() {
      this.renderComponent = false;
      this.$nextTick(() => {
        this.renderComponent = true;
      });
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #ffffff;
  margin-top: 60px;
}

.title {
  color: #ffffff;
}
</style>
