<template>
  <canvas id="canvas" ref="canvas"></canvas>
</template>
<script setup lang="ts">
// import { renderList, watch, watchEffect, ref } from "vue";

// const count = ref(0);
// const canvas = ref<HTMLCanvasElement>();
// const context = ref<CanvasRenderingContext2D | null>();

// defineProps({
//   renderList: number[]
// });

// const draw = () => {
//   // console.time("renderTime");
//   const ctx = context.value;
//   if (!ctx) return;
//   ctx.clearRect(0, 0, 1000, 1000);
//   ctx.beginPath();
//   renderList.forEach((node, i) => {
//     const x = node * Math.cos((Math.PI / (length / 2)) * i);
//     const y = node * Math.sin((Math.PI / (length / 2)) * i);
//     ctx.moveTo(500 + x, 500 + y);
//     ctx.fillStyle = "#000";
//     ctx.arc(500 + x, 500 + y, 2, 0, Math.PI * 2);
//     ctx.moveTo(500, 500);
//     ctx.stroke();
//   });
//   // console.log("draw");
//   // console.timeEnd("renderTime");
//   return new Promise<void>((res) => {
//     setTimeout(() => {
//       res();
//     });
//   });
// };
</script>
<style lang="scss">
#canvas {
  width: 500px;
  height: 500px;
  // background-color: black;
}
</style>
