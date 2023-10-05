<template>
  <div>
    <SelectLevels
      :levelInfos="levelInfos"
      @changeLevel="changeLevel"
    ></SelectLevels>
    <div
      class="my-4 mx-auto"
      :class="{
        easySize: levelInfos[0].selectedLevel,
        nomalSize: levelInfos[1].selectedLevel,
        hardSize: levelInfos[2].selectedLevel,
      }"
    >
      <Timer :timer="timer"></Timer>
      <Board
        :initial="initial"
        :empties="empties"
        :btnInfos="btnInfos"
        @isPress="isPress"
      ></Board>
      <Button @activeBtn="activeBtn"></Button>
    </div>
  </div>
</template>

<style>
.easySize {
  width: 220px;
}

.nomalSize {
  width: 270px;
}

.hardSize {
  width: 320px;
}
</style>

<script setup lang="ts">
// 難易度設定
const levelInfos = ref<any[]>([
  { id: 0, text: "EASY", selectedLevel: true, side: 4 },
  { id: 1, text: "NOMAL", selectedLevel: false, side: 5 },
  { id: 2, text: "HARD", selectedLevel: false, side: 6 },
]);
let currentLevel = ref<number>(0);
const changeLevel = (index: number) => {
  clearTimeout(timeoutId);
  timer.value = 0;
  initial.value = true;
  for (let i = 0; i < levelInfos.value.length; i++) {
    levelInfos.value[i].selectedLevel = false;
  }
  levelInfos.value[index].selectedLevel = true;
  currentLevel.value = index;
  nums.value = [];
  createNumber();
  empties.value = [];
  createEmpty();
  // console.log(nums.value);
};

// 初期パネルの生成
let nums = ref<number[]>([]);

function createNumber() {
  for (let i = 0; i < levelInfos.value[currentLevel.value].side ** 2; i++) {
    nums.value.push(i);
  }
}
createNumber();

let btnInfos = ref<any[]>([]);
onMounted(() => {
  randomNumsInfo();
});

// ボタンの初期化
const initial = ref<boolean>(true);

let randomNums = ref<number[]>([]);

function randomNumsInfo() {
  randomNums.value = nums.value.sort(() => Math.random() - 0.5);
  btnInfos.value = [];
  for (let i = 0; i < nums.value.length; i++) {
    btnInfos.value.push({
      num: randomNums.value[i],
      isPress: false,
    });
  }
}
// 初期値用の空のパネルの生成
const empties = ref<any[]>([]);
function createEmpty() {
  for (let i = 0; i < nums.value.length; i++) {
    empties.value.push("");
  }
}
createEmpty();

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
