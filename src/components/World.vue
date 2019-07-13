<template>
  <div>
    <canvas ref="world" class="world"></canvas>
    <template v-if="engine.world">
      <doll :world="engine.world" />
    </template>
  </div>
</template>

<script>
import Matter from 'matter-js';
import Doll from './world/Doll.vue';

let Engine;
let Render;
let World;
let Bodies;

export default {
  components: {
    Doll,
  },
  props: {
    nosePose: {
      type: Object,
      default: () => ({}),
    },
    globalConfig: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      head: null,
      engine: {},
    };
  },
  watch: {
    nosePose() {
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
        options: {
          width: this.globalConfig.canvas.width,
          height: this.globalConfig.canvas.height,
          wireframes: false,
        }
      });

      Engine.run(this.engine);
      Render.run(render);

      let ball = Bodies.circle(200, 10, 40, { isStatic: true });
      World.add(this.engine.world, [ball]);
    },

    trackHead() {
      const x = (1 - this.nosePose.x) * this.config.width;
      const y = this.nosePose.y * this.config.height;

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
  position: absolute;
  left: 50%;
  top: 0;
  transform: translateX(-50%);
  z-index: 1;
}
</style>