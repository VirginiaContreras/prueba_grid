<script>

const percentWidthToPix = (percent, ctx) => Math.floor((ctx.canvas.width / 100) * percent)
const percentHeightToPix = (percent, ctx) => Math.floor((ctx.canvas.height / 100) * percent)

export default {
  inject: ['provider'],
  props: {
    x: {
      type: Number,
      default: 0
    },
    y: {
      type: Number,
      default: 0
    },
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
  // methods: {    
  //   calcTime(endHour, startHour) {
  //     this.time = (new Date(this.endHour).getHours() - new Date(this.startHour).getHours())*60
  //   },
  //    calcMinAlto () {
  //     this.alto = 3508 / (24 * 60)
  //   },
  //   calcAncho () {
  //     this.ancho = 2480 / 9
  //   }
  // },
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
    ctx.background = 'blue';
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

    console.log(this.alto)
    console.log(this.ancho)
    console.log(this.time)
  },
  created() {
    //this.alto = 3508 / (24 * 60)
    //this.ancho = 2480 / 9
    
    if(this.tipo === 'celda') {
      let hoursEndHour = new Date(this.endHour).getHours()// horas fin
      let minutesEndHour = new Date(this.endHour).getMinutes()//minutos fin
      let hoursStartHour = new Date(this.startHour).getHours()// horas empieza
      let minutesStartHour = new Date(this.startHour).getMinutes()//minutos empieza
      this.time = ((hoursEndHour * 60) + minutesEndHour) - ((hoursStartHour * 60) + minutesStartHour)
      this.alto = this.time * (this.resolutionHigh / (24 * 60))
      this.ancho = this.resolutionWidth/7
      console.log(this.time, 'fff')
    }    
    // else if(this.tipo === 'hora') {
    //   this.time = ''
    //   this.alto = (this.resolutionHigh / (24 * 60)) * this.timeInterval//36.5416..
    //   this.ancho = 60
    //   this.anchoHora = this.ancho
    // }
    else if(this.tipo === 'cabecera') {
      this.time = ''
      this.alto = (this.resolutionHigh / (24 * 60)) * 15
      this.ancho = this.resolutionWidth / 7//2480
    }
    
  }
}
</script>