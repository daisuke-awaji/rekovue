<template>
  <div id="app">
    <img src="./assets/logo.png">
    <HelloWorld/>
    <!-- <Rekognition/> -->
    <PhotoUpload/>
    <PhotoList/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld'
import Rekognition from './components/Rekognition'
import PhotoUpload from './components/PhotoUpload'
import PhotoList from './components/PhotoList'

export default {
  name: 'App',
  components: {
    HelloWorld, Rekognition, PhotoUpload, PhotoList
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
