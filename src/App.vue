<template>
  <div id="app">
    <Webcam></Webcam>
  </div>
</template>

<script>
import Webcam from "./components/Webcam";

export default {
  name: "app",
  components: {
    Webcam
  }
};
</script>

<style lang="scss">
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
