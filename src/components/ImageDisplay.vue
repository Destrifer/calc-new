<template>
  <div class="image-display">
    <img :src="imageUrl" alt="Calendar Preview" v-if="imageUrl" />
  </div>
</template>

<script>
export default {
  props: {
    imageUrl: {
      type: String,
      required: true
    }
  }
};
</script>

<style scoped>
.image-display {
  margin-top: 20px;
}

.image-display img {
  max-width: 100%;
  height: auto;
}
</style>
