<template>
    <div>
      <input type="file" @change="onFileChange" accept="image/*">
      <img :src="imageSrc" v-if="imageSrc"/>
    </div>
  </template>
  
  <script>
  import { ref } from 'vue';
  
  export default {
    setup() {
      const imageSrc = ref(null);
  
      const onFileChange = (e) => {
        const file = e.target.files[0];
        if (file) {
          imageSrc.value = URL.createObjectURL(file);
        }
      };
  
      return { imageSrc, onFileChange };
    }
  };
  </script>
  
  <style scoped>
  img {
    width: 300px;
  }
  </style>