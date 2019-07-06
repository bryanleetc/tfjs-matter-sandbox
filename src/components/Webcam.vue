<template>
  <video ref="webcam" autoplay playsinline muted class="webcam" :width="config.width" :height="config.height"></video>
</template>

<script>
import * as tf from '@tensorflow/tfjs'
import * as posenet from '@tensorflow-models/posenet';

export default {
  props: {
    live: {
      type: Boolean,
      default: false,
    },
    globalConfig: {
      type: Object,
      required: true,
    },
  },

  computed: {
    config() { return this.globalConfig.canvas },
  },

  data() {
    return {
      tfModel: null,
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

  async mounted() {
    this.tfModel = await posenet.load();
  },

  methods: {
    async setupWebcam() {
      let pose;
      await this.streamWebcam();

      while(true) {
        pose = await this.tfModel.estimateSinglePose(this.$refs.webcam, { flipHorizontal: false });
        this.$emit('streaming', pose);
        await tf.nextFrame();
      }
    },

    async streamWebcam() {
      const webcamEl = this.$refs.webcam;
      const vm = this;

      return new Promise((resolve, reject) => {
        navigator.mediaDevices.getUserMedia({ video: true })
          .then((stream) => {
            webcamEl.srcObject = stream;
            vm.videoStream = stream.getTracks()[0];

            webcamEl.addEventListener('loadeddata', () => resolve(), false);
          })
          .catch(() => reject());
      });
    },
  },
};
</script>

<style lang="scss">
.webcam {
  position: absolute;
  left: 50%;
  top: 0;
  transform: translateX(-50%);
  z-index: 0;
}
</style>