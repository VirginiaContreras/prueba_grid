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
    startHour: {
      type: String,
      default: ''
    },
    endHour: {
      type: String,
      default: ''
    },
    transmitionDays: {
      type: Array,
      default: ''
    }
  },
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
      resolutionWidth: 2480,
      resolutionHigh: 3508,
      highHeadboard: null,
      yPrev: 41,
      altoPrevCell: null,
      yPrevLun: null,
      yPrevMar: null,
      yPrevMie: null,
      yPrevJue: null,
      yPrevVie: null,
      yPrevSab: null,
      yPrevDom: null,
      //x: null,
      //y: null
      //timeInterval: 15,
      //anchoHora: 60
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
      if(tipo === 'celda') {
        let hoursEndHour = new Date(this.endHour).getHours()// horas fin
        let minutesEndHour = new Date(this.endHour).getMinutes()//minutos fin
        let hoursStartHour = new Date(this.startHour).getHours()// horas empieza
        let minutesStartHour = new Date(this.startHour).getMinutes()//minutos empieza
        this.time = ((hoursEndHour * 60) + minutesEndHour) - ((hoursStartHour * 60) + minutesStartHour)
        this.alto = this.time * (this.resolutionHigh / (24 * 60))
        this.altoPrevCell = this.alto
        this.ancho = this.resolutionWidth/7
        console.log(this.time, 'time')
        debugger
        if( hoursStartHour === 5 && minutesStartHour === 16) {
          console.log(this.highHeadboard, 'highHeadboard')
          this.y = this.yPrev
          console.log(this.y, 'y1')
          // this.yPrev = this.y
          console.log(this.yPrev, 'yPrev1')
        } else {
          this.y = this.yPrev + this.altoPrevCell
          console.log(this.yPrev + this.altoPrevCell,'suma')
          console.log(this.y, 'y')        
          console.log(this.altoPrevCell, 'altoPrevCell')   
          this.yPrev = this.y
          console.log(this.yPrev, 'yPrev')
        }
        if ( transmitionDays.length !== 0) {
          switch(transmitionDays[0]) {//transmitionDays[0] indica en que día comienza el horario
            case "Lunes":
              //this.yPrevLun = this.y
              this.x = 0;
              //this.y = this.highHeadboard + 1
              //this.y = (3508 / (24 * 60)) * this.time
              break;
            case "Martes":
              //this.yPrevMar = this.y
              this.x = this.ancho + 1;
              //.y = (3508 / (24 * 60)) * (this.time * 2)
              break;
            case "Miércoles":
              this.x = (this.ancho) * 2 + 1;
              break;
            case "Jueves":
              this.x = (this.ancho) * 3 + 1;
              break;
            case "Viernes":
              this.x = (this.ancho) * 4 + 1;
              break;
            case "Sábado":
              this.x = (this.ancho) * 5 + 1;
              break;
            case "Domingo":
              this.x = (this.ancho) * 6 + 1;
            }   
        }
      } 
      else if( tipo === 'cabecera') {
        this.time = ''
        this.alto = 40 //36.54
        this.highHeadboard = this.alto
        this.ancho = this.resolutionWidth / 7//2480
        this.y = 0
      }
    },    
    calcTime(endHour, startHour) {
      this.time = (new Date(this.endHour).getHours() - new Date(this.startHour).getHours())*60
    },
     calcMinHightToPixel () {
      this.alto = this.resolutionHigh / (24 * 60)
    },
    calcAncho () {
      this.ancho = 2480 / 9
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

    if (this.tipo == 'celda') {
      background = '#FFFFFF';
      color = '#000000'; 
      border = '#00000';
    }
    else if (this.tipo == 'cabecera') {
      background = '#FF5000';
      color = '#FFFFFF';  
      border = '#FF5000';  
    } else {
      background = 'blue';
      color = '#FFFFFF';  
      border = 'blue'; 
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

    //Dibujando el cuadrado de la hora interna
    ctx.beginPath();
    ctx.rect(this.x, this.y,this.ancho / 3, this.alto / 3);
    ctx.fillStyle = background;
    ctx.fill();
    ctx.background = '#333';
    ctx.closePath();

    ctx.beginPath();
    ctx.rect(this.x, this.y, this.ancho / 3, this.alto / 3);
    ctx.strokeStyle = border;
    ctx.stroke();
    ctx.closePath();

    // Draw the text
    ctx.fillStyle = color;
    ctx.font = '10px sans-serif';
    ctx.textAlign = 'center';
    ctx.fillText(this.texto, (this.x + (this.ancho / 2)), (this.y + this.alto - 5));//this.y + (this.alto / 2
    // Draw the hours
    // ctx.fillStyle = color;
    // ctx.font = '8px sans-serif';
    // ctx.textAlign = 'center';
    // ctx.fillText(this.texto, (this.x + (this.ancho / 2)), (this.y + this.alto - 5));

    console.log(this.alto, 'alto')
    console.log(this.ancho, 'ancho')
    console.log(this.time, 'time')
    console.log(this.tipo, 'tipo')
    console.log(this.transmitionDays, 'transmitionDays')
  },
  created() {
    //this.alto = 3508 / (24 * 60)
    //this.ancho = 2480 / 9    
    this.createdCell(this.tipo, this.transmitionDays)
    console.log(this.transmitionDays, 'transmitionDays created')
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