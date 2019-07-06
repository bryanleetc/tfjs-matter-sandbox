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
      this.pose.forEach(({ position }) => {
        this.ctx.clearRect(0, 0, this.config.width, this.config.height);
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
      return this.globalConfig.canvas;
    },
  },

  mounted() {
    this.ctx = this.$refs.kanvas.getContext('2d');
  },

  methods: {
    drawPoint(x, y) {
      if (!this.ctx) return;
      this.ctx.arc(x, y, 5, 0, (Math.PI/180) * 360);
      this.ctx.fill();
    },
  }
};
</script>

<style lang="scss">
.kanvas {
  position: absolute;
  left: 50%;
  top: 0;
  transform: translateX(-50%);
  z-index: 1;
}
</style>
