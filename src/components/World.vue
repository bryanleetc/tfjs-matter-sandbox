<template>
  <canvas ref="world" class="world"></canvas>
</template>

<script>
import Matter from 'matter-js';

export default {
  mounted() {
    this.setupStage();
  },
  methods: {
    setupStage() {
      const Engine = Matter.Engine,
            Render = Matter.Render,
            World = Matter.World,
            Bodies = Matter.Bodies;
      // const Composites = Matter.Composites;

      const engine = Engine.create();
      const render = Render.create({
        canvas: this.$refs.world,
        engine,
      });

      Engine.run(engine);
      Render.run(render);

      let ball = Bodies.circle(200, 10, 40, { isStatic: true });
      World.add(engine.world, [ball]);
    },
  },
};
</script>

<style lang="scss">
.world {
  height: 800px;
  width: (800 / 1.3)px;
}
</style>