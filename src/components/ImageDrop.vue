<template>
  <div class="container">
    <div v-if="imageLoaded" class="zoomButtons">
      <button id="zout" @click="$emit('scaleDown')">Zoom Out</button> &nbsp;&nbsp;&nbsp;&nbsp;
      <button id="zin" @click="$emit('scaleUp')">Zoom In</button> 
    </div>
    <div class="dropzone" @dragover.prevent @drop="onDrop">
      <p v-if="!imageLoaded">여기에 이미지를 드래그 앤 드롭하세요.</p>
      <slot></slot>
    </div>
  </div>
</template>
  
<script>
  import { ref } from 'vue';
  import EllipseDraw from './EllipseDraw.vue';
  export default {
    components: {
        EllipseDraw
    }, 
    data() {
      return {
        imageLoaded: false,
      };
    },
    setup() {
      const scale = ref(1);
      const scaleUp = () => {
        scale.value += 0.1;
      };

      const scaleDown = () => {
        if (scale.value > 0.1) {
          scale.value -= 0.1;
        }
      };

      return { scaleUp, scaleDown, scale };
    },
    methods: {
      onDrop(event) {
        event.preventDefault();
        const file = event.dataTransfer.files[0];
        if (file.type.startsWith('image/')) {
          const reader = new FileReader();
          reader.onload = (e) => {
            const img = new Image();
            img.onload = () => {
              // 이미지 사이즈 검사 후에 imgae-loaded 이벤트 발생
              if (img.width > 500 || img.height > 400) {
                alert('500x400 이상의 이미지는 업로드할 수 없습니다.');
              } else {
                this.$emit('image-dropped', e.target.result);
                this.$emit('image-loaded');  // 이벤트 발생
                this.imageLoaded = true;
              }
          };
        img.src = e.target.result;
      };
      reader.readAsDataURL(file);
    } else {
      alert('이미지 파일만 업로드 가능합니다.');
    }
    },
    setImageSrc(src) {
    this.imageSrc = src;
    this.imageLoaded = true;  // imageLoaded 값을 true로 설정
  },
    },

  };
</script>
  
<style scoped>
  .container {
    position: relative;
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-top: 20px; /*드래그 앤 드롭박스와 버튼 간 간격 설정*/
  }

  .zoomButtons {
    position: absolute;
    top: 10px;
    left: 0;
    
    height: 40px;
    font-size: 45px;
    font-weight: bold;
    display: flex;
    flex-direction: row;
    
  }
  .dropzone {
    width: 600px;
    height: 600px;
    border: 2px dashed #ccc;
    position: relative;
    overflow: hidden;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    text-align: center;
    
  }
  .ellipse {
    fill: none; 
    stroke: red; 
    stroke-width: 2; 
    stroke-dasharray: 5,5;
    border-radius: 50%;
  }
  .dropzone p {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      z-index: 10;
      color: white;
    }
  #zout {
    width: 100px;
    background-color: blue; 
    color: white;
    border-radius: 10px;
    
  }
  #zin {
    width: 100px;
    background-color: aqua; 
    color: white;
    
    border-radius: 10px;
  }
</style>
  