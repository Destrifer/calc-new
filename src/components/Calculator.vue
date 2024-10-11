<template>
	<div class="calculator">
		<h2>Калькулятор производства календарей</h2>

		<div class="form-group">
			<label for="quantity">Тираж продукции:</label>
			<input type="number" v-model="quantity" min="1" required />
		</div>

		<div class="form-group">
			<label for="calendarType">Тип календаря:</label>
			<select v-model="calendarType" required>
				<option value="mini">Мини</option>
				<option value="midi">Миди</option>
				<option value="maxi">Макси</option>
			</select>
		</div>

		<div class="form-group">
			<label for="blockType">Тип блоков:</label>
			<select v-model="blockType" required>
				<option value="coated">Мелованые</option>
				<option value="offset">Овсетные</option>
				<option value="individual">Индивидуальные</option>
			</select>
		</div>

		<div class="form-group">
			<label for="adFields">Рекламные поля:</label>
			<select v-model="adFields" required>
				<option value="none">Без</option>
				<option value="one">Одно</option>
				<option value="three">Три</option>
			</select>
		</div>

		<div class="form-group">
			<label for="lamination">Ламинация:</label>
			<select v-model="lamination" required>
				<option value="none">Без</option>
				<option value="one-sided">Односторонняя</option>
				<option value="two-sided">Двусторонняя</option>
			</select>
		</div>

		<button @click="calculateCost">Рассчитать</button>

		<div v-if="totalCost !== null" class="result">
			<h3>Итоговая стоимость: {{ totalCost }} ₽</h3>
		</div>
	</div>
</template>

<script>
export default {
	data() {
		return {
			quantity: 1,
			calendarType: 'mini',
			blockType: 'coated',
			adFields: 'none',
			lamination: 'none',
			totalCost: null,
		};
	},
	methods: {
		calculateCost() {
			// Пример расчета (значения для каждого типа можно настроить)
			const basePrice = 100; // Базовая стоимость
			let cost = basePrice * this.quantity;

			// Дополнительные коэффициенты для характеристик
			const calendarTypeFactor = {
				mini: 1,
				midi: 1.2,
				maxi: 1.5
			};
			const blockTypeFactor = {
				coated: 1,
				offset: 1.1,
				individual: 1.3
			};
			const adFieldsFactor = {
				none: 1,
				one: 1.1,
				three: 1.3
			};
			const laminationFactor = {
				none: 1,
				'one-sided': 1.2,
				'two-sided': 1.5
			};

			cost *= calendarTypeFactor[this.calendarType];
			cost *= blockTypeFactor[this.blockType];
			cost *= adFieldsFactor[this.adFields];
			cost *= laminationFactor[this.lamination];
			console.log(this.totalCost);
			this.totalCost = cost.toFixed(2);
		}
	}
};
</script>

<style scoped>
.calculator {
	width: 300px;
	margin: 0 auto;
	padding: 20px;
	border: 1px solid #ccc;
	border-radius: 8px;
	background-color: #000;
}

.form-group {
	margin-bottom: 15px;
}

label {
	display: block;
	margin-bottom: 5px;
}

input,
select {
	width: 100%;
	padding: 8px;
	margin-bottom: 10px;
}

button {
	padding: 10px 15px;
	background-color: #007bff;
	color: white;
	border: none;
	border-radius: 4px;
	cursor: pointer;
}

.result {
	margin-top: 20px;
	font-weight: bold;
}
</style>
