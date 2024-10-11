<template>
  <div class="calculator">
    <div class="options">
      <label for="tirazh">Тираж: {{ tirazh }}</label>
      <input
        type="range"
        id="tirazh"
        v-model="tirazh"
        min="100"
        max="10000"
        step="100"
        @input="updateStack"
      />
    </div>
    <div class="stack-demo" ref="sceneContainer"></div>
  </div>
</template>

<script>
import * as THREE from 'three';

export default {
  data() {
    return {
      tirazh: 1000, // Это можно оставить реактивным
    };
  },
  mounted() {
    // Инициализация Three.js после монтирования компонента
    this.initThree();
  },
  methods: {
    initThree() {
      // Размеры для рендера
      const width = this.$refs.sceneContainer.clientWidth;
      const height = 300; // Высота контейнера

      // Создаем сцену (не реактивно)
      this.scene = new THREE.Scene();

      // Создаем камеру (не реактивно)
      this.camera = new THREE.PerspectiveCamera(75, width / height, 0.1, 1000);
      this.camera.position.z = 200; // Позиционирование камеры

      // Создаем рендерер (не реактивно)
      this.renderer = new THREE.WebGLRenderer();
      this.renderer.setSize(width, height);
      this.$refs.sceneContainer.appendChild(this.renderer.domElement);

      // Добавляем свет
      const light = new THREE.DirectionalLight(0xffffff, 1);
      light.position.set(1, 1, 1).normalize();
      this.scene.add(light);

      // Создаем начальную стопку
      this.createStack();

      // Начинаем анимацию
      this.animate();
    },
    createStack() {
      // Создаем геометрию стопки
      const geometry = new THREE.BoxGeometry(50, this.calculateStackHeight(), 50);
      const material = new THREE.MeshPhongMaterial({ color: 0x888888 });

      // Если стопка уже существует — удаляем её
      if (this.stack) {
        this.scene.remove(this.stack);
      }

      // Создаем новую стопку (не реактивно)
      this.stack = new THREE.Mesh(geometry, material);
      this.scene.add(this.stack);
    },
    updateStack() {
      // Обновляем стопку при изменении тиража
      this.createStack();
    },
    calculateStackHeight() {
      // Примерная зависимость высоты от тиража
      return (this.tirazh / 10000) * 200; // Высота до 200px при максимальном тираже
    },
    animate() {
      // Используем замыкание для requestAnimationFrame, чтобы избежать потери контекста
      requestAnimationFrame(() => this.animate());

      // Поворот стопки для визуального эффекта
      if (this.stack) {
        this.stack.rotation.y += 0.01;
      }

      // Рендерим сцену с камерой
      this.renderer.render(this.scene, this.camera);
    },
  },
};
</script>

<style scoped>
.calculator {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.options {
  width: 30%;
}

.stack-demo {
  width: 60%;
  height: 300px; /* Высота контейнера для рендера */
  border: 1px solid #ccc;
}
</style>
