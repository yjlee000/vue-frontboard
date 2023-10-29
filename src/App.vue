<template>
  <PageHeader/> <!-- 헤더 컴포넌트 -->
  <MainContent/> <!--바디 컴포넌트-->
  <router-view/>  <!-- 페이지 이동이 표시될 곳 -->
  <PageFooter/> <!-- 푸터 컴포넌트 -->
</template>

<script>
import PageHeader from '@/components/PageHeader'
import MainContent from '@/components/ImageUploader.vue'
// import ImageUpload from '@/components/ImageUpload.vue'
// import ImageZoomPan from '@/components/ImageZoomPan.vue'
// import EllipseZoomPan from '@/components/EllipseZoomPan.vue'
import PageFooter from '@/components/PageFooter';


export default {
  name: 'App',
  components: {
    PageHeader,
    MainContent,
    PageFooter
  }
}
</script>

<style>
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