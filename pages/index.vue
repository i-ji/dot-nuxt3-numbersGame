<template>
  <div class="w-[120px] my-4 mx-auto">
    <Timer></Timer>
    <Board
      :randomNums="randomNums"
      :initial="initial"
      :empties="empties"
      :btnState="btnState"
      @pushBtn="pushBtn"
    ></Board>
    <Button @activeBtn="activeBtn"></Button>
  </div>
</template>

<script setup lang="ts">
// ボタンの初期化
const initial = ref<boolean>(true);

// スタートボタンの処理
const btnState = ref<boolean>(true);
const activeBtn = () => {
  btnState.value = false;
  initial.value = false;
};

const nums = ref<number[]>([0, 1, 2, 3]);
let randomNums = ref<any[]>([]);

// 初期値用の殻のパネルの生成
const empties = ref<any[]>([]);
for (let i = 0; i < nums.value.length; i++) {
  empties.value.push("");
}

// パネルをクリックした時の処理
let currentNumber = ref<number>(0);
const pushBtn = (num: number, index: number) => {
  if (currentNumber.value !== num) {
    return;
  }

  // btnState.value = true;
  console.log(num + " num");
  console.log(index + " index");
  //   randomNums.value[index];

  currentNumber.value++;
};

// ランダムな数字を生成
onMounted(() => {
  randomNums.value = nums.value.sort(() => Math.random() - 0.5);
});
</script>
