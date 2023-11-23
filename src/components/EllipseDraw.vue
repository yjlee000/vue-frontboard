<template>
  <svg 
    style="position: absolute; top: 0; left: 0; z-index: 3; overflow: visible;" 
    @mousedown.prevent="startMovingEllipse" 
    @mousemove.prevent="moveEllipse" 
    @mouseup.prevent="endMovingEllipse" 
    @mouseleave.prevent="endMovingEllipse"
  >
  <ellipse 
    v-if="imageLoaded"
    :cx="ellipse.cx" 
    :cy="ellipse.cy" 
    :rx="ellipse.rx" 
    :ry="ellipse.ry" 
    style="fill: none; stroke: lemonchiffon; stroke-width: 2; stroke-dasharray: 5,5;"
    :transform="`rotate(${ellipse.rotation} ${ellipse.cx} ${ellipse.cy})`"
  />
    <circle v-if="imageLoaded" :cx="ellipse.cx" :cy="ellipse.cy - ellipse.ry" r="5" style="fill: yellow; cursor: ns-resize;" @mousedown.stop.prevent="startResizingEllipse" @mousemove.stop.prevent="resizeEllipse" @mouseup.stop.prevent="endResizingEllipse" @mouseleave.stop.prevent="endResizingEllipse" :transform="`rotate(${ellipse.rotation} ${ellipse.cx} ${ellipse.cy})`"/>
    <circle v-if="imageLoaded" :cx="ellipse.cx" :cy="ellipse.cy + ellipse.ry" r="5" style="fill: yellow; cursor: ns-resize;" @mousedown.stop.prevent="startResizingEllipse" @mousemove.stop.prevent="resizeEllipse" @mouseup.stop.prevent="endResizingEllipse" @mouseleave.stop.prevent="endResizingEllipse" :transform="`rotate(${ellipse.rotation} ${ellipse.cx} ${ellipse.cy})`"/>
    <circle v-if="imageLoaded" :cx="ellipse.cx - ellipse.rx" :cy="ellipse.cy" r="5" style="fill: yellow; cursor: ew-resize;" @mousedown.stop.prevent="startResizingEllipse" @mousemove.stop.prevent="resizeEllipse" @mouseup.stop.prevent="endResizingEllipse" @mouseleave.stop.prevent="endResizingEllipse" :transform="`rotate(${ellipse.rotation} ${ellipse.cx} ${ellipse.cy})`"/>
    <circle v-if="imageLoaded" :cx="ellipse.cx + ellipse.rx" :cy="ellipse.cy" r="5" style="fill: yellow; cursor: ew-resize;" @mousedown.stop.prevent="startResizingEllipse" @mousemove.stop.prevent="resizeEllipse" @mouseup.stop.prevent="endResizingEllipse" @mouseleave.stop.prevent="endResizingEllipse" :transform="`rotate(${ellipse.rotation} ${ellipse.cx} ${ellipse.cy})`"/>
    <circle v-if="imageLoaded" :cx="ellipse.cx" :cy="ellipse.cy + ellipse.ry - 80" r="5" style="fill: violet; cursor: pointer;" @mousedown.stop.prevent="startRotatingEllipse" @mousemove.stop.prevent="rotateEllipse" @mouseup.stop.prevent="endRotatingEllipse" @mouseleave.stop.prevent="endRotatingEllipse" :transform="`rotate(${ellipse.rotation} ${ellipse.cx} ${ellipse.cy})`"/>
    </svg>
  </template>
  
<script>
import { reactive, ref } from 'vue';

export default {
  props: ['imageLoaded'],
  setup(props) {
    const ellipse = reactive({ cx: 100, cy: 100, rx: 50, ry: 30, rotation: 0 });
    const movingEllipse = ref(false);
    const resizingEllipse = ref(false);
    const rotatingEllipse = ref(false);

    const startMovingEllipse = (e) => {
      movingEllipse.value = true;
    };

    const moveEllipse = (e) => {
      if (!movingEllipse.value) return;
      e.stopPropagation();
      ellipse.cx += e.movementX;
      ellipse.cy += e.movementY;
      event.stopPropagation();  // 이벤트의 전파를 중단
    };

    const endMovingEllipse = () => {
      movingEllipse.value = false;
      // this.$emit('drag-end');  // 타원 드래그가 끝났음을 부모 컴포넌트에게 알림
    };
  
      const startResizingEllipse = (e) => {
      resizingEllipse.value = true;
    };
  
    const resizeEllipse = (e) => {
      if (!resizingEllipse.value) return;
      let dy = e.clientY - ellipse.cy;
      let dx = e.clientX - ellipse.cx;
      let theta = Math.atan2(dy, dx);
      if (Math.abs(theta) > Math.PI / 4 && Math.abs(theta) < 3 * Math.PI / 4) {
        ellipse.ry += e.movementY;
      } else {
        ellipse.rx += e.movementX;
      }
    };
  
      const endResizingEllipse = () => {
        resizingEllipse.value = false;
      };

      const startRotatingEllipse = (e) => {
        rotatingEllipse.value = true;
    };

    const rotateEllipse = (e) => {
      if (!rotatingEllipse.value) return;
      let angle = Math.atan2(e.clientY - ellipse.cy, e.clientX - ellipse.cx) * (180 / Math.PI);
      ellipse.rotation = 3 * (angle - 90);
    };

    const endRotatingEllipse = () => {
      rotatingEllipse.value = false;
    };

    console.log(props.imageLoaded);  // imageLoaded prop 값이 실제로 true로 설정되는지 확인
  
    return { ellipse, startMovingEllipse, moveEllipse, endMovingEllipse, startResizingEllipse, resizeEllipse, endResizingEllipse, startRotatingEllipse, rotateEllipse, endRotatingEllipse };
  },
};
  </script>
  