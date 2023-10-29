// ImageUpload.vue
<template>
  <body>
    <div 
        class="dropzone" 
        @dragover.prevent 
        @drop="onDrop"
    >
    <p v-if="!imageSrc">여기에 이미지를 드래그 앤 드롭하세요.</p>
    <slot></slot>
  </div>
</body>
</template>
  

<script>
import { ref } from 'vue';

export default {
  props: ['onUpload'],
  setup(props) {
    const imageSrc = ref(null);

    const onDrop = (e) => {
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
      props.onUpload(imageSrc.value);
    };

    return { imageSrc, onDrop };
  },
};
</script>
