<template>
    <div 
      class="image-container" 
      @mousedown="startMoving" 
      @mousemove="move" 
      @mouseup="endMoving"
      @mouseleave="endMoving"
    >
      <img 
        :src="src" 
        :style="{ transform: `translate(${x}px, ${y}px) scale(${scale})`, }" 
        @contextmenu.prevent
        @dragstart.prevent
        style="position: absolute; top: 0; left: 0; z-index: 1;"
      />
      <slot></slot>
    </div>
  </template>
  
  <script>
  import { ref } from 'vue';
  
  export default {
    props: ['src', 'scale'],
    setup() {
      const x = ref(0);
      const y = ref(0);
      const moving = ref(false);
  
      const startMoving = () => {
        moving.value = true;
      };
  
      const move = (e) => {
        if (!moving.value) return;
        x.value += e.movementX;
        y.value += e.movementY;
      };
  
      const endMoving = () => {
        moving.value = false;
        // this.$emit('pan-end');  // 이미지 패닝이 끝났음을 부모 컴포넌트에게 알림
      };
  
      return { x, y, startMoving, move, endMoving };
    },
  };
  </script>
  
  <style scoped>
  .image-container {
    width: 800px;
    height: 600px;
    position: relative;
    overflow: hidden;
  }
  </style>
  