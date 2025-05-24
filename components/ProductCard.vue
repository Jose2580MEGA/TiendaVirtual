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

    <div class="add-to-cart-controls">
      <label for="quantity-{{ product.id }}">Cantidad:</label>
      <input
        type="number"
        :id="'quantity-' + product.id"
        v-model.number="quantityToAdd"
        min="1"
        @input="validateQuantity"
      />
      <button @click="addToCartWithQuantity">Añadir al carrito</button>
    </div>
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
  data() {
    return {
      quantityToAdd: 1 // Cantidad por defecto para añadir al carrito
    };
  },
  emits: ['add-to-cart'],
  computed: {
    discountedPrice() {
      if (this.product.discount && this.product.discount > 0) {
        const discountAmount = (this.product.price * this.product.discount) / 100;
        return this.product.price - discountAmount;
      }
      return this.product.price;
    }
  },
  methods: {
    addToCartWithQuantity() {
      // Asegurarse de que la cantidad sea al menos 1
      if (this.quantityToAdd < 1) {
        this.quantityToAdd = 1;
      }
      this.$emit('add-to-cart', this.product, this.quantityToAdd);
      this.quantityToAdd = 1; // Resetear la cantidad a 1 después de añadir
    },
    validateQuantity() {
      // Asegurarse de que la cantidad no sea menor que 1
      if (this.quantityToAdd < 1) {
        this.quantityToAdd = 1;
      }
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
  background-color: red;
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
  color: rgb(0, 215, 0); /* Un verde un poco más agradable */
  font-size: 1.4em; /* Precio más grande */
}

.add-to-cart-controls {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  margin-top: 15px;
}

.add-to-cart-controls label {
  font-size: 0.9em;
}

.add-to-cart-controls input[type="number"] {
  width: 60px;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
  text-align: center;
}

button {
  background-color: blueviolet;
  font-family: "Eras ITC", sans-serif;
  color: white;
  border: none;
  padding: 10px 20px; /* Padding ajustado */
  border-radius: 5px;
  cursor: pointer;
  font-size: 1em;
  transition: background-color 0.3s ease; /* Transición suave al pasar el ratón */
}

button:hover {
  background-color: purple;
}
</style>