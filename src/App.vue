<template>
  <div id="app">
    <img class="mx-auto" alt="Vue logo" src="./assets/logo.png" />
    <!-- <HelloWorld msg="Welcome to Your Vue.js App" /> -->
    <SearchInput />
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";
import SearchInput from "./components/SearchInput.vue";

export default {
  name: "App",
  components: {
    HelloWorld,
    SearchInput,
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
