<template>
  <navbar></navbar>
  <generate-api />
  <note></note>
</template>

<script>
import GenerateApi from "./components/GenerateApi.vue";
import Navbar from "./components/Navbar.vue";
import Note from "./components/Note.vue";

export default {
  name: "App",
  components: {
    GenerateApi,
    Navbar,
    Note,
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
