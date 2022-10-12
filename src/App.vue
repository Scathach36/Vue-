<script setup lang="ts">
  import ImgCheck from './components/ImgCheck.vue'

  let top = 50; // 该值为服务端传回的数据

  // 获取组件传回的坐标 x
  const getPosition = (x) => {
    console.log(x);
    
  }
</script>

<template>
  <div>
    <ImgCheck @getPosition="getPosition" :top="top" />
  </div>
</template>

<style>
html, body {
  width: 100%;
  height: 100%;
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
</style>
