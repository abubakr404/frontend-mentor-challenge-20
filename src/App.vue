<script>
import TipCalculator from "./components/TipCalculator.vue";
export default {
  components: {
    TipCalculator,
  },
};
</script>

<template>
  <header>
    <img alt="Tip calculator logo" class="logo" src="./assets/logo.svg" />
  </header>

  <main class="container">
    <TipCalculator />
  </main>
</template>

<style>
@import "./assets/main.css";
</style>
