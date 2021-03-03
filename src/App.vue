<template>
  <div id="app">
    <landing-page />
  </div>
</template>

<script>
import LandingPage from "./components/landing-page.vue";

export default {
  name: "App",
  components: {
    LandingPage,
  },
};
</script>

<style>
#app {
  font-family: Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
