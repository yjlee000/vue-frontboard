<template>
  <h1>Ellipse Labeling Component</h1>
  <div class="container">
    
    <ImageDrop @image-dropped="setImageSrc" @scaleUp="scaleUp" @scaleDown="scaleDown" @image-loaded="imageLoaded = true">
      <ImageMove :src="imageSrc" :scale="scale">
        <EllipseDraw v-if="imageLoaded" :imageLoaded="imageLoaded" />
      </ImageMove>
    </ImageDrop>
  </div>
</template>

<script>
import { ref } from 'vue';
import ImageDrop from './components/ImageDrop.vue';
import ImageMove from './components/ImageMove.vue';
import EllipseDraw from './components/EllipseDraw.vue';

export default {
  data() {
    return {
      imageSrc: '',
      scale: 1,
      imageLoaded: false,
      ellipseDragging: false,
      imagePanning: false,
    };
  },
  methods: {
    setImageSrc(src) {
      this.imageSrc = src;
    },
  },
  components: { ImageDrop, ImageMove, EllipseDraw },
  setup() {
  const imageSrc = ref(null);
  const scale = ref(1);

  const setImageSrc = (src) => {
    imageSrc.value = src;
  };

  const scaleUp = () => {
    scale.value += 0.1;
  };

  const scaleDown = () => {
    scale.value -= 0.1;
  };

  // imageLoaded ref 삭제
  return { imageSrc, setImageSrc, scale, scaleUp, scaleDown };
},
};
</script>

<style>
body {
  background-color: rgb(43,44,64);
}
h1 {
  color: white;
}
.container {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100vh;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>