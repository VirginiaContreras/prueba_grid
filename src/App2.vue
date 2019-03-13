<template>
  <div id="app">
    <h2>Calendario</h2>
    <!-- 600 -->
    <my-canvas style="width: 3508px; height: 3548px;">
      <my-cell
        v-for="(obj, index) in schedule"  :key="index"
        :tipo="obj.tipo"
        :texto="obj.texto"
        :scheduleStartHour="obj.scheduleStartHour"
        :scheduleStartMin="obj.scheduleStartMin"
        :scheduleEndHour="obj.scheduleEndHour"
        :scheduleEndMin="obj.scheduleEndMin"
        :transmitionDays="obj.transmitionDays"
      >
      </my-cell>
      <!--<ValidationDay v-for="(obj, index) in schedule" :key="index" :schedule="obj"></ValidationDay>-->
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
        {texto: 'LUNES', tipo: 'cabecera', scheduleStartHour: '', scheduleStartMin: '', scheduleEndHour: '', scheduleEndMin: '', transmitionDays:[]},
        {texto: 'MARTES', tipo: 'cabecera', scheduleStartHour: '', scheduleStartMin: '', scheduleEndHour: '', scheduleEndMin: '', transmitionDays:[]},
        {texto: 'MIERCOLES', tipo: 'cabecera', scheduleStartHour: '', scheduleStartMin: '', scheduleEndHour: '', scheduleEndMin: '', transmitionDays:[]},
        {texto: 'JUEVES', tipo: 'cabecera', scheduleStartHour: '', scheduleStartMin: '', scheduleEndHour: '', scheduleEndMin: '', transmitionDays:[]},
        {texto: 'VIERNES', tipo: 'cabecera', scheduleStartHour: '', scheduleStartMin: '', scheduleEndHour: '', scheduleEndMin: '', transmitionDays:[]},
        {texto: 'SABADO', tipo: 'cabecera', scheduleStartHour: '', scheduleStartMin: '', scheduleEndHour: '', scheduleEndMin: '', transmitionDays:[]},
        {texto: 'DOMINGO', tipo: 'cabecera', scheduleStartHour: '', scheduleStartMin: '', scheduleEndHour: '', scheduleEndMin: '', transmitionDays:[]},
        {texto: 'AMERICA NOTICIAS: PRIMERA EDICION: LOCAL (GP)', tipo: 'celda', scheduleStartHour: '05', scheduleStartMin: '16', scheduleEndHour: '06', scheduleEndMin: '30', transmitionDays: ['Lunes','Martes','Miércoles','Viernes','Sábado']},
        {texto: 'AMERICA NOTICIAS: PRIMERA EDICION (GP)', tipo: 'celda', scheduleStartHour: '06', scheduleStartMin: '30', scheduleEndHour: '08', scheduleEndMin: '00', transmitionDays: ['Lunes','Martes','Jueves','Sábado','Domingo']},
        {texto: 'AMERICA DEPORTE (G)', tipo: 'celda', scheduleStartHour: '08', scheduleStartMin: '00', scheduleEndHour: '08', scheduleEndMin: '30', transmitionDays: ['Lunes','Miércoles','Jueves','Sábado','Domingo']},
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
    
    this.schedule.forEach((item, index, object) => {
      if (item.tipo == 'celda') {
        let daysNumber = []
        
        //Creando nuevo array daysNumber para evaluar si los días son consecutivos asignandoles un valor numérico a los transmitionDays
        for (let i = 0; i < item.transmitionDays.length; i++) {
          
          if ( item.transmitionDays[i].includes('Lunes') ) {
            daysNumber.push(1)
          }
          else if ( item.transmitionDays[i].includes('Martes') ) {
            daysNumber.push(2)
          }
          else if ( item.transmitionDays[i].includes('Miércoles') ) {
            daysNumber.push(3)
          }
          else if ( item.transmitionDays[i].includes('Jueves') ) {
            daysNumber.push(4)
          }
          else if ( item.transmitionDays[i].includes('Viernes') ) {
            daysNumber.push(5)
          }
          else if ( item.transmitionDays[i].includes('Sábado') ) {
            daysNumber.push(6)
          }
          else if ( item.transmitionDays[i].includes('Domingo') ) {
            daysNumber.push(7)
          }
        }
        
        //Evaluando si son consecutivos
        if ( item.transmitionDays.length > 1 ) {
          
          //agrupar dias de semana en cadena, separado por guiones
          let a = ''
          
          for (let j = 0; j < daysNumber.length; j++) {
            
            if ( daysNumber[j] === (daysNumber[j + 1] - 1) ) {						
              
              if(a === '' || a.charAt(a.length-1) === '-'){
                a += daysNumber[j]
              }							
              a += daysNumber[j + 1]													 					
            }
            else {
              if(a === '' || a.charAt(a.length-1) === '-'){
                a += daysNumber[j]
              }
              a += '-'
            }
          
          }

          a = a.substr(0, a.length-1)  
          console.log(a);        

          //separar grupos por guion
          let arrayGrupos = a.split('-')
          //console.log(arrayGrupos);

          //solo rearmar array si el array tiene mas de un elemento
          if (arrayGrupos.length > 1) {

            arrayGrupos.forEach(grupo => {

              let arrDays = [];

              for (let i = 0; i < grupo.length; i++) {
                switch (grupo[i]) {
                  case '1':
                    arrDays.push('Lunes');
                    break;
                  case '2':
                    arrDays.push('Martes');
                    break;
                  case '3':
                    arrDays.push('Miércoles');
                    break;
                  case '4':
                    arrDays.push('Jueves');
                    break;
                  case '5':
                    arrDays.push('Viernes');
                    break;
                  case '6':
                    arrDays.push('Sábado');
                    break;
                  case '7':
                    arrDays.push('Domingo');
                    break;
                }
              } 

              let newArray = {texto: item.texto, tipo: item.tipo, scheduleStartHour: item.scheduleStartHour, scheduleStartMin: item.scheduleStartMin, scheduleEndHour: item.scheduleEndHour, scheduleEndMin: item.scheduleEndMin, transmitionDays: arrDays}

              //pushear el nuevo array con dias agrupados
              this.schedule.push(newArray);
            });

            //quitar el elemento original del array
            this.schedule.splice(index, 1)
          }
          
        }
      }
    }); //fin del loop

    //console.log(this.schedule);
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