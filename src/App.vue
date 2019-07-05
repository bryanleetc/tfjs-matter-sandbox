<template>
  <div id="app">
    <div class="interface">
      <button @click="isWebcamPlaying = !isWebcamPlaying">{{isWebcamPlaying ? 'Stop' : 'Start'}}</button>
    </div>

    <div class="main">
      <World />
      <Webcam />
    </div>
  </div>
</template>

<script>
import World from './components/World.vue';
import Webcam from './components/Webcam.vue';

export default {
  name: 'app',
  components: {
    World,
    Webcam,
  },
  data() {
    return {
      isWebcamPlaying: false,
    }
  },
  methods: {

  }
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
