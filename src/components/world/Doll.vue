<script>
import Matter from 'matter-js';

let World;
let Bodies;

export default {
  props: {
    world: {
      required: true,
    },
  },
  data() {
    return {
      head: null,
      commonOptions: {
        render: {
          fillStyle: '#FF9800',
        },
      },
    };
  },
  render() {
    return null;
  },
  created() {
    World = Matter.World;
    Bodies = Matter.Bodies;
  },
  mounted() {
    this.createHead();
  },
  methods: {
    createHead() {
      this.head = Bodies.rectangle(200, 200, 50, 70, {
        chamfer: {
          radius: [20, 20, 20, 20],
        },
        ...this.commonOptions,
      });
      World.add(this.world, [this.head]);
    }
  }
}
</script>
