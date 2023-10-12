<!-- MatterAnimation.vue -->

<template>
    
    <div id="matter-container"></div>
  </template>
  
  <script>
  import { Engine, Render, World, Bodies } from 'matter-js';
  
  export default {
    name: 'MatterAnimation',
    data() {
    return {
      engine: null,
      idRAF: null
    };
  },
  mounted() {
    this.init();
    window.addEventListener('resize', this.init);
  },

  beforeDestroy() {
    window.removeEventListener('resize', this.init);
    if (this.idRAF) {
      cancelAnimationFrame(this.idRAF);
    }
  },
    methods: {
        init() {
      let numm = Math.random();

      let canvases = document.querySelectorAll("canvas");
      canvases.forEach(canvas => canvas.remove());

      let width = window.innerWidth/2;
      let height = window.innerHeight/2;
      let vmin = Math.min(width, height)/2;

      if (this.engine) {
        this.engine.events = {};
        World.clear(this.engine.world);
        Engine.clear(this.engine);
      }

      this.engine = Engine.create();

      let render = Render.create({
        element: this.$el,
        engine: this.engine,
        options: {
          wireframes: false,
          background: 'transparent',
          width: width,
          height: height
        }
      });
  
      World.add(this.engine.world, [
          Bodies.rectangle(width / 2, height + 50, width, 100, {
            isStatic: true
          }),
          Bodies.rectangle(width / 2, -50, width, 100, {
            isStatic: true
          }),
          Bodies.rectangle(-50, height / 2, 100, height, {
            isStatic: true
          }),
          Bodies.rectangle(width + 50, height / 2, 100, height, {
            isStatic: true
          }),
          Bodies.rectangle(width / 2, height / 2, vmin * 0.961, vmin * 0.135, {
            isStatic: true,
            render: {
              fillStyle: "white"
            }
          })
        ]);
  
        for (let i = 0; i < 50; i++) {
          let radius = Math.round(20);
          World.add(this.engine.world, Bodies.circle(
            Math.random() * width,
            Math.random() * height / 4,
            radius, {
              render: {
                fillStyle: ['#EA1070', '#EAC03C', '#25DDBC', '#007DB0', '#252B7F', '#FF6040'][Math.round(Math.random() * 6 - 0.5)]
              }
            }
          ))
        }
  
        Engine.run(this.engine);
        Render.run(render);
        this.update();
      },
  
        // Resto del código ...
  
      update() {
        let num = 0;
  
        const loop = () => {
            this.engine.world.gravity.x = Math.sin(num / 100);
            this.engine.world.gravity.y = Math.cos(num / 100);
            num += 2.5;
            requestAnimationFrame(loop);
      };
        loop();
      }
    }
  }
  </script>
  
  <style scoped>

  </style>
  