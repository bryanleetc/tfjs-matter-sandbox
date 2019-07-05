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
      return new Promise((resolve, reject) => {
        const navigatorAny = navigator
        navigator.getUserMedia = navigator.getUserMedia ||
                                  navigatorAny.webkitGetUserMedia || navigatorAny.mozGetUserMedia ||
                                  navigatorAny.msGetUserMedia

        if (!navigator.getUserMedia) {
          reject()
        }

        navigator.getUserMedia({ video: true },
          (stream) => {
            this.$refs.webcam.srcObject = stream;
            this.$refs.webcam.addEventListener('loadeddata', () => resolve(), false);
            this.videoStream = stream.getTracks()[0];
          },
          () => reject(),
        )
      });
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