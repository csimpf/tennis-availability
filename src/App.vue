<template>
  <TennisTable />
</template>

<script>
import TennisTable from "./components/TennisTable.vue";

export default {
  name: "App",
  components: {
    TennisTable,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #1c110a;
  background-color: #e9b44c;
}
</style>
