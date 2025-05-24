<template>
  <div v-if="isVisible" class="shopping-cart">
    <h2>Carrito de compras</h2>
    <ul v-if="cartItems.length > 0">
      <li v-for="item in cartItems" :key="item.id">
        {{ item.name }} - ${{ item.price.toFixed(2) }}
      </li>
    </ul>
    <p v-else>El carrito está vacío.</p>
    <button @click="$emit('close-cart')">Cerrar carrito</button>
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
  emits: ['close-cart']
};
</script>

<style scoped>
.shopping-cart {
  position: absolute;
  top: 50px; /* Ajusta la posición vertical según necesites */
  right: 20px;
  background-color: #f9f9f9;
  border: 1px solid #ccc;
  border-radius: 5px;
  padding: 15px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
  z-index: 11; /* Asegúrate de que esté por encima de otros elementos */
  min-width: 200px;
  text-align: left;
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
}

.shopping-cart li {
  padding: 8px 0;
  border-bottom: 1px solid #eee;
}

.shopping-cart li:last-child {
  border-bottom: none;
}

.shopping-cart p {
  margin-bottom: 10px;
}

.shopping-cart button {
  padding: 8px 12px;
  background-color: black;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 0.9em;
}

.shopping-cart button:hover {
  background-color: magenta;
}

button{
  font-family: "Eras ITC", sans-serif;
}
</style>