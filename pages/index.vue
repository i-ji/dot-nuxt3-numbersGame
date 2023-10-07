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
    <div class="flex justify-between w-[760px] mx-auto">
      <Log :scores="easySliceScores" level="EASY"></Log>
      <Log :scores="nomalSliceScores" level="NOMAL"></Log>
      <Log :scores="hardSliceScores" level="HARD"></Log>
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
  { text: "EASY", selectedLevel: true, side: 4 },
  { text: "NOMAL", selectedLevel: false, side: 5 },
  { text: "HARD", selectedLevel: false, side: 6 },
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

// 記録
const easyScores = ref<number[]>([]);
const nomalScores = ref<number[]>([]);
const hardScores = ref<number[]>([]);

const easySliceScores = useCookie("sliceScores", {
  default: () => [] as number[],
});
const nomalSliceScores = useCookie("sliceScores", {
  default: () => [] as number[],
});
const hardSliceScores = useCookie("sliceScores", {
  default: () => [] as number[],
});

// パネルを押した時の処理
const isPress = (index: number) => {
  if (currentNumber.value !== btnInfos.value[index].num) {
    return;
  }
  btnInfos.value[index].isPress = true;
  currentNumber.value++;

  if (currentNumber.value === nums.value.length) {
    clearTimeout(timeoutId);
    if (nums.value.length === 16) {
      easyAddScore();
    } else if (nums.value.length === 25) {
      nomalAddScore();
    } else if (nums.value.length === 36) {
      hardAddScore();
    }
  }
};

// scoreInfosに追加する関数
function compareFn(a: number, b: number) {
  return a - b;
}

function easyAddScore() {
  // if (nums.value.length === 16) {
  console.log(nums.value + " easy");
  // }

  const easyTime = timer.value;
  easyScores.value.push(easyTime);
  easyScores.value.sort(compareFn);
  easySliceScores.value = easyScores.value.slice(0, 4);
}

function nomalAddScore() {
  // if (nums.value.length === 25) {
  console.log(nums.value + " nomal");
  // }

  const nomalTime = timer.value;
  nomalScores.value.push(nomalTime);
  nomalScores.value.sort(compareFn);
  nomalSliceScores.value = nomalScores.value.slice(0, 4);
}

function hardAddScore() {
  // if (nums.value.length === 36) {
  console.log(nums.value + " hard");
  // }

  const hardTime = timer.value;
  hardScores.value.push(hardTime);
  hardScores.value.sort(compareFn);
  hardSliceScores.value = hardScores.value.slice(0, 4);
}
</script>
