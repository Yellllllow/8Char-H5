<template>
  <!-- <yx-nav-header></yx-nav-header> -->
  <!-- <head-box></head-box> -->
  <sheet></sheet>
  <!-- <bottom></bottom> -->
</template>

<script setup>
import HeadBox from './components/index/head/head.vue';
import Sheet from './components/index/sheet/sheet.vue';
// import Bottom from './components/index/bottom/bottom.vue';
import { onMounted, nextTick } from 'vue';

// 发送高度到父页面的函数
const sendHeightToParent = () => {
  const height = document.querySelector("uni-page-body").clientHeight;//document.documentElement.scrollHeight;
  if(window.parent)
    window.parent.postMessage({ type: 'setHeight', height }, '*');
};

// 监听DOM变化以更新高度
const observeHeight = () => {
  const observer = new MutationObserver(() => {
    nextTick(() => {
      sendHeightToParent();
    });
  });
  
  observer.observe(document.body, {
    attributes: true,
    childList: true,
    subtree: true
  });
};

onMounted(() => {
  // #ifdef H5
  // 初始发送高度
  nextTick(() => {
    sendHeightToParent();
  });
  
  // 监听DOM变化
  observeHeight();
  
  // 监听窗口大小变化
  window.addEventListener('resize', sendHeightToParent);
  // #endif
});
</script>
