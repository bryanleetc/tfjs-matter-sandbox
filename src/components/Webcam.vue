<template>
  <video ref="webcam" autoplay playsinline muted class="webcam"></video>
</template>

<script>
export default {
  props: {
    live: {
      type: Boolean,
      default: false,
    },
  },

  data() {
    return {
      videoStream: null,
    };
  },

  watch: {
    live() {
      if (!this.live) {
        this.videoStream.stop();
        this.videoStream = null;
        return;
      }
      this.setupWebcam();
    },
  },

  methods: {
    setupWebcam() {
      navigator.mediaDevices.getUserMedia({ video: true })
        .then((stream) => {
          this.$refs.webcam.srcObject = stream;
          // console.log(stream);
          // this.$refs.webcam.srcObject = stream;
          // // this.$refs.webcam.addEventListener('loadeddata', () => resolve(), false);
          // this.$refs.webcam.onloadedmetadata = () => {
          //   this.$refs.webcam.play();
          //   this.videoStream = stream.getTracks()[0];
          //   console.log('hello');
          // }
        // eslint-disable-next-line no-console
        }).catch((e) => { console.error(e) });
    },
  },
};
</script>

<style lang="scss">
.webcam {
  width: 1200px;
  height: (1200 / 1.3)px;
}
</style>