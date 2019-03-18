<template>
  <div id="app">
    <h2>Calendario</h2>
    <!-- 600 -->
    <my-canvas style="width: 3508px; height: 3589px;">
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
        {texto: 'AMERICA NOTICIAS: PRIMERA EDICION: LOCAL (GP)', tipo: 'celda', scheduleStartHour: '05', scheduleStartMin: '16', scheduleEndHour: '06', scheduleEndMin: '50', transmitionDays: ['Lunes','Martes','Miércoles','Jueves','Viernes']},
        {texto: 'AMERICA ESPECTACULOS (REPETICION)', tipo: 'celda', scheduleStartHour: '05', scheduleStartMin: '30', scheduleEndHour: '06', scheduleEndMin: '30', transmitionDays: ['Sábado']},
        {texto: 'AUTOMUNDO', tipo: 'celda', scheduleStartHour: '05', scheduleStartMin: '30', scheduleEndHour: '06', scheduleEndMin: '00', transmitionDays: ['Domingo']},
        {texto: 'AMERICA NOTICIAS: PRIMERA EDICION (GP)', tipo: 'celda', scheduleStartHour: '06', scheduleStartMin: '50', scheduleEndHour: '08', scheduleEndMin: '20', transmitionDays: ['Lunes','Martes','Miércoles','Jueves','Viernes']},
        {texto: 'AMERICA NOTICIAS MATUTINO (GP)', tipo: 'celda', scheduleStartHour: '06', scheduleStartMin: '30', scheduleEndHour: '07', scheduleEndMin: '00', transmitionDays: ['Sábado']},
        {texto: 'ESTAS EN TODAS (REPETICION) (GP)', tipo: 'celda', scheduleStartHour: '06', scheduleStartMin: '00', scheduleEndHour: '06', scheduleEndMin: '50', transmitionDays: ['Domingo']},        
        {texto: 'AMERICA DEPORTES (G)', tipo: 'celda', scheduleStartHour: '08', scheduleStartMin: '20', scheduleEndHour: '08', scheduleEndMin: '30', transmitionDays: ['Lunes','Martes','Miércoles','Jueves','Viernes']},
        {texto: 'AMERICA NOTICIAS: EDICION DEL SABADO (GP)', tipo: 'celda', scheduleStartHour: '07', scheduleStartMin: '00', scheduleEndHour: '09', scheduleEndMin: '00', transmitionDays: ['Sábado']},
        {texto: 'AMERICA NOTICIAS: EDICIÓN DOMINICAL (GP)', tipo: 'celda', scheduleStartHour: '06', scheduleStartMin: '50', scheduleEndHour: '08', scheduleEndMin: '30', transmitionDays: ['Domingo']}, 
        {texto: 'AMERICA NOTICIAS: PRIMERA EDICION (GP)', tipo: 'celda', scheduleStartHour: '08', scheduleStartMin: '30', scheduleEndHour: '09', scheduleEndMin: '30', transmitionDays: ['Lunes','Martes','Miércoles','Jueves','Viernes']},
        {texto: '+ ESPECTACULOS (G)', tipo: 'celda', scheduleStartHour: '09', scheduleStartMin: '30', scheduleEndHour: '10', scheduleEndMin: '00', transmitionDays: ['Lunes','Martes','Miércoles','Jueves','Viernes']},
        {texto: 'TN. EL PRIVILEGIO DE AMAR (GP)', tipo: 'celda', scheduleStartHour: '10', scheduleStartMin: '00', scheduleEndHour: '11', scheduleEndMin: '00', transmitionDays: ['Lunes','Martes','Miércoles','Jueves','Viernes']},
        {texto: 'TN. MARIA MERCEDES (GP)', tipo: 'celda', scheduleStartHour: '11', scheduleStartMin: '00', scheduleEndHour: '12', scheduleEndMin: '00', transmitionDays: ['Lunes','Martes','Miércoles','Jueves','Viernes']},
        {texto: 'AMERICA NOTICIAS: EDICION DEL MEDIODIA (GP)', tipo: 'celda', scheduleStartHour: '12', scheduleStartMin: '00', scheduleEndHour: '13', scheduleEndMin: '00', transmitionDays: ['Lunes','Martes','Miércoles','Jueves','Viernes']},
        {texto: 'EN BOCA DE TODOS (GP)', tipo: 'celda', scheduleStartHour: '13', scheduleStartMin: '00', scheduleEndHour: '14', scheduleEndMin: '40', transmitionDays: ['Lunes','Martes','Miércoles','Jueves','Viernes']},
        {texto: 'ALFONDO HAY SITIO (5° TEMPORADA) (G)', tipo: 'celda', scheduleStartHour: '14', scheduleStartMin: '40', scheduleEndHour: '15', scheduleEndMin: '50', transmitionDays: ['Lunes','Martes','Miércoles','Jueves','Viernes']},
        {texto: 'TN. CAER EN TENTACION', tipo: 'celda', scheduleStartHour: '15', scheduleStartMin: '50', scheduleEndHour: '16', scheduleEndMin: '40', transmitionDays: ['Lunes']},
        {texto: 'TN. CAER EN TENTACION', tipo: 'celda', scheduleStartHour: '15', scheduleStartMin: '50', scheduleEndHour: '16', scheduleEndMin: '50', transmitionDays: ['Martes','Miércoles','Jueves','Viernes']},
        {texto: 'LA ROSA DE GUADALUPE (GP)/RERUN', tipo: 'celda', scheduleStartHour: '16', scheduleStartMin: '40', scheduleEndHour: '17', scheduleEndMin: '00', transmitionDays: ['Lunes']},
        {texto: 'CUMBIA POP (GP) (FIN)', tipo: 'celda', scheduleStartHour: '17', scheduleStartMin: '00', scheduleEndHour: '18', scheduleEndMin: '00', transmitionDays: ['Lunes']},
        {texto: 'LA ROSA DE GUADALUPE (GP)/RERUN', tipo: 'celda', scheduleStartHour: '18', scheduleStartMin: '00', scheduleEndHour: '18', scheduleEndMin: '50', transmitionDays: ['Lunes']},
        {texto: 'LA ROSA DE GUADALUPE (GP)/RERUN ESTRENO', tipo: 'celda', scheduleStartHour: '16', scheduleStartMin: '50', scheduleEndHour: '18', scheduleEndMin: '50', transmitionDays: ['Martes']},
        {texto: 'LA ROSA DE GUADALUPE (GP)/RERUN', tipo: 'celda', scheduleStartHour: '16', scheduleStartMin: '50', scheduleEndHour: '18', scheduleEndMin: '50', transmitionDays: ['Miércoles']},
        {texto: 'LA ROSA DE GUADALUPE (GP)/RERUN ESTRENO', tipo: 'celda', scheduleStartHour: '16', scheduleStartMin: '50', scheduleEndHour: '18', scheduleEndMin: '50', transmitionDays: ['Jueves']},
        {texto: 'LA ROSA DE GUADALUPE (GP)/RERUN', tipo: 'celda', scheduleStartHour: '16', scheduleStartMin: '50', scheduleEndHour: '18', scheduleEndMin: '50', transmitionDays: ['Viernes']},
        {texto: 'ESTO ES GUERRA (GP)', tipo: 'celda', scheduleStartHour: '18', scheduleStartMin: '50', scheduleEndHour: '20', scheduleEndMin: '30', transmitionDays: ['Lunes','Martes','Miércoles','Jueves','Viernes']},
        {texto: 'DE VUELTA AL BARRIO (2° TEMPORADA) (GP)', tipo: 'celda', scheduleStartHour: '20', scheduleStartMin: '30', scheduleEndHour: '21', scheduleEndMin: '30', transmitionDays: ['Lunes','Martes','Miércoles','Jueves','Viernes']},
        {texto: 'TN. OJITOS HECHICEROS (GP)', tipo: 'celda', scheduleStartHour: '21', scheduleStartMin: '30', scheduleEndHour: '22', scheduleEndMin: '30', transmitionDays: ['Lunes','Martes','Miércoles','Jueves','Viernes']},
        {texto: 'AMERICA NOTICIAS: EDICION CENTRAL (GP)', tipo: 'celda', scheduleStartHour: '21', scheduleStartMin: '30', scheduleEndHour: '23', scheduleEndMin: '20', transmitionDays: ['Lunes','Martes','Miércoles','Jueves','Viernes']},
        {texto: '36 AÑOS DESPUES (G)', tipo: 'celda', scheduleStartHour: '23', scheduleStartMin: '20', scheduleEndHour: '23', scheduleEndMin: '40', transmitionDays: ['Lunes','Martes','Miércoles','Jueves','Viernes']},
        {texto: 'LA BANDA DEL CHINO (MAY 14)', tipo: 'celda', scheduleStartHour: '23', scheduleStartMin: '40', scheduleEndHour: '00', scheduleEndMin: '40', transmitionDays: ['Lunes','Martes','Miércoles','Jueves','Viernes']},
        {texto: 'MICRO PROGRAMA: "EGS: EL ARTISTA DEL AÑO"', tipo: 'celda', scheduleStartHour: '00', scheduleStartMin: '40', scheduleEndHour: '00', scheduleEndMin: '42', transmitionDays: ['Lunes','Martes','Miércoles','Jueves','Viernes']},
        {texto: 'TN. AMOR Y CASTIGO (MAY 14)', tipo: 'celda', scheduleStartHour: '00', scheduleStartMin: '42', scheduleEndHour: '01', scheduleEndMin: '30', transmitionDays: ['Lunes','Martes','Miércoles','Jueves','Viernes']},
        {texto: 'TN. SIN TU MIRADA (GP)', tipo: 'celda', scheduleStartHour: '01', scheduleStartMin: '30', scheduleEndHour: '02', scheduleEndMin: '30', transmitionDays: ['Lunes','Martes','Miércoles','Jueves','Viernes']},
        {texto: 'EN BOCA DE TODOS (REPETICION) (GP)', tipo: 'celda', scheduleStartHour: '02', scheduleStartMin: '30', scheduleEndHour: '03', scheduleEndMin: '45', transmitionDays: ['Lunes','Martes','Miércoles','Jueves','Viernes']},
        {texto: 'TN. EL PRIVILEGIO DE AMAR (REPETICION) (GP)', tipo: 'celda', scheduleStartHour: '03', scheduleStartMin: '45', scheduleEndHour: '04', scheduleEndMin: '30', transmitionDays: ['Lunes','Martes','Miércoles','Jueves','Viernes']},
        {texto: 'AMERICA ESPECTACULOS (REPETICION) (GP)', tipo: 'celda', scheduleStartHour: '04', scheduleStartMin: '30', scheduleEndHour: '05', scheduleEndMin: '16', transmitionDays: ['Lunes','Martes','Miércoles','Jueves']},
        {texto: 'AMERICA NOTCIAS EDICION CENTRAL (REPETICION) (GP)', tipo: 'celda', scheduleStartHour: '04', scheduleStartMin: '30', scheduleEndHour: '05', scheduleEndMin: '16', transmitionDays: ['Viernes']},
        {texto: '(GP) ESTAS EN TODAS (5TA TEMPORADA)', tipo: 'celda', scheduleStartHour: '09', scheduleStartMin: '00', scheduleEndHour: '11', scheduleEndMin: '00', transmitionDays: ['Sábado']},
        {texto: 'CINESCAPE (G)', tipo: 'celda', scheduleStartHour: '11', scheduleStartMin: '00', scheduleEndHour: '12', scheduleEndMin: '00', transmitionDays: ['Sábado']},
        {texto: '(G) EL CHAVO DEL OCHO', tipo: 'celda', scheduleStartHour: '12', scheduleStartMin: '00', scheduleEndHour: '13', scheduleEndMin: '00', transmitionDays: ['Sábado']},
        {texto: '"SERIE" (GP) SOLAMENTE MILAGROS (1° TEMPORADA) "HOY SERAS MIA"', tipo: 'celda', scheduleStartHour: '13', scheduleStartMin: '00', scheduleEndHour: '14', scheduleEndMin: '00', transmitionDays: ['Sábado']},
        {texto: '(G) EL CHAVO DEL OCHO', tipo: 'celda', scheduleStartHour: '14', scheduleStartMin: '00', scheduleEndHour: '15', scheduleEndMin: '20', transmitionDays: ['Sábado']},
        {texto: '"MATINEE AMERICA - SABADO" YOKO (G)/RERUN JAMIE BICK FRIEDRICH HEINE', tipo: 'celda', scheduleStartHour: '15', scheduleStartMin: '20', scheduleEndHour: '17', scheduleEndMin: '00', transmitionDays: ['Sábado']},
        {texto: '"SERIE" LA ROSA DE GUADALUPE (GP)/RERUN', tipo: 'celda', scheduleStartHour: '17', scheduleStartMin: '00', scheduleEndHour: '18', scheduleEndMin: '00', transmitionDays: ['Sábado']},
        {texto: 'LA PREVIA', tipo: 'celda', scheduleStartHour: '18', scheduleStartMin: '00', scheduleEndHour: '19', scheduleEndMin: '00', transmitionDays: ['Sábado']},
        {texto: 'EL REVENTONAZO DE LA CHOLA (GP)', tipo: 'celda', scheduleStartHour: '19', scheduleStartMin: '00', scheduleEndHour: '21', scheduleEndMin: '00', transmitionDays: ['Sábado']},
        {texto: 'EGS "EL ARTISTA DEL AÑO" (1° TEMPORADA) (GP)', tipo: 'celda', scheduleStartHour: '21', scheduleStartMin: '00', scheduleEndHour: '23', scheduleEndMin: '30', transmitionDays: ['Sábado']},
        {texto: 'MAY 18 MUJERES SIN FILTRO (4° TEMPORADA)', tipo: 'celda', scheduleStartHour: '23', scheduleStartMin: '30', scheduleEndHour: '00', scheduleEndMin: '30', transmitionDays: ['Sábado']},
        {texto: '"MINISERIE" MOBY DICK (GP)/RERUN WILLIAM HURT ETHAN HAWKE', tipo: 'celda', scheduleStartHour: '00', scheduleStartMin: '30', scheduleEndHour: '03', scheduleEndMin: '30', transmitionDays: ['Sábado']},
        {texto: 'AMERICA NOTICIAS: EDICIOON DEL SABADO (REPETICION) (GP)', tipo: 'celda', scheduleStartHour: '03', scheduleStartMin: '30', scheduleEndHour: '05', scheduleEndMin: '00', transmitionDays: ['Sábado']},
        {texto: 'BOB ESPONJA (G)', tipo: 'celda', scheduleStartHour: '05', scheduleStartMin: '00', scheduleEndHour: '05', scheduleEndMin: '16', transmitionDays: ['Sábado']},
        {texto: 'DOMINGO AL DIA (GP)', tipo: 'celda', scheduleStartHour: '08', scheduleStartMin: '30', scheduleEndHour: '11', scheduleEndMin: '00', transmitionDays: ['Domingo']},
        {texto: 'TEC (G)', tipo: 'celda', scheduleStartHour: '11', scheduleStartMin: '00', scheduleEndHour: '12', scheduleEndMin: '00', transmitionDays: ['Domingo']},
        {texto: 'EL CHAVO DEL OCHO (G)', tipo: 'celda', scheduleStartHour: '12', scheduleStartMin: '00', scheduleEndHour: '14', scheduleEndMin: '00', transmitionDays: ['Domingo']},
        {texto: '"BUTACA AMERICA" MINIESPIAS 3 FIN DEL JUEGO (G)/RERUN ANTONIO BANDERAS ALEXA VEGA', tipo: 'celda', scheduleStartHour: '14', scheduleStartMin: '00', scheduleEndHour: '15', scheduleEndMin: '20', transmitionDays: ['Domingo']},
        {texto: '"BUTACA AMERICA" RAPIDOS Y FURIOSOS: SIN CONTROL (MAY 14)/RERUN VIN DIESEL PAUL WALKER', tipo: 'celda', scheduleStartHour: '15', scheduleStartMin: '20', scheduleEndHour: '17', scheduleEndMin: '30', transmitionDays: ['Domingo']},
        {texto: '"SERIE" LA ROSA DE GUADALUPE (GP)/ESTRENO', tipo: 'celda', scheduleStartHour: '17', scheduleStartMin: '30', scheduleEndHour: '19', scheduleEndMin: '00', transmitionDays: ['Domingo']},
        {texto: 'MI MAMA COCINA MEJOR QUE LA TUYA (2° TEMPORADA) (GP)', tipo: 'celda', scheduleStartHour: '19', scheduleStartMin: '00', scheduleEndHour: '20', scheduleEndMin: '00', transmitionDays: ['Domingo']},
        {texto: 'CUARTO PODER (GP)', tipo: 'celda', scheduleStartHour: '20', scheduleStartMin: '00', scheduleEndHour: '22', scheduleEndMin: '30', transmitionDays: ['Domingo']},
        {texto: 'FUTBOL EN AMERICA (G)', tipo: 'celda', scheduleStartHour: '22', scheduleStartMin: '30', scheduleEndHour: '00', scheduleEndMin: '00', transmitionDays: ['Domingo']},
        {texto: '"BUTACA AMERICA" CAPITAN PHILLIPS (GP)/ESTRENO TOM HANKS BARKHAD ABDI', tipo: 'celda', scheduleStartHour: '00', scheduleStartMin: '00', scheduleEndHour: '02', scheduleEndMin: '00', transmitionDays: ['Domingo']},
        {texto: 'AMERICA NOTICIAS: EDICION DOMINICAL (REPETICION) (GP)', tipo: 'celda', scheduleStartHour: '02', scheduleStartMin: '00', scheduleEndHour: '03', scheduleEndMin: '30', transmitionDays: ['Domingo']},
        {texto: 'CUARTO PODER (REPETICION) (GP)', tipo: 'celda', scheduleStartHour: '03', scheduleStartMin: '30', scheduleEndHour: '05', scheduleEndMin: '16', transmitionDays: ['Domingo']},
        {texto: 'LUNES', tipo: 'cabecera', scheduleStartHour: '', scheduleStartMin: '', scheduleEndHour: '', scheduleEndMin: '', transmitionDays:[]},
        {texto: 'MARTES', tipo: 'cabecera', scheduleStartHour: '', scheduleStartMin: '', scheduleEndHour: '', scheduleEndMin: '', transmitionDays:[]},
        {texto: 'MIERCOLES', tipo: 'cabecera', scheduleStartHour: '', scheduleStartMin: '', scheduleEndHour: '', scheduleEndMin: '', transmitionDays:[]},
        {texto: 'JUEVES', tipo: 'cabecera', scheduleStartHour: '', scheduleStartMin: '', scheduleEndHour: '', scheduleEndMin: '', transmitionDays:[]},
        {texto: 'VIERNES', tipo: 'cabecera', scheduleStartHour: '', scheduleStartMin: '', scheduleEndHour: '', scheduleEndMin: '', transmitionDays:[]},
        {texto: 'SABADO', tipo: 'cabecera', scheduleStartHour: '', scheduleStartMin: '', scheduleEndHour: '', scheduleEndMin: '', transmitionDays:[]},
        {texto: 'DOMINGO', tipo: 'cabecera', scheduleStartHour: '', scheduleStartMin: '', scheduleEndHour: '', scheduleEndMin: '', transmitionDays:[]},
      ]
    }
  },
  created() {

    let indexs = []
    let newElements = []
    
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
              //this.schedule.push(newArray);
              newElements.push(newArray)
            });

            //quitar el elemento original del array
            //this.schedule.splice(index, 1)
            indexs.push(index)
          }
          
        }
      }
    }); //fin del loop

    for(let i = 0; i < indexs.length; i++) {
      this.schedule.splice(indexs[i], 1)
    }

    for(let j = 0; j < newElements.length; j++) {
      this.schedule.push(newElements[j]);
    }

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