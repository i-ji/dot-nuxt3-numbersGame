<template>
  <div class="p-[10px] bg-white rounded-[4px] mb-2">
    <ul class="flex flex-wrap">
      <li
        class="bg-[#ccc] w-10 h-10 m-[5px] rounded-[4px]"
        v-for="empty in empties"
        :key="empty"
        v-show="initial"
      >
        {{ empty }}
      </li>
      <li
        class="bg-[#00aaff] w-10 h-10 m-[5px] cursor-pointer rounded-[4px] leading-10 text-center shadow-[0_4px_0_#0088cc]"
        v-for="(num, index) in randomNums"
        :key="num"
        :class="{ pressed: btnState }"
        v-show="!initial"
        @click="pushBtn(num, index)"
      >
        {{ num }}
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
const props = defineProps(["randomNums", "empties", "initial", "btnState"]);
const emits = defineEmits(["pushBtn"]);
const pushBtn = (num: number, index: number) => {
  emits("pushBtn", num, index);
};
</script>
