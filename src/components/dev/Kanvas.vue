<template>
  <canvas class="kanvas" :width="config.width" :height="config.height"></canvas>
</template>

<script>
export default {
  props: {
    globalConfig: {
      type: Object,
      required: true,
    },
  },

  computed: {
    config() {
      return this.globalConfig.canvas;
    },
  },
};
</script>

<style lang="scss">

</style>
