<template>
  <div class="home">
    <canvas id="canvas" ref="canvas"></canvas>
    <div>
      <div class="button" @click="bubStart">冒泡排序</div>
      <div class="button" @click="insertStart">插入排序</div>
      <div class="button" @click="() => (flag = !flag)">
        {{ flag ? "暂停" : "开始" }}
      </div>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { onMounted, ref } from "vue";
const length = 720;
const WIDTH = 1000;
const HEIGHT = 1000;
const WIDTHCENTER = WIDTH / 2;
const HEIGHTCENTER = HEIGHT / 2;
let randomList = Array.from({ length }).map(() => Math.random() * 500);

const count = ref(0);
const flag = ref(true);
const canvas = ref<HTMLCanvasElement>();
const context = ref<CanvasRenderingContext2D | null>();

const draw = (changeList: number[]) => {
  const ctx = context.value;
  if (!ctx) return;
  ctx.clearRect(0, 0, WIDTH, HEIGHT);

  randomList.forEach((node, i) => {
    const x = node * Math.cos((Math.PI / (length / 2)) * i);
    const y = node * Math.sin((Math.PI / (length / 2)) * i);
    ctx.beginPath();
    ctx.moveTo(WIDTHCENTER + x, HEIGHTCENTER + y);
    if (changeList.includes(i)) {
      ctx.fillStyle = "#f00";
      ctx.strokeStyle = "#f00";
    } else {
      ctx.fillStyle = "#000";
      ctx.strokeStyle = "#000";
    }
    ctx.arc(WIDTHCENTER + x, HEIGHTCENTER + y, 4, 0, Math.PI * 2);
    ctx.fill();
    ctx.moveTo(WIDTHCENTER, HEIGHTCENTER);
    ctx.stroke();
  });

  return new Promise<void>((res) => {
    if (flag.value) {
      setTimeout(() => {
        res();
      }, 0);
    } else {
      setInterval(() => {
        if (flag.value) {
          res();
        }
      });
    }
  });
};

// 冒泡排序
async function* sort() {
  for (let i = 0; i < randomList.length; i++) {
    let changeList = [];
    for (let j = 0; j < randomList.length; j++) {
      if (randomList[j] > randomList[j + 1]) {
        let temp = randomList[j];
        randomList[j] = randomList[j + 1];
        randomList[j + 1] = temp;
        changeList.push(j);
      }
    }
    await draw(changeList);
    yield true;
  }
  await draw([]);
  yield false;
}

// 插入排序
async function* sort2() {
  for (let i = 1; i < randomList.length; i++) {
    let changeList = [];
    let perIndex = i;
    let count = randomList[i];
    while (perIndex >= 1 && randomList[perIndex - 1] > count) {
      randomList[perIndex] = randomList[perIndex - 1];
      perIndex -= 1;
      changeList.push(perIndex);
    }
    randomList[perIndex] = count;
    await draw(changeList);
    yield true;
  }
  await draw([]);
  yield false;
}

const bubStart = async () => {
  randomList = Array.from({ length }).map(() => Math.random() * 500);
  let next = sort();
  while (!(await next.next()).done);
};

const insertStart = async () => {
  randomList = Array.from({ length }).map(() => Math.random() * 500);
  let next = sort2();
  while (!(await next.next()).done);
};

onMounted(async () => {
  console.log(canvas.value);
  context.value = canvas.value?.getContext("2d");
  if (!canvas.value) return;
  canvas.value.width = WIDTH;
  canvas.value.height = HEIGHT;
});
</script>
<style lang="scss" scoped>
#canvas {
  width: 500px;
  height: 500px;
  // background-color: black;
}
.button {
  padding: 15px;
  display: inline-block;
  border-radius: 5px;
  user-select: none;
  cursor: pointer;
  border: 1px solid #cecece;
}
</style>
