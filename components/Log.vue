<template>
  <div
    class="text-black bg-white w-60 h-40 border border-black rounded-[4px] mt-5"
  >
    <h2 class="text-center my-2">Best Score ({{ level }})</h2>
    <ul class="text-center">
      <li v-for="(score, index) in scores" :key="score">
        {{ index + 1 }}: {{ score }}
      </li>
    </ul>
  </div>
</template>

<script setup lang="ts">
const props = defineProps(["scores", "level"]);
</script>
