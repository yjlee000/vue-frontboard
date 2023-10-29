<template>
   <body>
    <svg v-if="imageSrc" style="position: absolute; top: 0; left: 0; z-index: 3;">
      <ellipse 
        :cx="ellipse.cx" 
        :cy="ellipse.cy" 
        :rx="ellipse.rx" 
        :ry="ellipse.ry" 
        style="fill: none; stroke: red; stroke-width: 2; stroke-dasharray: 5,5;" 
        @mousedown="startInteraction"
        @mousemove="interact"
        @mouseup="endInteraction"
        @mouseleave="endInteraction"
      />
      <circle 
            v-for="(handle, index) in handles" 
            :key="index" 
            :cx="handle.x" 
            :cy="handle.y" 
            r="5" 
            style="fill: blue; cursor: pointer;"
            @mousedown="startHandleInteraction(index)"
          />
    </svg>
   </body>
  </template>
  
  <script>
  import { ref, reactive } from 'vue';
  
  export default {
    setup() {
      const ellipse = reactive({ cx: 100, cy: 100, rx: 50, ry: 30 });
      // 타원 확대, 축소, 패닝 로직
      // ...
      const handles = computed(() => [
          { x: ellipse.cx - ellipse.rx, y: ellipse.cy }, 
          { x: ellipse.cx + ellipse.rx, y: ellipse.cy }, 
          { x: ellipse.cx, y: ellipse.cy - ellipse.ry }, 
          { x: ellipse.cx, y: ellipse.cy + ellipse.ry }
        ]);
        let currentHandle = null;
        const startInteraction = (e) => {
          interacting.value = true;
        };
        const startHandleInteraction = (index) => {
          e.stopPropagation();
          interacting.value = true;
          currentHandle = index;
        };
    
        const startEllipseInteraction = (e) => {
          e.stopPropagation();
          interacting.value = true;
          currentHandle = null;
        };
    
        const interact = (e) => {
          if (!interacting.value) return;
          if (currentHandle !== null) {
            switch (currentHandle) {
              case 0: ellipse.rx -= e.movementX; break;
              case 1: ellipse.rx += e.movementX; break;
              case 2: ellipse.ry -= e.movementY; break;
              case 3: ellipse.ry += e.movementY; break;
            }
            ellipse.rx = Math.max(0, ellipse.rx);
            ellipse.ry = Math.max(0, ellipse.ry);
          } else {
            let newCx = ellipse.cx + e.movementX;
            let newCy = ellipse.cy + e.movementY;
            // 이미지의 경계를 벗어나지 않도록 조정
            newCx = Math.max(0, Math.min(newCx, imageWidth - ellipse.rx));
            newCy = Math.max(0, Math.min(newCy, imageHeight - ellipse.ry));
            ellipse.cx = newCx;
            ellipse.cy = newCy;
          }
        };
    
        const endInteraction = () => {
          interacting.value = false;
          currentHandle = null;
        };
      return { ellipse, startInteraction, interact, endInteraction };
    },
  };
  </script>