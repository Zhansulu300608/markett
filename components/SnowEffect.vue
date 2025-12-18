<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'

const snowCount = 100
const snows = Array.from({ length: snowCount }, () => ({
  x: Math.random() * 100,
  size: 2 + Math.random() * 6,
  duration: 5 + Math.random() * 10,
  delay: Math.random() * 10,
}))
</script>

<template>
  <div class="snow__inner">
    <div
      v-for="(snow, index) in snows"
      :key="index"
      class="snow"
      :style="{
        left: snow.x + '%',
        width: snow.size + 'px',
        height: snow.size + 'px',
        animationDuration: snow.duration + 's',
        animationDelay: snow.delay + 's'
      }"
    ></div>
  </div>
</template>

<style scoped>
.snow__inner {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  z-index: 50; 
  overflow: hidden;
}

.snow {
  position: absolute;
  top: -10px;
  background: white;
  border-radius: 50%;
  opacity: 0.9;
  animation-name: fall;
  animation-timing-function: linear;
  animation-iteration-count: infinite;
}

@keyframes fall {
  to {
    transform: translateY(110vh);
  }
}
</style>
