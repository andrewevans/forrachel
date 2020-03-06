<template>
  <div class="fib-lines">
    <div class="canvas-controls">
      <div class="row">
        <div class="col-md-2">
          <button
            @click="addLineSet()"
            class="add-set btn btn-primary">
            Add Set
          </button>
        </div>
      </div>
      <div class="line-sets row">
        <div
          v-for="(canvas, i) in canvases"
          :key="i"
          class="col-md-2">
          <line-set :canvas="canvas" :ctx="ctxes[i]"></line-set>
        </div>
      </div>
    </div>

    <div id="canvas-park">
    </div>
  </div>
</template>

<script>
  import LineSet from './LineSet.vue';

  export default {
    name: 'FibLines',
    props: {
      msg: String
    },
    data() {
      return {
        canvases: [],
        ctxes: [],
        canvasPark: {},
        ctx: {
          canvas: {},
        },
        canvas: {},
        ctx2: {
          canvas: {},
        },
        canvas2: {},
      };
    },
    components: {
      LineSet,
    },
    methods: {
      addLineSet() {
        const canvas = document.createElement('canvas');
        const canvasPark = document.getElementById('canvas-park');
        const ctx = canvas.getContext('2d');
        canvas.id = `canvas-${this.canvases.length}`;

        ctx.lineWidth = 1;
        ctx.canvas.width  = window.innerWidth - 100;
        ctx.canvas.height = window.innerHeight - 100;

        this.canvases.push(canvas);
        this.ctxes.push(ctx);

        canvasPark.append(canvas);
      },
    },
    mounted() {
      this.canvasPark = document.getElementById('canvas-park');
      this.canvasPark.style.width  = `${window.innerWidth - 100}px`;
      this.canvasPark.style.height = `${window.innerHeight - 100}px`;
    },
  };
</script>

<style lang="scss">
  #canvas-park {
    position: relative;
    background: #fefefe;
    border: 1px solid #cccccc;

    canvas {
      /*width: 100vh;*/
      /*height: 100vh;*/
      position: absolute;
      display: block;
      background: transparent;
    }
  }
  .add-set {
    margin: 0 2em 2em 2em;
  }
</style>
