<template>
  <div class="calculator">
    <h2>Калькулятор производства календарей</h2>

    <div class="form-group">
      <label for="quantity">Тираж продукции:</label>
      <input
        type="range"
        v-model="quantity"
        :min="minQuantity"
        :max="maxQuantity"
        @input="snapToNearest"
        step="1"
      />
      <p>{{ quantity }} шт.</p>
    </div>

    <div class="form-group">
      <label for="calendarType">Тип календаря:</label>
      <select v-model="calendarType" @change="updateImage" required>
        <option value="mini">Мини</option>
        <option value="midi">Миди</option>
        <option value="maxi">Макси</option>
      </select>
    </div>

    <div class="form-group">
      <label for="blockType">Тип блоков:</label>
      <select v-model="blockType" @change="updateImage" required>
        <option value="coated">Мелованые</option>
        <option value="offset">Овсетные</option>
        <option value="individual">Индивидуальные</option>
      </select>
    </div>

    <div class="form-group">
      <label for="adFields">Рекламные поля:</label>
      <select v-model="adFields" @change="updateImage" required>
        <option value="none">Без</option>
        <option value="one">Одно</option>
        <option value="three">Три</option>
      </select>
    </div>

    <div class="form-group">
      <label for="lamination">Ламинация:</label>
      <select v-model="lamination" @change="updateImage" required>
        <option value="none">Без</option>
        <option value="one-sided">Односторонняя</option>
        <option value="two-sided">Двусторонняя</option>
      </select>
    </div>

    <button @click="calculateCost">Рассчитать</button>

    <div v-if="totalCost !== null" class="result">
      <h3>Итоговая стоимость: {{ totalCost }} ₽</h3>
    </div>

    <ImageDisplay :imageUrl="currentImage" />
  </div>
</template>

<script>
import ImageDisplay from './ImageDisplay.vue';

export default {
  components: {
    ImageDisplay
  },
  data() {
    return {
      quantity: 1,
      calendarType: 'mini',
      blockType: 'coated',
      adFields: 'none',
      lamination: 'none',
      totalCost: null,
      minQuantity: 1,
      maxQuantity: 1000,
      currentImage: '', // URL для текущего изображения
      snapValues: [1, 5, 10, 20, 50, 100, 200, 500, 1000], // "прилипающие" значения
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
      this.totalCost = cost.toFixed(2);
    },
    snapToNearest() {
      // "Прилипание" ползунка к ближайшему значению
      const nearestValue = this.snapValues.reduce((prev, curr) =>
        (Math.abs(curr - this.quantity) < Math.abs(prev - this.quantity) ? curr : prev), this.snapValues[0]);
      this.quantity = nearestValue;
      this.calculateCost(); // Пересчитать стоимость при изменении
    },
    updateImage() {
      // Обновление изображения в зависимости от выбранных параметров
      this.currentImage = this.getImageUrl();
    },
    getImageUrl() {
      // Логика для возврата URL изображения в зависимости от выбранных параметров
      const images = {
        calendarType: {
          mini: '/images/mini_calendar.jpg',
          midi: '/images/midi_calendar.jpg',
          maxi: '/images/maxi_calendar.jpg',
        },
        blockType: {
          coated: '/images/coated_block.jpg',
          offset: '/images/offset_block.jpg',
          individual: '/images/individual_block.jpg',
        },
        adFields: {
          none: '/images/no_ad_fields.jpg',
          one: '/images/one_ad_field.jpg',
          three: '/images/three_ad_fields.jpg',
        },
        lamination: {
          none: '/images/no_lamination.jpg',
          'one-sided': '/images/one_sided_lamination.jpg',
          'two-sided': '/images/two_sided_lamination.jpg',
        }
      };

      // Выбор изображения по типу календаря
      const calendarImage = images.calendarType[this.calendarType];
      return calendarImage; // Вернуть URL для выбранного типа календаря
    }
  },
  mounted() {
    this.updateImage(); // Установить изображение при монтировании компонента
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

input[type="range"] {
  width: 100%;
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
