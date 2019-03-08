<template>
  <div id="app">
    <h2>Calendario</h2>
    <!-- 600 -->
    <my-canvas style="width: 3508px; height: 3548px;">
      <my-cell
        v-for="(obj, index) in schedule"  :key="index"
        :tipo="obj.tipo"
        :texto="obj.texto"
        :startHour="obj.startHour"
        :endHour="obj.endHour"
        :transmitionDays="obj.transmitionDays"
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
      // arrayDaysUno: ['Lunes','Martes'],
      // arrayDaysDos: ['Lunes','Martes','Miércoles'],
      schedule: [        
        // {texto: '', tipo: 'hora', x: 0, y: 0, startHour: '', endHour: ''},
        // {texto: '05:15', tipo: 'hora', x: 0, y: 31, startHour: '', endHour: ''},
        // {texto: '05:30', tipo: 'hora', x: 0, y: 71, startHour: '', endHour: ''},
        // {texto: '05:45', tipo: 'hora', x: 0, y: 111, startHour: '', endHour: ''},
        // {texto: '06:00', tipo: 'hora', x: 0, y: 151, startHour: '', endHour: ''},
        {texto: 'LUNES', tipo: 'cabecera', x: 0, y: 0, startHour: '', endHour: '', transmitionDays:''},
        {texto: 'MARTES', tipo: 'cabecera', x: 0, y: 0, startHour: '', endHour: '', transmitionDays:''},
        {texto: 'MIERCOLES', tipo: 'cabecera', x: 0, y: 0, startHour: '', endHour: '', transmitionDays:''},
        // {texto: 'JUEVES', tipo: 'cabecera', x: 885, y: 0, startHour: '', endHour: '', transmitionDays:''},
        // {texto: 'VIERNES', tipo: 'cabecera', x: 1160, y: 0, startHour: '', endHour: '', transmitionDays:''},
        // {texto: 'SABADO', tipo: 'cabecera', x: 1435, y: 0, startHour: '', endHour: '', transmitionDays:''},
        // {texto: 'DOMINGO', tipo: 'cabecera', x: 1710, y: 0, startHour: '', endHour: '', transmitionDays:''},
        {texto: 'AMERICA NOTICIAS: PRIMERA EDICION: LOCAL (GP)', tipo: 'celda', startHour: 'March 13, 08 05:16', endHour: 'March 13, 08 05:35', transmitionDays: ['Lunes','Martes','Miércoles']},//x: 360, y: (3508 / (24 * 60)) * 15
        {texto: 'AMERICA NOTICIAS: PRIMERA EDICION (GP)', tipo: 'celda', startHour: 'March 13, 08 05:36', endHour: 'March 13, 08 05:50', transmitionDays: ['Martes','Viernes']},//x: 61, y: (3508 / (24 * 60)) * 15 + 40
        {texto: 'ESPECTACULOS', tipo: 'celda', startHour: 'March 13, 08 05:51', endHour: 'March 13, 08 06:50', transmitionDays: ['Miércoles','Viernes']}
      ]
    }
  },
  created() {
    //console.log(this.arrayDaysUno)
    //console.log(this.arrayDaysDos)
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