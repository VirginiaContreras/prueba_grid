<template>
  <div id="app">
    <h2>Calendario</h2>
    <!-- 600 -->
    <my-canvas style="width: 3508px; height: 3548px;">
      <!-- <my-cell
        v-for="(obj, index) in schedule"  :key="index"
        :tipo="obj.tipo"
        :texto="obj.texto"
        :scheduleStartHour="obj.scheduleStartHour"
        :scheduleStartMin="obj.scheduleStartMin"
        :scheduleEndHour="obj.scheduleEndHour"
        :scheduleEndMin="obj.scheduleEndMin"
        :transmitionDays="obj.transmitionDays"
      >
      </my-cell> -->
      <ValidationDay v-for="(obj, index) in schedule" :key="index" :schedule="obj"></ValidationDay>
    </my-canvas>
  </div>
</template>

<script>
import MyCanvas from './components/MyCanvas.vue';
import MyCell from './components/MyCell.vue';
import ValidationDay from './components/ValidationDay.vue';
export default {
  name: 'app',
  components: {
    MyCanvas,
    MyCell,
    ValidationDay
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
        {texto: 'LUNES', tipo: 'cabecera', scheduleStartHour: '', scheduleStartMin: '', scheduleEndHour: '', scheduleEndMin: '', transmitionDays:[]},
        {texto: 'MARTES', tipo: 'cabecera', scheduleStartHour: '', scheduleStartMin: '', scheduleEndHour: '', scheduleEndMin: '', transmitionDays:[]},
        {texto: 'MIERCOLES', tipo: 'cabecera', scheduleStartHour: '', scheduleStartMin: '', scheduleEndHour: '', scheduleEndMin: '', transmitionDays:[]},
        {texto: 'JUEVES', tipo: 'cabecera', scheduleStartHour: '', scheduleStartMin: '', scheduleEndHour: '', scheduleEndMin: '', transmitionDays:[]},
        {texto: 'VIERNES', tipo: 'cabecera', scheduleStartHour: '', scheduleStartMin: '', scheduleEndHour: '', scheduleEndMin: '', transmitionDays:[]},
        {texto: 'SABADO', tipo: 'cabecera', scheduleStartHour: '', scheduleStartMin: '', scheduleEndHour: '', scheduleEndMin: '', transmitionDays:[]},
        {texto: 'DOMINGO', tipo: 'cabecera', scheduleStartHour: '', scheduleStartMin: '', scheduleEndHour: '', scheduleEndMin: '', transmitionDays:[]},
        {texto: 'AMERICA NOTICIAS: PRIMERA EDICION: LOCAL (GP)', tipo: 'celda', scheduleStartHour: '05', scheduleStartMin: '16', scheduleEndHour: '06', scheduleEndMin: '30', transmitionDays: ['Lunes','Martes','Miércoles', 'Viernes','Sábado']},//x: 360, y: (3508 / (24 * 60)) * 15
        {texto: 'AMERICA NOTICIAS: PRIMERA EDICION (GP)', tipo: 'celda', scheduleStartHour: '06', scheduleStartMin: '30', scheduleEndHour: '08', scheduleEndMin: '00', transmitionDays: ['Lunes','Martes','Miércoles','Jueves','Viernes','Sábado', 'Domingo']},//x: 61, y: (3508 / (24 * 60)) * 15 + 40
        {texto: 'AMERICA DEPORTE (G)', tipo: 'celda', scheduleStartHour: '08', scheduleStartMin: '00', scheduleEndHour: '08', scheduleEndMin: '30', transmitionDays: ['Miércoles','Jueves']},
        // {texto: 'AMERICA ESPECTACULOS (REPETICION)', tipo: 'celda', startHour: 'March 13, 08 05:16', endHour: 'March 13, 08 06:25', transmitionDays: ['Martes']},
        // {texto: 'AMERICA NOTICIAS: MATUTINO (GP)', tipo: 'celda', startHour: 'March 13, 08 06:25', endHour: 'March 13, 08 06:50', transmitionDays: ['Martes']},
        // {texto: 'AMERICA NOTICIAS: EDICION DEL SABADO (GP)', tipo: 'celda', startHour: 'March 13, 08 06:50', endHour: 'March 13, 08 08:35', transmitionDays: ['Martes']},
        // {texto: 'AMERICA NOTICIAS: PRIMERA EDICION: LOCAL (GP)', tipo: 'celda', startHour: 'March 13, 08 05:16', endHour: 'March 13, 08 06:50', transmitionDays: ['Miércoles']},
        // {texto: 'AMERICA NOTICIAS: EDICION DEL SABADO (GP)', tipo: 'celda', startHour: 'March 13, 08 06:50', endHour: 'March 13, 08 08:35', transmitionDays: ['Miércoles']},
        // {texto: 'AMERICA NOTICIAS: EDICION DEL SABADO (GP)', tipo: 'celda', startHour: 'March 13, 08 08:35', endHour: 'March 13, 08 09:35', transmitionDays: ['Miércoles']},
        // {texto: 'AMERICA NOTICIAS: EDICION DEL SABADO (GP)', tipo: 'celda', startHour: 'March 13, 08 09:35', endHour: 'March 13, 08 12:35', transmitionDays: ['Miércoles']},
        // {texto: 'AMERICA NOTICIAS: EDICION DEL SABADO (GP)', tipo: 'celda', startHour: 'March 13, 08 12:35', endHour: 'March 13, 08 13:35', transmitionDays: ['Miércoles']},
        // {texto: 'AMERICA NOTICIAS: EDICION DEL JUEVES (GP)', tipo: 'celda', startHour: 'March 13, 08 05:16', endHour: 'March 13, 08 00:00', transmitionDays: ['Jueves']},
        // {texto: 'AMERICA NOTICIAS: EDICION DEL JUEVES 2 (GP)', tipo: 'celda', startHour: 'March 13, 08 00:00', endHour: 'March 13, 08 05:15', transmitionDays: ['Jueves']},
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