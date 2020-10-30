<template>
  <div id="app">
    <Canvas></Canvas>
  </div>
</template>

<script>


import Canvas from "./components/Canvas";
export default {
  name: 'App',
  components: {
    Canvas
  }
}
</script>

<style>
  * { margin:0; padding:0; }
  html, body{
    overflow: hidden;
   width:100%;
   height:100%;
 }
#app {

}
</style>
