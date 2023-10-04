<template>
  <div class="w-[120px] my-4 mx-auto">
    <Timer></Timer>
    <Board
      :initial="initial"
      :empties="empties"
      :btnInfos="btnInfos"
      @isPress="isPress"
    ></Board>
    <Button @click="activeBtn"></Button>
  </div>
</template>

<script setup lang="ts">
// ボタンの初期化
const initial = ref<boolean>(true);

let randomNums = ref<number[]>([]);

function randomNumsInfo() {
  randomNums.value = nums.value.sort(() => Math.random() - 0.5);
  btnInfos.value = [
    { id: 0, num: randomNums.value[0], isPress: false },
    { id: 1, num: randomNums.value[1], isPress: false },
    { id: 2, num: randomNums.value[2], isPress: false },
    { id: 3, num: randomNums.value[3], isPress: false },
  ];
}

// 初期パネルの生成
let nums = ref<number[]>([]);
for (let i = 0; i < 4; i++) {
  nums.value.push(i);
}
let btnInfos = ref<any[]>([]);
onMounted(() => {
  randomNumsInfo();
});

// 初期値用の殻のパネルの生成
const empties = ref<any[]>([]);
for (let i = 0; i < nums.value.length; i++) {
  empties.value.push("");
}

// スタートボタンを押した処理
let currentNumber = ref<number>(0);
const activeBtn = () => {
  currentNumber.value = 0;
  initial.value = false;
  randomNums.value = nums.value.sort(() => Math.random() - 0.5);
  randomNumsInfo();
};

// パネルを押した時の処理
const isPress = (index: number) => {
  if (currentNumber.value !== btnInfos.value[index].num) {
    return;
  }
  btnInfos.value[index].isPress = true;
  currentNumber.value++;
};
</script>
