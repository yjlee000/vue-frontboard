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
   <template>
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
    </style>