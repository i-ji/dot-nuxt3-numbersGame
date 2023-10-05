<template>
  <div class="p-[10px] bg-white rounded-[4px] mb-4">
    <ul class="flex flex-wrap justify-between">
      <li
        class="bg-[#ccc] w-10 h-10 m-[5px] rounded-[4px]"
        v-for="empty in empties"
        :key="empty"
        v-show="initial"
      >
        {{ empty }}
      </li>
      <li
        v-show="!initial"
        class="bg-[#00aaff] w-10 h-10 m-[5px] cursor-pointer rounded-[4px] leading-10 text-center shadow-[0_4px_0_#0088cc]"
        v-for="(btnInfo, index) in btnInfos"
        :key="btnInfo.id"
        @click="isPress(index)"
        :class="{ pressed: btnInfo.isPress }"
      >
        {{ btnInfo.num }}
      </li>
    </ul>
  </div>
</template>

<style>
.pressed {
  background: #ccc;
  box-shadow: none;
  margin-top: 9px;
  margin-bottom: 1px;
}
</style>

<script setup lang="ts">
const props = defineProps(["empties", "initial", "btnInfos"]);
const emits = defineEmits(["isPress"]);
const isPress = (index: number) => {
  emits("isPress", index);
};
</script>
