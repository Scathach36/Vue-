<template>
  <div class="box">
    <h3>拖动下方滑块完成验证</h3>
    <div class="img-box" ref="bgImgRef">
      <img class="img-bg" src="../assets/test-img.jpg" alt="" />
      <img
        class="img-piece"
        src="../assets/img-piece.png"
        alt=""
        :style="{ left: data.left + 'px', top: data.top + 'px' }"
      />
    </div>
    <div class="dragger-box">
      <div
        ref="btnRef"
        class="dragger"
        @mousedown="btnMouseDown"
        :style="{left: data.left + 'px'}"
      >
        |||
      </div>
    </div>
  </div>
</template>

<script setup>
import { reactive, toRefs } from "@vue/reactivity";
import { onMounted } from "@vue/runtime-core";

let $props = defineProps({
  top: {
    type: Number,
    default: 50,
  },
});

let $emits = defineEmits(['getPosition'])

const data = reactive({
  left: 26,
  top: $props.top,
  btnRef: null,
  btnLeft: 0,
  offsetLeft: 0,
  bgImgRef: null
});

const { btnRef, bgImgRef } = toRefs(data); // 拿到

onMounted(() => {
  console.log("OnMounted");
  data.offsetLeft = data.bgImgRef.offsetLeft
  document.body.addEventListener("mouseup", btnMouseUp);
});

// 滑动按钮按下后的事件
const btnMouseDown = (e) => {
  // 为了用户体验的流畅，给 body 添加了监听事件，这样用户只要不松开鼠标，在整个页面都可以滑动按钮
  document.body.addEventListener("mousemove", btnMouseMove);

  data.btnLeft = e.pageX - data.offsetLeft;
};

// 按钮滑动事件: 只要鼠标不松开就能触发
const btnMouseMove = (e) => {
  console.log("滑动");
  let x = e.pageX - data.offsetLeft - data.btnLeft;
  // 设置滑动边界
  if (x <= 0) x = 0;
  else if (x >= 290) x = 290;
  data.left = x;
};

// 鼠标松开，开始校验
const btnMouseUp = (e) => {
  document.body.removeEventListener("mousemove", btnMouseMove);
  // console.log(data.left);
  $emits("getPosition", data.left) // 传值回父组件
  data.left = 26
  
};
</script>

<style scoped>
html,
body {
  width: 100%;
  height: 100%;
  user-select: none;
}
.box {
  border: 2px solid #ccc;
  width: 340px;
  margin: 0 auto;
  padding: 10px;
}
.img-box {
  position: relative;
}
.img-piece {
  position: absolute;
  /* left: 26px; */
  /* top: 50px; */
}
.img-bg {
  width: 100%;
  border: 1px solid #ccc;
}
.dragger-box {
  width: 100%;
  height: 10px;
  border-radius: 5px;
  background-color: #e4e4e4;
  margin-top: 15px;
}
.dragger {
  position: relative;
  top: -10px;
  left: 26px;
  width: 55px;
  height: 30px;
  text-align: center;
  letter-spacing: 5px;
  line-height: 30px;
  color: #fff;
  border-radius: 15px;
  background-color: rgb(26, 121, 255);
  box-shadow: rgb(26 122 255/ 52%) 0px 0px 10px 1px;
  cursor: pointer;
  user-select: none;
}
</style>