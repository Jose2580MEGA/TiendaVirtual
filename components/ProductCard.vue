<template>
  <div class="product-card">
    <div v-if="product.discount" class="discount-hexagon">
      {{ product.discount }}%
    </div>
    <img v-if="product.image" :src="product.image" alt="Imagen del producto" class="product-image">
    <h3>{{ product.name }}</h3>
    <p class="product-description">{{ product.description }}</p>
    <div v-if="product.discount" class="price-container">
      <span class="original-price">${{ product.price.toFixed(2) }}</span>
      <span class="current-price">${{ discountedPrice.toFixed(2) }}</span>
    </div>
    <div v-else class="price-container">
      <span class="current-price">${{ product.price.toFixed(2) }}</span>
    </div>
    <button @click="$emit('add-to-cart', product)">Añadir al carrito</button>
  </div>
</template>

<script>
export default {
  props: {
    product: {
      type: Object,
      required: true
    }
  },
  computed: {
    discountedPrice() {
      if (this.product.discount) {
        const discountAmount = (this.product.price * this.product.discount) / 100;
        return this.product.price - discountAmount;
      }
      return this.product.price;
    }
  },
  emits: ['add-to-cart']
};
</script>

<style scoped>
.product-card {
  border: 1px solid #ccc;
  padding: 15px;
  margin-bottom: 15px;
  position: relative;
  text-align: center; /* Centrar el contenido de la tarjeta */
}

.discount-hexagon {
  position: absolute;
  top: 10px;
  right: 10px;
  background-color: #f00;
  color: white;
  width: 50px;
  height: 30px;
  line-height: 30px;
  text-align: center;
  font-size: 0.8em;
  clip-path: polygon(50% 0%, 100% 25%, 100% 75%, 50% 100%, 0% 75%, 0% 25%);
}

.product-image {
  max-width: 100%;
  height: auto;
  margin-bottom: 10px;
}

.product-description {
  margin-bottom: 10px;
  font-size: 0.9em;
  color: #555;
}

.price-container {
  margin-bottom: 10px;
}

.original-price {
  text-decoration: line-through;
  color: gray;
  margin-right: 5px;
}

.current-price {
  font-weight: bold;
  color: green;
  font-size: 1.1em;
}

button {
  background-color: #007bff;
  color: white;
  border: none;
  padding: 8px 15px;
  border-radius: 5px;
  cursor: pointer;
  font-size: 1em;
}
</style>