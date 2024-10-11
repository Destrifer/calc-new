<template>
	<div class="stack-visualization">
		<!-- Пример визуализации стопки, можно заменить на 3D или другой тип визуализации -->
		<div class="stack" :style="{ height: calculateStackHeight + 'px' }"></div>
	</div>
</template>

<script>
export default {
	props: ['quantity', 'calendarType'],
	computed: {
		calculateStackHeight() {
			// Пример зависимости высоты от тиража
			const baseHeight = 50; // Минимальная высота стопки
			return baseHeight + (this.quantity / 10000) * 250; // Высота до 300px при максимальном тираже
		}
	}
};
</script>

<style scoped>
.stack-visualization {
	width: 200px;
	height: 300px;
	border: 1px solid #ccc;
	display: flex;
	align-items: flex-end;
	justify-content: center;
	background-color: #fff;
}

.stack {
	width: 100px;
	background-color: #999;
	transition: height 0.3s ease;
}
</style>
