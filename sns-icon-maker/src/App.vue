<template>
  <img alt="Vue logo" src="./assets/logo.png" />
  <HelloWorld msg="Hello Vue 3 + Vite" />
  <imgPreview :path="path" :face-parts="faceParts" :type-names="typeNames" face-tag="girl" />
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import faceParts from './model/faceParts'
import { typeNames } from './model/names.js'
import imgPreview from './components/imgPreview.vue'

export default {
  components: {
    HelloWorld,
    imgPreview,
  },
  setup() {
    const path = {
      faceParts: '/src/assets/images/face-parts/',
      faceIcon: '/src/assets/images/face-icon/',
      other: '/src/assets/images/other/',
    }
    return {
      path,
      faceParts,
      typeNames
    }
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
