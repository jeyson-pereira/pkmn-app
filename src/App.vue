<template>
  <div id="app">
    <Pokedex />
  </div>
</template>

<script>
import Pokedex from "./components/Pokedex.vue";
export default {
  components: { Pokedex },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css?family=Lobster");

body {
  margin: 0;
  padding: 0;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen,
    Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
}
</style>
