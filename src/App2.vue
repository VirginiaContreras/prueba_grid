<template>
  <div id="app">
    <h2>Calendario</h2>
    <my-canvas style="width: 100%; height: 600px;">
      <my-cell
        v-for="(obj, index) in schedule"  :key="index"
        :x="obj.x"
        :y="obj.y"
        :ancho="obj.ancho"
        :alto="obj.alto"
        :tipo="obj.tipo"
        :texto="obj.texto"
      >
      </my-cell>
    </my-canvas>
  </div>
</template>

<script>
import MyCanvas from './components/MyCanvas.vue';
import MyCell from './components/MyCell.vue';

export default {
  name: 'app',
  components: {
    MyCanvas,
    MyCell
  },

  data () {
    return {
      schedule: [
        {texto: 'AMERICA NOTICIAS: PRIMERA EDICION: LOCAL (GP)', tipo: 'celda', x: 0, y: 0, ancho: 500, alto: 50},
        {texto: 'AMERICA NOTICIAS: PRIMERA EDICION (GP)', tipo: 'celda', x: 0, y: 31, ancho: 500, alto: 50},
      ]
    }
  }
}
</script>

<style>
html, body {
  margin: 0;
  padding: 0;
}

#app {
  position: relative;
  height: 100vh;
  width: 100vw;
  padding: 20px;
  box-sizing: border-box;
}
</style>