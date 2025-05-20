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
  emits: ['add-to-cart'], // Se recomienda declarar los emits explícitamente
  computed: {
    discountedPrice() {
      if (this.product.discount) {
        const discountAmount = (this.product.price * this.product.discount) / 100;
        return this.product.price - discountAmount;
      }
      return this.product.price;
    }
  }
};
</script>

<style scoped>
.product-card {
  border: 1px solid #ccc;
  padding: 15px;
  margin-bottom: 15px;
  position: relative;
  text-align: center; /* Centrar el contenido de la tarjeta */
  border-radius: 8px; /* Pequeño ajuste para mejor estética */
  box-shadow: 0 2px 5px rgba(0,0,0,0.05); /* Sutil sombra */
}

.discount-hexagon {
  position: absolute;
  top: 10px;
  right: 10px;
  background-color: #f00;
  color: white;
  width: 40px; /* Aumentado el tamaño */
  height: 40px; /* Aumentado el tamaño */
  line-height: 40px; /* Ajuste para centrar texto verticalmente */
  text-align: center;
  font-size: 0.8em;
  clip-path: polygon(50% 0%, 100% 25%, 100% 75%, 50% 100%, 0% 75%, 0% 25%);
  display: flex;
  justify-content: center;
  align-items: center;
}

.product-image {
  max-width: 100%;
  height: 200px; /* Altura fija para las imágenes */
  object-fit: contain; /* Asegura que la imagen se ajuste sin distorsionarse */
  margin-bottom: 10px;
}

h3 {
  font-size: 1.3em;
  margin-top: 0;
  margin-bottom: 10px;
  font-family: "Eras ITC", sans-serif;
}

.product-description {
  margin-bottom: 10px;
  font-size: 0.9em;
  color: #555;
  height: 60px; /* Altura fija para la descripción */
  overflow: hidden; /* Oculta el texto que desborda */
  text-overflow: ellipsis; /* Añade puntos suspensivos si el texto es muy largo */
}

.price-container {
  margin-bottom: 15px;
}

.original-price {
  text-decoration: line-through;
  color: gray;
  margin-right: 8px; /* Ajustado el margen */
  font-size: 1em;
}

.current-price {
  font-weight: bold;
  color: #28a745; /* Un verde un poco más agradable */
  font-size: 1.4em; /* Precio más grande */
}

button {
  background-color: #007bff;
  color: white;
  border: none;
  padding: 10px 20px; /* Padding ajustado */
  border-radius: 5px;
  cursor: pointer;
  font-size: 1em;
  transition: background-color 0.3s ease; /* Transición suave al pasar el ratón */
}

button:hover {
  background-color: #0056b3;
}
</style>