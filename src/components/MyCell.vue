<script>

const percentWidthToPix = (percent, ctx) => Math.floor((ctx.canvas.width / 100) * percent)
const percentHeightToPix = (percent, ctx) => Math.floor((ctx.canvas.height / 100) * percent)

export default {
  inject: ['provider'],
  props: {
    // x: {
    //   type: Number,
    //   default: 0
    // },
    // y: {
    //   type: Number,
    //   default: 0
    // },
    texto: {
      type: String,
      default: ''
    },
    tipo: {
      type: String,
      default: 'celda'
    },
    scheduleStartHour: {
      type: String,
      default: ''
    },
    scheduleStartMin: {
      type: String,
      default: ''
    },
    scheduleEndHour: {
      type: String,
      default: ''
    },
    scheduleEndMin: {
      type: String,
      default: ''
    },
    transmitionDays: {
      type: Array,
      default: ''
    },
    days: {
      type: Array,
      default: ''
    },
    type: {
      type: String,
      default: ''
    }
  },
  // props: [ 'texto', 'tipo']
  data () {
    return {
      // We cache the dimensions of the previous
      // render so that we can clear the area later.
      oldBox: {
        x: null,
        y: null,
        w: null,
        h: null
      },
      time: null,
      alto: null,
      ancho: null,
      x: 0,
      y: 0,
      resolutionWidth: 2480,
      resolutionHigh: 3508,
      highHeadboard: null,
      yPrev: 41,
      altoPrevCell: null,
      hourString: ''
      // yCountLun: 0,
      // yCountMar: 0,
      // yCountMie: 0,
      // yCountJue: 0,
      // yCountVie: 0,
      // yCountSab: 0,
      // yCountDom: 0,
      //timeInterval: 15,
    }
  },
  computed: {
    calculatedBox () {
      const ctx = this.provider.context

      // Turn start / end percentages into x, y, width, height in pixels.
      const calculated = {
        x: percentWidthToPix(this.x, ctx),
        y: percentHeightToPix(this.y, ctx),
        w: percentWidthToPix(this.ancho, ctx),//this.ancho
        h: percentHeightToPix(this.alto, ctx)
      }

      // Yes yes, side-effects. This lets us cache the box dimensions of the previous render.
      // before we re-calculate calculatedBox the next render.
      this.oldBox = calculated
      return calculated
    }
  },
  methods: {
    createdCell(tipo, transmitionDays) {
      // console.log(tipo, 'tipo')
      // console.log(transmitionDays, 'transmitionDays')
      if(tipo === 'celda') {
        //Calculando tiempo del programa
        let hoursStartHour = this.generateHour(this.scheduleStartHour)// horas fin
        let minutesStartHour = this.generateMinutes(this.scheduleStartMin)//minutos fin
        let hoursEndHour = this.generateHour(this.scheduleEndHour)// horas empieza
        let minutesEndHour = this.generateMinutes(this.scheduleEndMin)//minutos empieza
        

        this.hourString = this.showHoursString(hoursStartHour, minutesStartHour, hoursEndHour, minutesEndHour)
        let sumMinEndHour = (hoursEndHour * 60) + minutesEndHour
        let sumMinStartHour = (hoursStartHour * 60) + minutesStartHour
        let startTime
        //debugger
        if ( sumMinEndHour < sumMinStartHour ) {
          this.time = (24 * 60) - sumMinStartHour + sumMinEndHour
        } else {          
          this.time = (sumMinEndHour) - (sumMinStartHour)
        }
        
        //Calculando alto de la celda
        this.alto = this.time * this.calcMinHightToPixel()  
        //Calculando ancho de la celda
        //debugger
        if( this.transmitionDays.length === 1) {
          this.ancho = this.resolutionWidth/7
        } else if ( this.transmitionDays.length > 1 ) {//Si tiene más de una celda y es consecutivo
          if( this.transmitionDays.includes('Lunes') && this.transmitionDays.includes('Martes') && this.transmitionDays.includes('Miércoles') && this.transmitionDays.includes('Jueves') && this.transmitionDays.includes('Viernes') && this.transmitionDays.includes('Sábado') && this.transmitionDays.includes('Domingo')) {
            this.ancho = (this.resolutionWidth/7) * 7
          }
          else if ( (this.transmitionDays.includes('Lunes') && this.transmitionDays.includes('Martes') && this.transmitionDays.includes('Miércoles') && this.transmitionDays.includes('Jueves') && this.transmitionDays.includes('Viernes') && this.transmitionDays.includes('Sábado')) || (this.transmitionDays.includes('Martes') && this.transmitionDays.includes('Miércoles') && this.transmitionDays.includes('Jueves') && this.transmitionDays.includes('Viernes') && this.transmitionDays.includes('Sábado') && this.transmitionDays.includes('Domingo'))) {
            this.ancho = (this.resolutionWidth/7) * 6
          }
          else if ( (this.transmitionDays.includes('Lunes') && this.transmitionDays.includes('Martes') && this.transmitionDays.includes('Miércoles') && this.transmitionDays.includes('Jueves') && this.transmitionDays.includes('Viernes')) || (this.transmitionDays.includes('Martes') && this.transmitionDays.includes('Miércoles') && this.transmitionDays.includes('Jueves') && this.transmitionDays.includes('Viernes') && this.transmitionDays.includes('Sábado')) || (this.transmitionDays.includes('Miércoles') && this.transmitionDays.includes('Jueves') && this.transmitionDays.includes('Viernes') && this.transmitionDays.includes('Sábado') && this.transmitionDays.includes('Domingo'))) {
            this.ancho = (this.resolutionWidth/7) * 5
          }
          else if ((this.transmitionDays.includes('Lunes') && this.transmitionDays.includes('Martes') && this.transmitionDays.includes('Miércoles') && this.transmitionDays.includes('Jueves')) || (this.transmitionDays.includes('Martes') && this.transmitionDays.includes('Miércoles') && this.transmitionDays.includes('Jueves') && this.transmitionDays.includes('Viernes')) || (this.transmitionDays.includes('Miércoles') && this.transmitionDays.includes('Jueves') && this.transmitionDays.includes('Viernes') && this.transmitionDays.includes('Sábado')) || (this.transmitionDays.includes('Jueves') && this.transmitionDays.includes('Viernes') && this.transmitionDays.includes('Sábado') && this.transmitionDays.includes('Domingo'))) {
            this.ancho = (this.resolutionWidth/7) * 4
          }
          else if ((this.transmitionDays.includes('Lunes') && this.transmitionDays.includes('Martes') && this.transmitionDays.includes('Miércoles')) || (this.transmitionDays.includes('Martes') && this.transmitionDays.includes('Miércoles') && this.transmitionDays.includes('Jueves')) || (this.transmitionDays.includes('Miércoles') && this.transmitionDays.includes('Jueves') && this.transmitionDays.includes('Viernes')) || (this.transmitionDays.includes('Jueves') && this.transmitionDays.includes('Viernes') && this.transmitionDays.includes('Sábado')) || (this.transmitionDays.includes('Viernes') && this.transmitionDays.includes('Sábado') && this.transmitionDays.includes('Domingo'))) {
            this.ancho = (this.resolutionWidth/7) * 3
          }
          else if ( (this.transmitionDays.includes('Lunes') && this.transmitionDays.includes('Martes')) || (this.transmitionDays.includes('Martes') && this.transmitionDays.includes('Miércoles')) || (this.transmitionDays.includes('Miércoles') && this.transmitionDays.includes('Jueves')) || (this.transmitionDays.includes('Jueves') && this.transmitionDays.includes('Viernes')) || (this.transmitionDays.includes('Viernes') && this.transmitionDays.includes('Sábado')) || (this.transmitionDays.includes('Sábado') && this.transmitionDays.includes('Domingo')) ) {
            this.ancho = (this.resolutionWidth/7) * 2
          }          
        }   
        
        //Calculando Y
        startTime = ((hoursStartHour * 60) + minutesStartHour) - 40
    
        if ( hoursStartHour <= 5 && minutesStartHour <= 15) {
          this.y = (((24 * 60) + startTime )* this.calcMinHightToPixel()) - 632 
        } else {
          this.y = (startTime * this.calcMinHightToPixel()) - 632 //pixeles desde 00:00 hasta 05:16
        }
        
        // console.log(startTime)
        // console.log(this.y)
        // console.log(this.time)
        // console.log(this.alto)
        //transmitionDays[0] indica en que día comienza el horario estableciendo 'X'
        if ( this.transmitionDays.length !== 0) {
          switch(this.transmitionDays[0]) {
            case "Lunes":
              this.x = 0;
              break;
            case "Martes":
              //Calculando X de la celda
              this.x = this.calcWidth() + 1;
              break;
            case "Miércoles":
              //Calculando X de la celda
              this.x = this.calcWidth() * 2 + 1;
              break;
            case "Jueves":
              //Calculando X de la celda
              this.x = this.calcWidth() * 3 + 1;
              break;
            case "Viernes":
              //Calculando X de la celda
              this.x = this.calcWidth() * 4 + 1;
              break;
            case "Sábado":
              //Calculando X de la celda
              this.x = this.calcWidth() * 5 + 1;
              break;
            case "Domingo":
              //Calculando X de la celda
              this.x = this.calcWidth() * 6 + 1;
          }   
        }
        
        //Calculando posición 'Y' en función al tiempo en pixeles
        //debugger
        if( hoursStartHour === 5 && minutesStartHour === 16) {//Calculando el y por tiempo en pixeles
          //Calculando la primera celda
          //this.y = this.yPrev
          // console.log(this.y, 'y1')
          // console.log(this.yPrev, 'yPrev1')
          localStorage.setItem("yPrev", this.yPrev);
          localStorage.setItem("altoPrev", this.alto);
        }
         /*else {             
          switch(transmitionDays[0]) {
            case "Lunes":
              this.x = 0;
              break;
            case "Martes":
              //Reiniciando valores de Y - verificando si es la primera celda Martes
              if ( Number(localStorage.getItem('yCountMar')) === 0 ) {
                localStorage.setItem("yPrev", this.yPrev);
              }           
              this.yCountMar++
              localStorage.setItem("yCountMar", this.yCountMar)
              break;
            case "Miércoles":
              //Reiniciando valores de Y - verificando si es la primera celda Miércoles
              if ( Number(localStorage.getItem('yCountMie')) === 0 ) {
                localStorage.setItem("yPrev", this.yPrev);
              }              
              this.yCountMie++
              localStorage.setItem("yCountMie", this.yCountMie)
              break;
            case "Jueves":
              //Reiniciando valores de Y - verificando si es la primera celda Jueves
              if ( Number(localStorage.getItem('yCountJue')) === 0 ) {
                localStorage.setItem("yPrev", this.yPrev);
              }              
              this.yCountJue++
              localStorage.setItem("yCountJue", this.yCountJue)
              break;
            case "Viernes":
              //Reiniciando valores de Y - verificando si es la primera celda Viernes
              if ( Number(localStorage.getItem('yCountVie')) === 0 ) {
                localStorage.setItem("yPrev", this.yPrev);
              }              
              this.yCountVie++
              localStorage.setItem("yCountVie", this.yCountVie)
              //Calculando X de la celda
              this.x = (this.ancho) * 4 + 1;
              break;
            case "Sábado":
              //Reiniciando valores de Y - verificando si es la primera celda Sábado
              if ( Number(localStorage.getItem('yCountSab')) === 0 ) {
                localStorage.setItem("yPrev", this.yPrev);
              }              
              this.yCountSab++
              localStorage.setItem("yCountSab", this.yCountSab)
              //Calculando X de la celda
              this.x = (this.ancho) * 5 + 1;
              break;
            case "Domingo":
              //Reiniciando valores de Y - verificando si es la primera celda Domingo
              if ( Number(localStorage.getItem('yCountDom')) === 0 ) {
                localStorage.setItem("yPrev", this.yPrev);
              }              
              this.yCountDom++
              localStorage.setItem("yCountDom", this.yCountDom)
              //Calculando X de la celda
              this.x = (this.ancho) * 6 + 1;
          }  
          //Calculando Y de las celdas posteriores 
          let altoPrev = localStorage.getItem('altoPrev')  
          let yPrev = localStorage.getItem('yPrev')  
          this.y = Number(yPrev) + Number(altoPrev)
          console.log('Datos')
          console.log(yPrev,'yPrev')
          console.log(altoPrev,'altoPrev')
          console.log(Number(yPrev) + Number(altoPrev),'suma')
          console.log(this.y, 'y')        
          console.log(altoPrev, 'altoPrevCell')   
          console.log(this.yPrev, 'yPrev')
          localStorage.setItem("yPrev", this.y);
          localStorage.setItem("altoPrev", this.alto);
        }*/
        
      } 
      else if( tipo === 'cabecera') {
        this.time = ''
        this.alto = 40 //36.54
        this.highHeadboard = this.alto
        this.ancho = this.resolutionWidth / 7//2480
        this.y = 0       

        if(this.texto == 'LUNES'){
          this.x = 0
        }
        else if(this.texto == 'MARTES'){
          this.x = (this.resolutionWidth / 7) + 1
        }
        else if(this.texto == 'MIERCOLES'){
          this.x = ((this.resolutionWidth / 7) * 2) + 1
        }
        else if(this.texto == 'JUEVES'){
          this.x = ((this.resolutionWidth / 7) * 3) + 1
        }
        else if(this.texto == 'VIERNES'){
          this.x = ((this.resolutionWidth / 7) * 4) + 1
        }
        else if(this.texto == 'SABADO'){
          this.x = ((this.resolutionWidth / 7) * 5) + 1
        }
        else if(this.texto == 'DOMINGO'){
          this.x = ((this.resolutionWidth / 7) * 6) + 1
        }
      }
    },
    generateHour(hour) {
      return parseInt(hour)
    },
    generateMinutes(minutes) {
      return parseInt(minutes)
    },
    calcTime(endHour, startHour) {
      this.time = (new Date(this.endHour).getHours() - new Date(this.startHour).getHours())*60
    },
     calcMinHightToPixel () {
      return this.resolutionHigh / (24 * 60)
    },
    showHoursString (hoursStartHour, minutesStartHour, hoursEndHour, minutesEndHour) {
      if ( hoursStartHour === 0 || hoursStartHour < 10 ) {
        hoursStartHour = '0' + hoursStartHour
      }
      if ( minutesStartHour === 0 || minutesStartHour < 10 ) {
        minutesStartHour = '0' + minutesStartHour
      }
      if ( hoursEndHour === 0 || hoursEndHour < 10 ) {
        hoursEndHour = '0' + hoursEndHour
      }
      if ( minutesEndHour === 0 || minutesEndHour < 10 ) {
        minutesEndHour = '0' + minutesEndHour
      }
      return hoursStartHour + ':' + minutesStartHour + ' - ' + hoursEndHour + ':' + minutesEndHour
    },
    calcWidth () {
      return this.resolutionWidth / 7
    }
  },
  render () {
    if(!this.provider.context) return;
    const ctx = this.provider.context;

    // Keep a reference to the box used in the previous render call.
    //const oldBox = this.oldBox
    // Calculate the new box. (Computed properties update on-demand.)
    const newBox = this.calculatedBox
    
    let background = '';
    let border = '';
    let color = '';
    let labelBackgroundColor = '';
    let labelFontColor = '';

    if (this.tipo == 'celda') {
      background = '#FFFFFF';
      color = '#000000'; 
      border = '#00000';
      // labelBackgroundColor = "gray";
			// labelFontColor = "white";
    }
    else if (this.tipo == 'cabecera') {
      background = '#FF5000';
      color = '#FFFFFF';  
      border = '#FF5000';  
    } else {
      // labelBackgroundColor = "gray";
			// labelFontColor = "white";
      background = '#FFDCCC';
      color = '#000000';  
      border = '#FFDCCC'; 
    }
    //Dibujando el cuadrado del horario
    ctx.beginPath();
    ctx.rect(this.x, this.y,this.ancho, this.alto);
    ctx.fillStyle = background;
    ctx.fill();
    ctx.closePath();

    ctx.beginPath();
    ctx.rect(this.x, this.y, this.ancho, this.alto);
    ctx.strokeStyle = border;
    ctx.stroke();
    ctx.closePath();

    // ctx.beginPath();
    // ctx.rect(this.x, this.y, this.ancho / 3, this.alto / 3);
    // ctx.strokeStyle = border;
    // ctx.stroke();
    // ctx.closePath();

    // Draw the text
    ctx.fillStyle = color;
    ctx.font = '12px sans-serif';
    ctx.textAlign = 'center';
    ctx.fillText(this.texto, (this.x + (this.ancho / 2)), (this.y + this.alto - 5));//this.y + (this.alto / 2
    // Draw the hours

    //Dibujando el cuadrado de la hora interna
    if ( this.tipo === 'celda') {
      ctx.beginPath();
      ctx.rect(this.x, this.y, 65, 15);
      ctx.fillStyle = '#FFDCCC';
      ctx.fill();
      ctx.background = '#FFDCCC';
      ctx.closePath();
      
      ctx.fillStyle = color;
      ctx.font = '10px sans-serif';
      ctx.textAlign = 'left';
      ctx.fillText(this.hourString, (this.x + 5), (this.y + 10));
      //ctx.fillText(this.texto, (this.x + (this.ancho / 2)), (this.y + this.alto - 5));
    }
    

    // console.log(this.alto, 'alto')
    // console.log(this.ancho, 'ancho')
    // console.log(this.time, 'time')
    // console.log(this.tipo, 'tipo')
    // console.log(this.transmitionDays, 'transmitionDays')
  },
  created() {
    //this.alto = 3508 / (24 * 60)
    //this.ancho = 2480 / 9    
    this.createdCell(this.tipo, this.type, this.days)
    // console.log(this.transmitionDays, 'transmitionDays created')
    // if(this.tipo === 'celda') {
    //   let hoursEndHour = new Date(this.endHour).getHours()// horas fin
    //   let minutesEndHour = new Date(this.endHour).getMinutes()//minutos fin
    //   let hoursStartHour = new Date(this.startHour).getHours()// horas empieza
    //   let minutesStartHour = new Date(this.startHour).getMinutes()//minutos empieza
    //   this.time = ((hoursEndHour * 60) + minutesEndHour) - ((hoursStartHour * 60) + minutesStartHour)
    //   this.alto = this.time * (this.resolutionHigh / (24 * 60))
    //   this.ancho = this.resolutionWidth/7
    //   console.log(this.time, 'fff')
    //   if (this.transmitionDays.length !== 0) {
    //     for(i = 0; i < this.transmitionDays.length; i++){
    //       switch(this.transmitionDays) {
    //       case "Lunes":
    //         this.x = 60;
    //         this.y = (3508 / (24 * 60)) * this.time
    //         break;
    //       case "Martes":
    //         this.x = 335;
    //         this.y = (3508 / (24 * 60)) * this.time
    //         break;
    //       case "Miércoles":
    //         this.x = 610;
    //         this.y = (3508 / (24 * 60)) * this.time
    //         break;
    //       case "Jueves":
    //         this.x = 885;
    //         break;
    //       case "Viernes":
    //         this.x = 1160;
    //         break;
    //       case "Sábado":
    //         this.x = 1435;
    //         break;
    //       case "Domingo":
    //         this.x = 1710;
    //       }
    //   }
      
      
    //   }
    // }    
    // else if(this.tipo === 'hora') {
    //   this.time = ''
    //   this.alto = (this.resolutionHigh / (24 * 60)) * this.timeInterval//36.5416..
    //   this.ancho = 60
    //   this.anchoHora = this.ancho
    // }
    // else if(this.tipo === 'cabecera') {
    //   this.time = ''
    //   this.alto = (this.resolutionHigh / (24 * 60)) * 15
    //   this.ancho = this.resolutionWidth / 7//2480
    // }
    
  }
}
</script>