<template>
  <div>
    <SelectLevels
      :levelInfos="levelInfos"
      @changeLevel="changeLevel"
    ></SelectLevels>
    <div class="w-[120px] my-4 mx-auto">
      <Timer :timer="timer"></Timer>
      <Board
        :initial="initial"
        :empties="empties"
        :btnInfos="btnInfos"
        @isPress="isPress"
      ></Board>
      <Button @click="activeBtn"></Button>
    </div>
  </div>
</template>

<script setup lang="ts">
// 難易度設定
let currentLevel = ref<number>(0);
const levelInfos = ref<any[]>([
  { text: "EASY", selectedLevel: true, level: 0, side: 4 },
  { text: "NOMAL", selectedLevel: false, level: 1, side: 5 },
  { text: "HARD", selectedLevel: false, level: 2, side: 6 },
]);
const changeLevel = (index: number) => {
  for (let i = 0; i < levelInfos.value.length; i++) {
    levelInfos.value[i].selectedLevel = false;
  }
  levelInfos.value[index].selectedLevel = true;
  currentLevel.value = levelInfos.value[index].level;
};
// console.log(currentLevel.value);

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

// タイマー処理
let startTime = ref<number>(0);
let timer = ref<number>(0.0);
let timeoutId: NodeJS.Timeout;
function runTimer() {
  timer.value = +((Date.now() - startTime.value) / 1000).toFixed(2);

  timeoutId = setTimeout(() => {
    runTimer();
  }, 10);
}

// スタートボタンを押した処理
let currentNumber = ref<number>(0);
const activeBtn = () => {
  if (typeof timeoutId !== "undefined") {
    clearTimeout(timeoutId);
  }
  currentNumber.value = 0;
  initial.value = false;
  randomNumsInfo();
  startTime.value = Date.now();
  runTimer();
};

// パネルを押した時の処理
const isPress = (index: number) => {
  if (currentNumber.value !== btnInfos.value[index].num) {
    return;
  }
  btnInfos.value[index].isPress = true;
  currentNumber.value++;

  if (currentNumber.value === nums.value.length) {
    clearTimeout(timeoutId);
  }
};
</script>
