<template>
  <div id="app">
    <div class="interface">
      <button @click="isWebcamPlaying = !isWebcamPlaying">{{isWebcamPlaying ? 'Stop' : 'Start'}}</button>
    </div>

    <div class="main">
      <World :globalConfig="globalConfig" :pose="pose"></World>
      <Webcam :globalConfig="globalConfig" :live="isWebcamPlaying" @streaming="onStreaming" />
    </div>

  </div>
</template>

<script>
// <World :pose="pose" />
import Kanvas from './components/dev/Kanvas.vue';
import World from './components/World.vue';
import Webcam from './components/Webcam.vue';

export default {
  name: 'app',
  components: {
    Kanvas,
    World,
    Webcam,
  },
  data() {
    return {
      globalConfig: {
        canvas: {
          width: 800,
          height: 800 / 1.3,
        },
      },
      isWebcamPlaying: false,
      pose: null,
      overallScore: null,
    }
  },
  methods: {
    onStreaming(pose) {
      if (pose.score < 0.2) return;

      this.overallScore = pose.score;
      this.pose = pose.keypoints;
    },
  },
}
</script>

<style lang="scss">
@import "./styles/reset";

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.interface {
  position: absolute;
}
</style>
