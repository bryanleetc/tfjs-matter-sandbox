<template>
  <canvas ref="kanvas" class="kanvas" :width="config.width" :height="config.height"></canvas>
</template>

<script>
export default {
  props: {
    pose: {
      type: Array,
      default: () => ([]),
    },
    globalConfig: {
      type: Object,
      required: true,
    },
  },

  watch: {
    pose() {
      this.ctx.clearRect(0, 0, this.config.width, this.config.height);
      this.pose.forEach(({ score, position }) => {
        if (score < 0.5) return;
        this.drawPoint(position.x, position.y);
      });
    },
  },

  data() {
    return {
      ctx: null,
    };
  },

  computed: {
    config() {
      return this.globalConfig.video;
    },
  },

  mounted() {
    this.ctx = this.$refs.kanvas.getContext('2d');
    this.ctx.save();
  },

  methods: {
    drawPoint(x, y) {
      if (!this.ctx) return;
      this.ctx.fillRect(x, y, 3, 3);
    },
  }
};
</script>

<style lang="scss">
.kanvas {
  position: absolute;
  left: 0;
  z-index: 1;
}
</style>
