<template>
  <div class="my-canvas-wrapper">
    <canvas ref="my-canvas">
      
    </canvas>
    <div class="tooltip">
      <span class="tooltiptext"></span>
    </div>
    <slot></slot>
  </div>
</template>

<script>
export default {
  data() {
    return {
      // By creating the provider in the data property, it becomes reactive,
      // so child components will update when `context` changes.
      provider: {
        // This is the CanvasRenderingContext that children will draw to.
        context: null
      }
    }
  },

  // Allows any child component to `inject: ['provider']` and have access to it.
  provide () {
    return {
      provider: this.provider
    }
  },

  mounted () {
    // We can't access the rendering context until the canvas is mounted to the DOM.
    // Once we have it, provide it to all child components.
    this.provider.context = this.$refs['my-canvas'].getContext('2d')

    // Resize the canvas to fit its parent's width.
    // Normally you'd use a more flexible resize system.
    this.$refs['my-canvas'].width = this.$refs['my-canvas'].parentElement.clientWidth
    this.$refs['my-canvas'].height = this.$refs['my-canvas'].parentElement.clientHeight
    this.$refs['my-canvas'].offsetX = this.$refs['my-canvas'].parentElement.clientLeft
    this.$refs['my-canvas'].offsetY = this.$refs['my-canvas'].parentElement.clientTop
    // this.$refs['my-canvas'].offsetX = this.$refs['my-canvas'].parentElement.getBoundingClientRect.left
    // this.$refs['my-canvas'].offsetY = this.$refs['my-canvas'].parentElement.getBoundingClientRect.top
  },
  methods: {
    mouseover() {

    }
  }
}
</script>

<style>
html, body {
  margin: 0;
  padding: 0;
}

.tooltip {
  position: absolute;
  background-color: red;
  z-index: 9999;
  display: block;
  top: 0;
  left: 0;
  width: 200px;
  height: 50px;
  /* display: inline-block;
  border-bottom: 1px dotted black; */
}

.tooltip .tooltiptext {
  /*visibility: hidden;*/
  width: 120px;
  background-color: #555;
  color: #fff;
  text-align: center;
  border-radius: 6px;
  padding: 5px 0;
  position: absolute;
  z-index: 1;
  top: 0;
  left: 50%;
  margin-left: -60px;
  /*opacity: 0;
  transition: opacity 0.3s;*/
}

.tooltip .tooltiptext::after {
  content: "";
  position: absolute;
  top: 100%;
  left: 50%;
  margin-left: -5px;
  border-width: 5px;
  border-style: solid;
  border-color: #555 transparent transparent transparent;
}

.tooltip:hover .tooltiptext {
  visibility: visible;
  opacity: 1;
}
</style>