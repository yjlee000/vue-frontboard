<!-- <template>
    <div>
      <div class="file-section">
        <input type="file" @change="loadImage" accept="image/*">
      </div>
      <div class="image-section" v-if="imageSrc">
        
        <button @click="increaseSize">확대</button>
        <button @click="decreaseSize">축소</button>
        <div><img :src="imageSrc" :style="{ width: imageWidth + 'px' }"/></div>
      </div>
    </div>
  </template>
  
  <script>
  import { ref } from 'vue';
  
  export default {
    setup() {
      const imageSrc = ref(null);
      const imageWidth = ref(300); // 이미지의 초기 너비 설정
  
      const loadImage = (event) => {
        const file = event.target.files[0];
        if (file) {
          imageSrc.value = URL.createObjectURL(file);
        }
      };
  
      const increaseSize = () => {
        imageWidth.value += 50; // 너비를 50px 증가
      };
  
      const decreaseSize = () => {
        if(imageWidth.value > 50) { // 최소 크기 제한
          imageWidth.value -= 50; // 너비를 50px 감소
        }
        
      };
  
       return { imageSrc, loadImage, increaseSize, decreaseSize, imageWidth };
     }
  };
  </script>
  
  <style scoped>
  .file-section,
  .image-section {
   margin-bottom: 20px;
  }
  
  img {
   height: auto;
  }
  </style> 
   -->
   <!-- <template>
    <div class="container">
      <div 
        class="dropzone" 
        @dragover.prevent 
        @drop="onDrop"
      >
        <p>여기에 이미지를 드래그 앤 드롭하세요.</p>
        <div v-if="imageSrc" class="image-container">
          <img :src="imageSrc" />
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import { ref } from 'vue';
  
  export default {
    setup() {
      const imageSrc = ref(null);
  
      const onDrop = (e) => {
        const files = e.dataTransfer.files;
        if (!files.length) return;
  
        const file = files[0];
        const reader = new FileReader();
  
        reader.onload = (e) => {
          imageSrc.value = e.target.result;
        };
        reader.readAsDataURL(file);
      };
  
      return { imageSrc, onDrop };
    },
  };
  </script>
  
  <style>
  .container {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 100vh;
  }
  
  .dropzone {
    width: 700px;
    height: 700px;
    border: 2px dashed #ccc;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    text-align: center;
  }
  
  .image-container {
    width: 400px;
    height: 500px;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  
  .image-container img {
    max-width: 100%;
    max-height: 100%;
  }
  </style>
   -->




   <!-- <template>
    <div class="container">
      <div 
        ref="dropzone"
        class="dropzone" 
        @dragover.prevent 
        @drop="onDrop"
      >
        <p>여기에 이미지를 드래그 앤 드롭하세요.</p>
        <div v-if="imageSrc" class="image-container" style="position: relative;">
          <img 
            :src="imageSrc" 
            :style="{ transform: `translate(${x}px, ${y}px) scale(${scale})` }" 
            @mousedown="startMoving" 
            @mousemove="move" 
            @mouseup="endMoving"
            @mouseleave="endMoving"
            style="position: absolute; top: 0; left: 0; z-index: 1;"
          />
          <svg 
            style="position: absolute; top: 0; left: 0; z-index: 3;" 
            @mousedown="startMovingEllipse" 
            @mousemove="moveEllipse" 
            @mouseup="endMovingEllipse" 
            @mouseleave="endMovingEllipse"
          >
            <ellipse 
              :cx="ellipse.cx" 
              :cy="ellipse.cy" 
              :rx="ellipse.rx" 
              :ry="ellipse.ry" 
              style="fill: none; stroke: red; stroke-width: 2; stroke-dasharray: 5,5;"
            />
            <rect 
              :x="ellipse.cx + ellipse.rx" 
              :y="ellipse.cy" 
              width="10" 
              height="10" 
              style="fill: blue; cursor: ew-resize;" 
              @mousedown="startResizingEllipse" 
              @mousemove="resizeEllipse" 
              @mouseup="endResizingEllipse" 
              @mouseleave="endResizingEllipse"
            />
          </svg>
        </div>
        <div v-if="imageSrc">
          <button @click="scaleUp">Zoom In</button> &nbsp;&nbsp;&nbsp;
          <button @click="scaleDown">Zoom Out</button>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import { ref, reactive } from 'vue';
  
  export default {
    setup() {
        const imageSrc = ref(null);
        const scale = ref(1);
        const x = ref(0);
        const y = ref(0);
        const moving = ref(false);
        const dropzone = ref(null);
        const ellipse = reactive({ cx: 100, cy: 100, rx: 50, ry: 30, rotation: 0 });
        const movingEllipse = ref(false);
        const resizingEllipse = ref(false);
        const rotatingEllipse = ref(false);
  
      const onDrop = (e) => {
        const files = e.dataTransfer.files;
        if (!files.length) return;
  
        const file = files[0];
        const reader = new FileReader();
  
        reader.onload = (e) => {
          imageSrc.value = e.target.result;
        };
        reader.readAsDataURL(file);
      };
  
      const scaleUp = () => {
        scale.value += 0.1;
      };
  
      const scaleDown = () => {
        scale.value -= 0.1;
      };
  
      const startMoving = (e) => {
        moving.value = true;
      };
  
      const move = (e) => {
        if (!moving.value) return;
        x.value += e.movementX;
        y.value += e.movementY;
      };
  
      const endMoving = () => {
        moving.value = false;
      };
      const startMovingEllipse = (e) => {
      movingEllipse.value = true;
    };

    const moveEllipse = (e) => {
      if (!movingEllipse.value) return;
      ellipse.cx += e.movementX;
      ellipse.cy += e.movementY;
    };

    const endMovingEllipse = () => {
      movingEllipse.value = false;
    };
    const startResizingEllipse = (e) => {
      e.stopPropagation();
      resizingEllipse.value = true;
    };

    const resizeEllipse = (e) => {
      if (!resizingEllipse.value) return;
      ellipse.rx += e.movementX;
      ellipse.ry += e.movementY;
    };

    const endResizingEllipse = () => {
      resizingEllipse.value = false;
    };

    return { imageSrc, onDrop, scaleUp, scaleDown, startMoving, move, endMoving, x, y, scale, dropzone, ellipse, startMovingEllipse, moveEllipse, endMovingEllipse, startResizingEllipse, resizeEllipse, endResizingEllipse };
  },

  };
  </script>
  
  
  <style>
    .container {
      display: flex;
      align-items: center;
      justify-content: center;
      height: 100vh;
    }
    
    .dropzone {
      width: 700px;
      height: 700px;
      border: 2px dashed #ccc;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      text-align: center;
    }
    
    .image-container {
      width: 400px;
      height: 500px;
      display: flex;
      align-items: center;
      justify-content: center;
    }
    
    .image-container img {
      max-width: 100%;
      max-height: 100%;
    }
    </style> -->

    <template>
      <div 
        class="dropzone" 
        @dragover.prevent 
        @drop="onDrop"
        @mousedown="startInteraction"
        @mousemove="interact"
        @mouseup="endInteraction"
        @mouseleave="endInteraction"
      >
        <p v-if="!imageSrc">여기에 이미지를 드래그 앤 드롭하세요.</p>
        <img 
          v-if="imageSrc" 
          :src="imageSrc" 
          :style="{ transform: `translate(${x}px, ${y}px) scale(${scale})`, }"
          @contextmenu.prevent
        />
        <svg v-if="imageSrc" style="position: absolute; top: 0; left: 0; z-index: 3;">
          <ellipse 
            :cx="ellipse.cx" 
            :cy="ellipse.cy" 
            :rx="ellipse.rx" 
            :ry="ellipse.ry" 
            style="fill: none; stroke: red; stroke-width: 2; stroke-dasharray: 5,5;" 
            @mousedown="startEllipseInteraction"
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
        <div v-if="imageSrc" class="controls">
          <button @click="zoomIn">확대</button>
          <button @click="zoomOut">축소</button>
        </div>
      </div>
    </template>
    
    <script>
    import { ref, reactive, computed } from 'vue';
    
    export default {
      setup() {
        const imageSrc = ref(null);
        const x = ref(0);
        const y = ref(0);
        const scale = ref(1);
        const interacting = ref(false);
        const ellipse = reactive({ cx: 100, cy: 100, rx: 50, ry: 30 });
        const handles = computed(() => [
          { x: ellipse.cx - ellipse.rx, y: ellipse.cy }, 
          { x: ellipse.cx + ellipse.rx, y: ellipse.cy }, 
          { x: ellipse.cx, y: ellipse.cy - ellipse.ry }, 
          { x: ellipse.cx, y: ellipse.cy + ellipse.ry }
        ]);
        let currentHandle = null;

        const onDrop = (e) => {
      e.preventDefault();
      const file = e.dataTransfer.files[0];
      
      // 파일이 이미지인지 확인
      if (!file.type.startsWith('image/')) {
        console.log('이미지 파일만 업로드 가능합니다.');
        return;
      }

      // 이미지 사이즈 확인
      const image = new Image();
      image.onload = function() {
        const width = this.width;
        const height = this.height;

        // 원하는 사이즈 범위를 지정합니다.
        const minWidth = 200;
        const minHeight = 200;
        const maxWidth = 800;
        const maxHeight = 800;

        if (width >= minWidth && width <= maxWidth && height >= minHeight && height <= maxHeight) {
          // 이미지를 업로드하는 로직을 수행합니다.
          imageSrc.value = URL.createObjectURL(file);
        } else {
          // 원하는 사이즈 범위를 벗어나는 경우 에러 처리를 합니다.
          console.log('이미지 사이즈가 유효하지 않습니다.');
        }
      };
      image.src = URL.createObjectURL(file);
    };
    
        // 가상의 이미지 크기
        const imageWidth = 800;  
        const imageHeight = 600; 
    
        // const onDrop = (e) => {
        //   e.preventDefault();
        //   const file = e.dataTransfer.files[0];
        //   if (file && file.type.startsWith('image/')) {
        //     const reader = new FileReader();
        //     reader.onload = (e) => {
        //       imageSrc.value = e.target.result;
        //     };
        //     reader.readAsDataURL(file);
        //   }
        // };
    
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
    
        const zoomIn = () => {
          scale.value *= 1.1;
        };
    
        const zoomOut = () => {
          scale.value /= 1.1;
        };
    
        return { imageSrc, x, y, scale, ellipse, handles, onDrop, startInteraction, startHandleInteraction, startEllipseInteraction, interact, endInteraction, zoomIn, zoomOut };
      },
    };
    </script>
    
    <style scoped>
    .dropzone {
      width: 800px;
      height: 600px;
      border: 1px solid black;
      position: relative;
      overflow: hidden;
    }
    
    .controls {
      position: absolute;
      bottom: 10px;
      left: 10px;
      z-index: 10;
    }
    
    img {
        pointer-events: none;
        user-select: none;
    }
    </style>
    