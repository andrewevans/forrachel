<template>
  <div class="fib-lines">

    <b-card title="Colors" style="max-width: 20rem;" class="mb-2">
      <b-card-text>
        <p>Hold <i>Command/Ctrl</i> to select multiple</p>
        <select v-model="colors" multiple>
          <option v-for="(option, i) in colorList" :key="i">{{ option }}</option>
        </select>
      </b-card-text>
    </b-card>

    <b-card title="Origin" style="max-width: 20rem;" class="mb-2">
      <b-card-text>
        <b-input-group prepend="0" :append="`${maxWidth}`" class="mt-3">
          <b-form-input v-model="origin" type="range" min="0" :max="`${maxWidth}`"></b-form-input>
        </b-input-group>
      </b-card-text>
    </b-card>

    <b-card title="Direction" style="max-width: 20rem;" class="mb-2">
      <b-card-text>
        <select v-model="direction">
          <option v-for="(option, i) in directions" :key="i">{{ option }}</option>
        </select>
      </b-card-text>
    </b-card>

    <b-card title="Unit" style="max-width: 20rem;" class="mb-2">
      <b-card-text>
        <select v-model="unit">
          <option v-for="(option, i) in units" :key="i">{{ option }}</option>
        </select>
      </b-card-text>
    </b-card>

    <b-card title="Algorithm" style="max-width: 20rem;" class="mb-2">
      <b-card-text>
        <select v-model="algorithm">
          <option v-for="(option, i) in algorithms" :key="i">{{ option }}</option>
        </select>
      </b-card-text>
    </b-card>

    <b-card title="Set Details" style="max-width: 20rem;" class="mb-2">
      <b-card-text>
        <div>Colors: <pre>{{ colors }}</pre></div>
        <div>Origin: <pre>{{ origin }}</pre></div>
        <div>Direction: <pre>{{ direction }}</pre></div>
        <div>Unit: <pre>{{ unit }}</pre></div>
        <div>Algorithm: <pre>{{ algorithm }}</pre></div>
      </b-card-text>
    </b-card>

    <canvas id="canvas" width="400" height="400"></canvas>
  </div>
</template>

<script>
  export default {
    name: 'FibLines',
    props: {
      msg: String
    },
    data() {
      return {
        colors: [
          'black', // Default colors
        ],
        origin: 0, // Default origin
        direction: 'right', // Default direction
        unit: 10, // Default unit
        algorithm: 'fibonacci', // Default algorithm
        colorList: [
          'aqua',
          'black',
          'blue',
          'fuchsia',
          'gray',
          'green',
          'lime',
          'maroon',
          'navy',
          'olive',
          'purple',
          'red',
          'silver',
          'teal',
          'white',
          'yellow',
        ],
        directions : ['left', 'right', 'up', 'down'],
        units: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 15, 20, 25, 30, 35, 40, 50, 60, 75, 100, 200],
        algorithms: ['fibonacci', 'goldenRatio', 'random'],
        maxWidth: window.innerWidth,
      };
    },
    methods: {
      fibonacci(n) {
        return n < 2 ? n : this.fibonacci(n - 1) + this.fibonacci(n - 2);
      },
      goldenRatio(n) {
        return n < 2 ? n : this.goldenRatio(n - 1) * 1.6180339;
      },
      random(n) {
        return n * Math.random() * 10;
      },
      drawLines() {
        const canvas = document.getElementById('canvas');
        const ctx = canvas.getContext('2d');
        const fullWidth = canvas.width;
        const fullHeight = canvas.height;
        const opposite = (this.direction === 'left' || this.direction === 'up' ? -1 : 1);

        ctx.canvas.width  = window.innerWidth - 200;
        ctx.canvas.height = window.innerHeight - 200;
        ctx.lineWidth = 1;

        let start = 0;
        let i = 2;
        const colors = this.colors;

        while (i < 20) {
          start = parseInt(this.origin, 10) + (opposite * (this[this.algorithm](i) * this.unit));
          ctx.beginPath();

          switch (this.direction) {
            case 'left':
              ctx.moveTo(start, 0);
              ctx.lineTo(start, fullHeight);
              break;

            case 'right':
              ctx.moveTo(start, 0);
              ctx.lineTo(start, fullHeight);
              break;

            case 'up':
              ctx.moveTo(0, start);
              ctx.lineTo(fullWidth, start);
              break;

            case 'down':
              ctx.moveTo(0, start);
              ctx.lineTo(fullWidth, start);
              break;

          }
          ctx.strokeStyle = colors[i % 3];
          ctx.stroke();

          window.console.info(i, i % 3, start);
          i++;
        }
      },
    },
    mounted() {
      this.drawLines();
    },
    updated() {
      this.drawLines();
    }
  };
</script>

<style scoped>
  #canvas {
    /*width: 100vh;*/
    /*height: 100vh;*/
    background: bisque;
  }
</style>
