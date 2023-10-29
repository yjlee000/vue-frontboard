<template>
    <body>
        <div>
      <img 
        :src="imageSrc" 
        :style="{ transform: `translate(${x}px, ${y}px) scale(${scale})` }" 
        @mousedown="startInteraction"
        @mousemove="interact"
        @mouseup="endInteraction"
        @mouseleave="endInteraction"
      />
      <slot></slot>
    </div>
    </body>
  </template>
 
  
  <script>
  import { ref } from 'vue';
  
  export default {
    props: ['imageSrc'],
    setup() {
      // 이미지 확대, 축소, 패닝 로직
      // ...
        const startInteraction = (e) => {
            interacting.value = true;
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
    


      return { startInteraction, interact, endInteraction };
    },
  };
  </script>