<template>
  <div>
    <button
      class="bg-[#f44336] p-2 w-full rounded-[4px] text-center shadow-[0_4px_0_#d1483e] active:mt-1 active:shadow-none"
      @click="activeBtn"
    >
      START
    </button>
  </div>
</template>

<script setup lang="ts">
const emits = defineEmits(["activeBtn"]);
const activeBtn = () => {
  emits("activeBtn");
};
</script>
