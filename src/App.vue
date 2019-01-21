<template>
  <div id="app">
    <double-video :bottomVideoUrl="bottomVideoUrl" :topVideoUrl="topVideoUrl" @changeposition="changePosition" @videomouseover="videoMouseover" @videomouseout="videoMouseout" @videoplay="videoPlay"></double-video>
  </div>
</template>
<script>
import doubleVideo from 'double-video'
export default {
  name: 'App',
  components: { doubleVideo },
  data() {
    return {
      bottomVideoUrl: require("@/assets/side.mp4"),
      topVideoUrl: require("@/assets/top.mp4")
    }
  },
  methods: {
    changePosition() {},
    videoMouseover() {},
    videoMouseout() {},
    videoPlay() {},
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
