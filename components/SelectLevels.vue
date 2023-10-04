<template>
  <div>
    <ul class="flex justify-between w-52 mx-auto mt-4 text-xl">
      <li
        class="cursor-pointer"
        :class="{ selected: levelInfo.selectedLevel }"
        v-for="(levelInfo, index) in levelInfos"
        :key="levelInfo"
        @click="changeLevel(index)"
      >
        {{ levelInfo.text }}
      </li>
    </ul>
  </div>
</template>

<style>
.selected {
  color: black;
}
</style>

<script setup lang="ts">
const props = defineProps(["levelInfos"]);
const emits = defineEmits(["changeLevel"]);
const changeLevel = (index: number) => {
  emits("changeLevel", index);
};
</script>
