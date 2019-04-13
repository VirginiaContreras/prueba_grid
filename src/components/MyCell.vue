<script>

const percentWidthToPix = (percent, ctx) => Math.floor((ctx.canvas.width / 100) * percent)
const percentHeightToPix = (percent, ctx) => Math.floor((ctx.canvas.height / 100) * percent)
const percentTopToPix = (percent, ctx) => Math.floor((ctx.canvas.offsetX / 100) * percent)
const percentLeftToPix = (percent, ctx) => Math.floor((ctx.canvas.offsetY / 100) * percent)
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
    /*days: {
      type: Array,
      default: ''
    },*/
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
      hourString: '',
      offsetX: 0,
      offsetY: 0,
      active: false,
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
        h: percentHeightToPix(this.alto, ctx),
        offsetX: percentTopToPix(this.offsetX,ctx),
        offsety: percentTopToPix(this.offsetY,ctx)
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

        let startTimestamp = new Date()
        startTimestamp.setHours(hoursStartHour, minutesStartHour, 0)

        let limitTimestamp = new Date()
        limitTimestamp.setHours(5, 16, 0)

        if ( startTimestamp < limitTimestamp) {
          this.y = (((24 * 60) + startTime ) * this.calcMinHightToPixel()) - 632 
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
        // if( hoursStartHour === 5 && minutesStartHour === 16) {//Calculando el y por tiempo en pixeles
        //   //Calculando la primera celda
        //   //this.y = this.yPrev
        //   // console.log(this.y, 'y1')
        //   // console.log(this.yPrev, 'yPrev1')
        //   localStorage.setItem("yPrev", this.yPrev);
        //   localStorage.setItem("altoPrev", this.alto);
        // }           
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
    },
    reOffset () {
      let offsetX = this.offsetX;
      let offsetY = this.offsetY;  
      console.log(this.offsetX,'left')
      console.log(this.offsetY,'top')
      console.log(offsetX,'offsetX')
      console.log(offsetY,'offsetY')
      
    },
    mouseOver () {
      if (this.alto <= 22) {
        this.active = !this.active; 
        // e.preventDefault();
        // e.stopPropagation();
      }
        
    }
  },
  render () {
    if(!this.provider.context) return;
    const ctx = this.provider.context;
    //console.log(ctx.canvas.offsetY, 'canvas top' )
    //console.log(ctx.canvas.offsetX, 'canvas left' )
    //console.log(ctx.canvas.width, 'canvas width' )
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
    }
    else if (this.tipo == 'cabecera') {
      background = '#FF5000';
      color = '#FFFFFF';  
      border = '#FF5000';  
    } else {
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
    if (this.alto > 22 && this.active === false) {

      let textWidth = this.texto.length //40 letras maximo por columna
      let countColumns = this.transmitionDays.length

      if(textWidth < (40 * countColumns)){
        ctx.fillStyle = color;
        ctx.font = '12px sans-serif';
        ctx.textAlign = 'center';
        ctx.fillText(this.texto, (this.x + (this.ancho / 2)), (this.y + this.alto - 5));
      }
      else{        
        let arrofwords = this.texto.split(' '); //separar texto por espacios
        let middle = arrofwords.length / 2; //contar la mitad de las palabras
        arrofwords.splice(middle, 0, '\n'); //añadir salto de linea a la mitad
        let textoFormat = arrofwords.join(''); //volver a unir

        let lines = textoFormat.split('\n'); //separar por salto de linea
        let lineheight = 15; //separacion de lineas

        //dibujar
        ctx.fillStyle = color;
        ctx.font = '12px sans-serif';
        ctx.textAlign = 'center';

        //dibujar cada linea
        for (let i = 0; i<lines.length; i++){
          ctx.fillText(lines[i], (this.x + (this.ancho / 2)), ((this.y + this.alto - 5) - 20) + (i * lineheight) );
        }
      }
      
    } 
    else {
      // e.preventDefault();
      // e.stopPropagation();
      /*ctx.beginPath();
      ctx.rect(this.x, this.y, this.ancho, this.alto);
      ctx.fillStyle = 'blue';
      ctx.fill();
      ctx.background = 'blue';
      ctx.closePath();*/

      let myX = this.x
      let myY = this.y
      let myAncho = this.ancho
      let myAlto = this.alto
      let myTexto = this.texto
      //let myTooltip = document.getElementById('tooltip')
      //let myTitle = this.title
      
      ctx.canvas.addEventListener('mousemove', function (event) {
        //Prueba con div
        //ctx.canvas.parentNode.childNodes[1].style.position  = 'relative'

        //ctx.canvas.parentNode.childNodes[1].innerHTML = 'avisooooooooooo'
        // ctx.canvas.parentNode.childNodes[1].childNodes[0].innerHTML = 'avisooooooooooo'
        // ctx.canvas.parentNode.childNodes[1].childNodes[0].style.color = 'white'
        //ctx.canvas.parentNode.childNodes[1].childNodes[0].style.backgroundColor  = 'yellow' 
        /*ctx.canvas.parentNode.childNodes[1].childNodes[0].tabIndex  = '9999'    
        ctx.canvas.parentNode.childNodes[1].childNodes[0].width = 200
        ctx.canvas.parentNode.childNodes[1].childNodes[0].height = 50 
        ctx.canvas.parentNode.childNodes[1].childNodes[0].style.visibility = 'visible'
        ctx.canvas.parentNode.childNodes[1].childNodes[0].style.zIndex  = '9999'*/

        console.log(ctx.canvas.parentNode.childNodes[1].childNodes[0].innerHTML = 'aviso')

        let rectangle = new Path2D()
        rectangle.rect(myX, myY, myAncho, myAlto)
        ctx.stroke(rectangle)
        console.log(myAncho,'myAncho')
        console.log(myAlto,'myAlto')
        let r = this.getBoundingClientRect()
        let coordX = event.clientX - r.left
        let coordY = event.clientY - r.top

        //preparo el rectangulo
        ctx.beginPath();
        ctx.rect(myX, myY, myAncho, myAlto);

        if (ctx.isPointInPath(rectangle, coordX, coordY)) {
          console.log('dentro')          
          ctx.fillStyle = 'blue'; //color de relleno si estoy dentro

          //dibujar tooltip
          //tooltip con div
          //contenedor
          ctx.canvas.parentNode.childNodes[1].style.position  = 'relative'
          ctx.canvas.parentNode.childNodes[1].style.top  = '${coordX}'
          ctx.canvas.parentNode.childNodes[1].style.left  = coordY
          //tooltip
          ctx.canvas.parentNode.childNodes[1].childNodes[0].innerHTML = myTexto
          ctx.canvas.parentNode.childNodes[1].childNodes[0].style.width = '800'
          ctx.canvas.parentNode.childNodes[1].childNodes[0].style.width = '80'
          ctx.canvas.parentNode.childNodes[1].childNodes[0].style.color = 'white'
          
          // let rectangle = new Path2D()
          // rectangle.rect(coordX - 400, coordY - 85, 800, 80)
          // ctx.canvas.zIndex = -999
          // ctx.stroke(rectangle)
          
          // //dibujar texto
          // ctx.fillStyle = 'blue';
          // ctx.font = '12px sans-serif';
          // ctx.textAlign = 'center';
          // ctx.fillText(myTexto, (coordX), (coordY - 10));
          // ctx.fillText(myTexto, (myX + (myAncho / 2)), (myY + myAlto - 5));
        }
        else{
          //console.log('fuera')
          ctx.fillStyle = 'white'; //color de relleno si estoy fuera
          //ctx.clearRect(coordX - 400,coordY - 80,800,80);
        }

        //dibujo rectangulo
        ctx.fill();
        ctx.closePath();
        
           
        /*ctx.beginPath();
        ctx.rect(this.x, this.y, this.ancho, this.alto);
        ctx.fillStyle = ctx.isPointInPath(this.x, this.y) ? "blue":"yellow";
        ctx.fill();

        ctx.fillStyle = color;
        ctx.font = '12px sans-serif';
        ctx.textAlign = 'center';
        ctx.fillText(this.texto, (this.x + (this.ancho / 2)), (this.y + this.alto - 5));*/

      }, false)

    }

    // Draw the hours
    //Dibujando el cuadrado de la hora interna
    if ( this.tipo === 'celda' && this.alto > 22) {
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
    this.createdCell(this.tipo, this.type, this.days)    
    // this.reOffset();
    // window.onscroll = (e) => { this.reOffset() }
    // window.onresize = (e) => { this.reOffset() }
    // this.provider.context.onmousemove((e) => {handleMouseMove(e);});
    // function handleMouseMove(e){
    //   // tell the browser we're handling this event
    //   e.preventDefault();
    //   e.stopPropagation();
    //   alert(this.left + this.top + 'probando')
    //   // mouseX=parseInt(e.clientX-offsetX);
    //   // mouseY=parseInt(e.clientY-offsetY);

    //   // //ctx.clearRect(0,0,cw,ch);
    //   // //draw();
    //   // for(let i=0;i<hotspots.length;i++){
    //   //   let h=hotspots[i];
    //   //   let dx=mouseX-h.x;
    //   //   let dy=mouseY-h.y;
    //   //   if(dx*dx+dy*dy<h.radius*h.radius){
    //   //     ctx.fillText(h.tip,h.x,h.y);
    //   //   }
    //   // }

    // }
  }
}
</script>