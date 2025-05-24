<template>
  <div v-if="isVisible" class="shopping-cart">
    <h2>Carrito de compras</h2>
    <ul v-if="cartItems.length > 0">
      <li v-for="item in cartItems" :key="item.id" class="cart-item">
        <div class="item-info">
          {{ item.name }} - ${{ calculateItemPrice(item).toFixed(2) }}
          <span v-if="item.quantity > 1"> (x{{ item.quantity }})</span>
        </div>
        <div class="item-controls">
          <button @click="$emit('update-quantity', { productId: item.id, change: -1 })">-</button>
          <span>{{ item.quantity }}</span>
          <button @click="$emit('update-quantity', { productId: item.id, change: 1 })">+</button>
          <button @click="$emit('remove-item', item.id)" class="remove-item-btn">X</button>
        </div>
      </li>
    </ul>
    <p v-else>El carrito está vacío.</p>
    <div class="cart-summary" v-if="cartItems.length > 0">
      <p>Total: ${{ totalPrice.toFixed(2) }}</p>
    </div>
    <div class="cart-actions">
      <button @click="$emit('close-cart')">Cerrar carrito</button>
      <button @click="$emit('checkout')" class="checkout-btn">Comprar</button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    cartItems: {
      type: Array,
      default: () => []
    },
    isVisible: {
      type: Boolean,
      default: false
    }
  },
  emits: ['close-cart', 'update-quantity', 'remove-item', 'checkout'],
  computed: {
    totalPrice() {
      return this.cartItems.reduce((sum, item) => {
        return sum + (this.calculateItemPrice(item) * item.quantity);
      }, 0);
    }
  },
  methods: {
    calculateItemPrice(item) {
      if (item.discount && item.discount > 0) {
        const discountAmount = (item.price * item.discount) / 100;
        return item.price - discountAmount;
      }
      return item.price;
    }
  }
};
</script>

<style scoped>
.shopping-cart {
  position: absolute;
  top: 50px; /* Ajusta la posición vertical según necesites */
  right: 0; /* Cambiado a 0 para que se alinee con el borde del dropdown del carrito */
  background-color: #f9f9f9;
  border: 1px solid #ccc;
  border-radius: 5px;
  padding: 15px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
  z-index: 11; /* Asegúrate de que esté por encima de otros elementos */
  min-width: 280px; /* Aumentado para acomodar los controles */
  text-align: left;
  font-family: "Eras ITC", sans-serif;
}

.shopping-cart h2 {
  margin-top: 0;
  margin-bottom: 10px;
  font-size: 1.2em;
}

.shopping-cart ul {
  list-style: none;
  padding: 0;
  margin-bottom: 10px;
  max-height: 300px; /* Limita la altura del listado del carrito */
  overflow-y: auto; /* Agrega scroll si hay muchos ítems */
}

.shopping-cart li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 8px 0;
  border-bottom: 1px solid #eee;
}

.shopping-cart li:last-child {
  border-bottom: none;
}

.item-info {
  flex-grow: 1;
}

.item-controls {
  display: flex;
  align-items: center;
  gap: 5px;
}

.item-controls button {
  padding: 4px 8px;
  font-size: 0.8em;
  min-width: 25px; /* Asegura un tamaño mínimo para los botones de cantidad */
  text-align: center;
}

.remove-item-btn {
  background-color: #dc3545; /* Rojo para eliminar */
  color: white;
  border: none;
}

.remove-item-btn:hover {
  background-color: #c82333;
}

.cart-summary {
  margin-top: 15px;
  padding-top: 10px;
  border-top: 1px solid #eee;
  text-align: right;
  font-weight: bold;
}

.cart-actions {
  display: flex;
  justify-content: space-between;
  margin-top: 15px;
  gap: 10px;
}

.shopping-cart button {
  padding: 8px 12px;
  background-color: black;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 0.9em;
  flex-grow: 1; /* Permite que los botones crezcan y llenen el espacio */
}

.shopping-cart button:hover {
  background-color: magenta;
}

.checkout-btn {
  background-color: #28a745; /* Un verde para el botón de comprar */
}

.checkout-btn:hover {
  background-color: #218838;
}

button {
  font-family: "Eras ITC", sans-serif;
}
</style>