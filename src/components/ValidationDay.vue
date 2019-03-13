<template>
	<!-- <h1>{{text}}</h1> -->
	<my-cell
		:tipo="schedule.tipo"
		:texto="schedule.texto"
		:scheduleStartHour="schedule.scheduleStartHour"
		:scheduleStartMin="schedule.scheduleStartMin"
		:scheduleEndHour="schedule.scheduleEndHour"
		:scheduleEndMin="schedule.scheduleEndMin"
		:transmitionDays="schedule.transmitionDays"
		:type="type"
		:days="transmitionDays"
	>
	</my-cell>
</template>

<script>
import MyCell from './MyCell.vue';
export default {
	props: ['schedule'],
	components: {
		MyCell
	},
	data() {
		return {
			transmitionDays: [],
			type: ''
		}
	},
	methods: {
		consecutiveNumbers(elemento) {
			
		}, 
		validationDays(tipo, transmitionDays) {
			let daysNumber = []
			//Creando nuevo array daysNumber para evaluar si los días son consecutivos asignandoles un valor numérico a los transmitionDays
			for (let i=0; i < transmitionDays.length; i++) {
				if ( this.schedule.transmitionDays[i].includes('Lunes') ) {
					daysNumber.push(1)
				}
				else if ( this.schedule.transmitionDays[i].includes('Martes') ) {
					daysNumber.push(2)
				}
				else if ( this.schedule.transmitionDays[i].includes('Miércoles') ) {
					daysNumber.push(3)
				}
				else if ( this.schedule.transmitionDays[i].includes('Jueves') ) {
					daysNumber.push(4)
				}
				else if ( this.schedule.transmitionDays[i].includes('Viernes') ) {
					daysNumber.push(5)
				}
				else if ( this.schedule.transmitionDays[i].includes('Sábado') ) {
					daysNumber.push(6)
				}
				else if ( this.schedule.transmitionDays[i].includes('Domingo') ) {
					daysNumber.push(7)
				}
			}
			//Evaluando si son consecutivos
			if ( this.schedule.transmitionDays.length === 1 ) {
				this.type = tipo
				this.transmitionDays = this.schedule.transmitionDays
				// console.log(this.type, 'type')
      	// console.log(this.transmitionDays, 'days')
				// this.createdCell(tipo, this.schedule.transmitionDays)
			}
			else {
				let arrayDaysCell = []
				let arrayDaysCellGroup = []
				let arrayDaysCellUniques = []
				let cacthValue = false
				let i = 0
				// while(daysNumber[i] === (daysNumber[i + 1] - 1)) {
				// 	cacthValue = true
				// 	arrayDaysCell.push(this.schedule.transmitionDays[i])
				// 	arrayDaysCell.push(this.schedule.transmitionDays[i + 1])
				// 	arrayDaysCellUniques = [[...new Set(arrayDaysCell)]]
				// }
				// arrayDaysCellGroup.push(arrayDaysCellUniques)
				// cacthValue = false
				
				let a = ''
				console.log(daysNumber);
				for (let i = 0; i < daysNumber.length; i++) {
					
					if ( daysNumber[i] === (daysNumber[i + 1] - 1) ) {						
						//arrayDaysCell.push(this.schedule.transmitionDays[i])
						//arrayDaysCell.push(this.schedule.transmitionDays[i + 1])
						//arrayDaysCellUniques = [[...new Set(arrayDaysCell)]]
						if(a === '' || a.charAt(a.length-1) === '-'){
							a += daysNumber[i]
						}							
						a += daysNumber[i + 1]													 					
					}
					else {
						if(a === '' || a.charAt(a.length-1) === '-'){
							a += daysNumber[i]
						}
						a += '-'
					}
					
					/*arrayDaysCellGroup[0] = arrayDaysCellUniques
					for (let i=0; i < arrayDaysCellGroup.length; i++) {
						this.transmitionDays = arrayDaysCellGroup[i]
						this.type = tipo
					}*/
					//console.log(arrayDaysCellGroup, 'arrayDaysCellGroup')
					//console.log(arrayDaysCellGroup.length, 'arrayDaysCellGroup length')
					// arrayDaysCellUniques = [...new Set(arrayDaysCell)]
					
					// this.transmitionDays = arrayDaysCellUniques
					//console.log(this.type, 'type')
					//console.log(this.transmitionDays, 'days')
					//console.log(arrayDaysCell, 'arrayDaysCell')
					//console.log(arrayDaysCellUniques, 'arrayDaysCellUniques')
					//console.log(arrayDaysCellUniques.length, 'arrayDaysCellUniques length')
					// this.createdCell(tipo, arrayDaysCell)
				}

				a = a.substr(0, a.length-1)
				console.log(a);
				
			}
			
			// console.log('transmitionDays')
			// console.log(this.schedule.transmitionDays)
			// console.log('daysNumber')
			// console.log(daysNumber)
			// console.log(daysNumber.length)
		}
	},
	created() {
		this.validationDays(this.schedule.tipo, this.schedule.transmitionDays)
		//console.log(this.schedule.transmitionDays,'hhh')
	}
}
</script>
