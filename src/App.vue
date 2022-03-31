<template>
  <generate-api />
</template>

<script>
import GenerateApi from "./components/GenerateApi.vue";

export default {
  name: "App",
  components: {
    GenerateApi,
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
