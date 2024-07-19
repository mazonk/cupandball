<template>
    <div class="flex flex-col items-center justify-center h-screen">
      <h1 class="text-2xl font-bold mb-4">Cup and Ball Game</h1>
      <div class="flex space-x-4 relative">
        <div
          v-for="(cup, index) in cups"
          :key="index"
          class="relative"
          @click="selectCup(index)"
        >
          <img
            src="src/assets/cup.jpg"
            class="w-24 h-24 transform transition-transform duration-500"
            :class="{
              'translate-y-0': selectedCup === index || showBallIndex === index,
              'translate-y-8': selectedCup !== index && showBallIndex !== index
            }"
          />
          <img
            v-if="showBall && ballPosition === index"
            src="src\assets\ball.jpg"
            class="w-12 h-12 absolute bottom-0 left-1/2 transform -translate-x-1/2"
          />
        </div>
      </div>
      <button
        class="mt-8 px-4 py-2 bg-blue-500 text-white rounded"
        @click="startGame"
      >
        Start Game
      </button>
      <p class="mt-4 text-lg">Score: {{ score }}</p>
    </div>
  </template>
  
  <script lang="ts">
  import { defineComponent, ref } from 'vue';
  
  export default defineComponent({
    name: 'CupAndBallGame',
    setup() {
      const cups = ref([0, 1, 2]);
      const ballPosition = ref<number | null>(null);
      const selectedCup = ref<number | null>(null);
      const score = ref(0);
      const showBall = ref(true);
  
      const shuffleCups = async () => {
        for (let i = 0; i < 5; i++) {
          await new Promise<void>((resolve) =>
            setTimeout(() => {
              const [a, b] = [
                Math.floor(Math.random() * 3),
                Math.floor(Math.random() * 3),
              ];
              if (a !== b) {
                [cups.value[a], cups.value[b]] = [cups.value[b], cups.value[a]];
              }
              resolve();
            }, 500)
          );
        }
      };
  
      const startGame = async () => {
        showBall.value = true;
        ballPosition.value = Math.floor(Math.random() * 3);
        await new Promise((resolve) => setTimeout(resolve, 2000));
        showBall.value = false;
        await shuffleCups();
        selectedCup.value = null;
      };
  
      const selectCup = (index: number) => {
        if (selectedCup.value === null) {
          selectedCup.value = index;
          if (index === cups.value.indexOf(ballPosition.value!)) {
            score.value += 1;
          }
        }
      };
  
      return {
        cups,
        ballPosition,
        selectedCup,
        score,
        showBall,
        startGame,
        selectCup,
      };
    },
  });
  </script>
  
  <style scoped>
  </style>
  