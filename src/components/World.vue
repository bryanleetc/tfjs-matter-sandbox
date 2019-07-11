<template>
  <canvas ref="world" class="world"></canvas>
</template>

<script>
import Matter from 'matter-js';

let Engine;
let Render;
let World;
let Bodies;

export default {
  props: {
    pose: {
      type: Array,
      default: () => ([]),
    },
  },
  data() {
    return {
      head: null,
      engine: {},
    };
  },
  watch: {
    pose() {
      this.trackHead();
    },
  },
  mounted() {
    this.setupStage();
  },
  methods: {
    setupStage() {
      Engine = Matter.Engine;
      Render = Matter.Render;
      World = Matter.World;
      Bodies = Matter.Bodies;
      // const Composites = Matter.Composites;

      this.engine = Engine.create();
      const render = Render.create({
        canvas: this.$refs.world,
        engine: this.engine,
      });

      Engine.run(this.engine);
      Render.run(render);

      let ball = Bodies.circle(200, 10, 40, { isStatic: true });
      World.add(this.engine.world, [ball]);
    },

    trackHead() {
      const nose = this.pose.find((part) => part.part === 'nose');
      const {x, y} = nose.position;

      if (nose.score < 0.6) return;

      if (!this.head) {
        this.createHead(x, y);
        return;
      }

      Matter.Body.setPosition(this.head, { x, y });
    },

    createHead(x, y) {
      this.head = Bodies.circle(x, y, 100, { isStatic: true });
      World.add(this.engine.world, [this.head]);
    },
  },
};
</script>

<style lang="scss">
.world {
  height: 800px;
  width: (800 / 1.3)px;
  position: absolute;
  left: 50%;
  top: 0;
  transform: translateX(-50%);
  z-index: 1;
}
</style>