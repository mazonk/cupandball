<template>
  <div id="app">
    <CupAndBallGame />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import CupAndBallGame from './components/CupAndBallGame.vue';

export default defineComponent({
  name: 'App',
  components: {
    CupAndBallGame,
  },
});
</script>

<style>
body {
  @apply bg-white;
}
</style>
