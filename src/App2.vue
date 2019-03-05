<template>
  <div id="app">
    <h2>Calendario</h2>
    <my-canvas style="width: 3508px; height: 600px;">
      <my-cell
        v-for="(obj, index) in schedule"  :key="index"
        :x="obj.x"
        :y="obj.y"
        :tipo="obj.tipo"
        :texto="obj.texto"
        :startHour="obj.startHour"
        :endHour="obj.endHour"
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
        {texto: '', tipo: 'hora', x: 0, y: 0, startHour: '', endHour: ''},
        {texto: '05:15', tipo: 'hora', x: 0, y: 31, startHour: '', endHour: ''},
        {texto: '05:30', tipo: 'hora', x: 0, y: 71, startHour: '', endHour: ''},
        {texto: '05:45', tipo: 'hora', x: 0, y: 111, startHour: '', endHour: ''},
        {texto: '06:00', tipo: 'hora', x: 0, y: 151, startHour: '', endHour: ''},
        {texto: 'LUNES', tipo: 'cabecera', x: 60, y: 0, startHour: '', endHour: ''},
        {texto: 'MARTES', tipo: 'cabecera', x: 335, y: 0, startHour: '', endHour: ''},
        {texto: 'MIERCOLES', tipo: 'cabecera', x: 610, y: 0, startHour: '', endHour: ''},
        {texto: 'JUEVES', tipo: 'cabecera', x: 885, y: 0, startHour: '', endHour: ''},
        {texto: 'VIERNES', tipo: 'cabecera', x: 1160, y: 0, startHour: '', endHour: ''},
        {texto: 'SABADO', tipo: 'cabecera', x: 1435, y: 0, startHour: '', endHour: ''},
        {texto: 'DOMINGO', tipo: 'cabecera', x: 1710, y: 0, startHour: '', endHour: ''},
        {texto: 'AMERICA NOTICIAS: PRIMERA EDICION: LOCAL (GP)', tipo: 'celda', x: 60, y: 40, startHour: 'March 13, 08 05:20', endHour: 'March 13, 08 05:30'},
        //{texto: 'AMERICA NOTICIAS: PRIMERA EDICION (GP)', tipo: 'celda', x: 100, y: 80, startHour: 'March 13, 08 05:31', endHour: 'March 13, 08 06:30'},
      ]
      // schedule: [
      //   {texto: '', tipo: 'cabecera', x: 0, y: 0, ancho: 100, alto: 40},
      //   {texto: '05:15', tipo: 'cabecera', x: 0, y: 31, ancho: 100, alto: 40},
      //   {texto: '05:30', tipo: 'cabecera', x: 0, y: 71, ancho: 100, alto: 40},
      //   {texto: '05:45', tipo: 'cabecera', x: 0, y: 111, ancho: 100, alto: 40},
      //   {texto: '06:00', tipo: 'cabecera', x: 0, y: 151, ancho: 100, alto: 40},
      //   {texto: 'LUNES', tipo: 'cabecera', x: 100, y: 0, ancho: 120, alto: 40},
      //   {texto: 'MARTES', tipo: 'cabecera', x: 220, y: 0, ancho: 120, alto: 40},
      //   {texto: 'MIERCOLES', tipo: 'cabecera', x: 340, y: 0, ancho: 120, alto: 40},
      //   {texto: 'JUEVES', tipo: 'cabecera', x: 460, y: 0, ancho: 120, alto: 40},
      //   {texto: 'VIERNES', tipo: 'cabecera', x: 580, y: 0, ancho: 120, alto: 40},
      //   {texto: 'SABADO', tipo: 'cabecera', x: 700, y: 0, ancho: 120, alto: 40},
      //   {texto: 'DOMINGO', tipo: 'cabecera', x: 820, y: 0, ancho: 120, alto: 40},
      //   {texto: 'AMERICA NOTICIAS: PRIMERA EDICION: LOCAL (GP)', tipo: 'celda', x: 100, y: 40, ancho: 500, alto: 50},
      //   {texto: 'AMERICA NOTICIAS: PRIMERA EDICION (GP)', tipo: 'celda', x: 100, y: 80, ancho: 500, alto: 50},
      // ]
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