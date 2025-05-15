<template>
  <div>
    <h1>Tienda Virtual</h1>

    <div class="top-right-menu">
      <div class="dropdown">
        <button @click="toggleAdminMenu" class="dropbtn">Administrar Productos</button>
        <div v-if="isAdminMenuOpen" class="dropdown-content">
          <button @click="showAddProductForm" class="admin-button">Añadir Producto</button>
          <button @click="showEditDeleteList" class="admin-button">Editar Productos</button>

          <div v-if="isAddProductFormVisible">
            <h2>Añadir Producto</h2>
            <div>
              <label for="name">Nombre:</label>
              <input type="text" id="name" v-model="newProduct.name">
            </div>
            <div>
              <label for="description">Descripción:</label>
              <textarea id="description" v-model="newProduct.description"></textarea>
            </div>
            <div>
              <label for="price">Precio:</label>
              <input type="number" id="price" v-model.number="newProduct.price">
            </div>
            <div>
              <label for="discount">Descuento (%):</label>
              <input type="number" id="discount" v-model.number="newProduct.discount">
            </div>
            <div>
              <label for="image">Imagen URL:</label>
              <input type="text" id="image" v-model="newProduct.image">
            </div>
            <button @click="addProduct">Añadir</button>
          </div>

          <div v-if="isEditDeleteListVisible" class="edit-delete-list-container">
            <h2>Editar Productos</h2>
            <ul class="edit-delete-list">
              <li v-for="(product, index) in products" :key="product.id">
                <span>{{ product.name }}</span>
                <button @click="startEdit(product)">Editar</button>
                <button class="delete-button" @click="deleteProduct(product.id)">Eliminar</button>
              </li>
            </ul>
          </div>
        </div>
      </div>

      <div class="dropdown">
        <button @click="toggleCart" class="dropbtn">
          🛒 ({{ cart.length }})
        </button>
        <ShoppingCart
          :cartItems="cart"
          :isVisible="isCartVisible"
          @close-cart="toggleCart"
        />
      </div>
    </div>

    <div class="product-list">
      <ProductCard
        v-for="product in products"
        :key="product.id"
        :product="product"
      />
    </div>

    <dialog ref="editDialog">
      <form @submit.prevent="saveEdit">
        <h2>Editar Producto</h2>
        <div>
          <label for="edit-name">Nombre:</label>
          <input type="text" id="edit-name" v-model="editingProduct.name">
        </div>
        <div>
          <label for="edit-description">Descripción:</label>
          <textarea id="edit-description" v-model="editingProduct.description"></textarea>
        </div>
        <div>
          <label for="edit-price">Precio:</label>
          <input type="number" id="edit-price" v-model.number="editingProduct.price">
        </div>
        <div>
          <label for="edit-discount">Descuento (%):</label>
          <input type="number" id="edit-discount" v-model.number="editingProduct.discount">
        </div>
        <div>
          <label for="edit-image">Imagen URL:</label>
          <input type="text" id="edit-image" v-model="editingProduct.image">
        </div>
        <button type="submit">Guardar</button>
        <button type="button" @click="cancelEdit">Cancelar</button>
      </form>
    </dialog>
  </div>
</template>

<script>
import ProductCard from '@/components/ProductCard.vue';
import ShoppingCart from '@/components/ShoppingCart.vue';

export default {
  components: {
    ProductCard,
    ShoppingCart
  },
  data() {
    return {
      products: [],
      newProduct: {
        name: '',
        description: '',
        price: null,
        discount: null,
        image: ''
      },
      editingProduct: {
        id: null,
        name: '',
        description: '',
        price: null,
        discount: null,
        image: ''
      },
      cart: [],
      isCartVisible: false,
      isAdminMenuOpen: false,
      isAddProductFormVisible: false,
      isEditDeleteListVisible: false
    };
  },
  methods: {
    showAddProductForm() {
      this.isAddProductFormVisible = true;
      this.isEditDeleteListVisible = false;
    },
    showEditDeleteList() {
      this.isAddProductFormVisible = false;
      this.isEditDeleteListVisible = true;
    },
    addProduct() {
      if (this.newProduct.name && this.newProduct.price !== null) {
        this.products.push({
          id: Date.now(),
          ...this.newProduct
        });
        this.newProduct = { name: '', description: '', price: null, discount: null, image: '' };
        this.isAddProductFormVisible = false;
      }
    },
    startEdit(product) {
      this.editingProduct = { ...product };
      this.$refs.editDialog.showModal();
    },
    saveEdit() {
      const index = this.products.findIndex(p => p.id === this.editingProduct.id);
      if (index !== -1) {
        this.products.splice(index, 1, { ...this.editingProduct });
      }
      this.$refs.editDialog.close();
      this.editingProduct = { id: null, name: '', description: '', price: null, discount: null, image: '' };
    },
    cancelEdit() {
      this.$refs.editDialog.close();
      this.editingProduct = { id: null, name: '', description: null, price: null, discount: null, image: '' };
    },
    deleteProduct(id) {
      this.products = this.products.filter(product => product.id !== id);
    },
    addToCart(product) {
      console.log('Función addToCart ejecutada', product)
      console.log('Producto añadido al carrito:', product);
      this.cart.push(product);
      console.log('Contenido del carrito:', this.cart);
      console.log('Carrito actual:', this.cart);
    },
    toggleCart() {
      this.isCartVisible = !this.isCartVisible;
    },
    toggleAdminMenu() {
      this.isAdminMenuOpen = !this.isAdminMenuOpen;
      this.isAddProductFormVisible = false;
      this.isEditDeleteListVisible = false;
    }
  }
};
</script>

<style scoped>
h1 {
  text-align: center;
  margin-bottom: 20px;
  font-family: "Eras ITC", sans-serif;
}

.top-right-menu {
  position: absolute;
  top: 20px;
  right: 20px;
  display: flex;
  gap: 20px;
  z-index: 10;
  font-family: "Eras ITC", sans-serif;
}

.dropdown {
  position: relative;
  display: inline-block;
  font-family: "Eras ITC", sans-serif;
}

.dropbtn {
  background-color: #f9f9f9;
  color: black;
  padding: 10px 15px;
  border: 1px solid #ccc;
  border-radius: 5px;
  cursor: pointer;
  font-family: "Eras ITC", sans-serif;
}

.dropdown-content {
  display: none;
  position: absolute;
  background-color: #f9f9f9;
  min-width: 250px;
  box-shadow: 0px 8px 16px 0px rgba(0,0,0,0.2);
  z-index: 1;
  padding: 15px;
  border: 1px solid #ccc;
  border-radius: 5px;
  text-align: left;
  font-family: "Eras ITC", sans-serif;
}

.dropdown:hover .dropdown-content {
  display: block;
  font-family: "Eras ITC", sans-serif;
}

.dropdown-content .admin-button {
  display: block;
  width: 100%;
  padding: 10px;
  margin-bottom: 10px;
  text-align: left;
  border: 1px solid #ddd;
  border-radius: 4px;
  background-color: white;
  cursor: pointer;
  font-family: "Eras ITC", sans-serif;
}

.dropdown-content .admin-button:hover {
  background-color: #eee;
}

.dropdown-content h2 {
  margin-top: 15px;
  margin-bottom: 10px;
  font-size: 1.2em;
  font-family: "Eras ITC", sans-serif;
}

.dropdown-content > div {
  margin-top: 15px;
  border-top: 1px solid #eee;
  padding-top: 15px;
}

.dropdown-content > div:first-child {
  margin-top: 0;
  border-top: none;
  padding-top: 0;
}

.dropdown-content div div {
  margin-bottom: 10px;
  font-family: "Eras ITC", sans-serif;
}

.dropdown-content div label {
  display: block;
  margin-bottom: 5px;
}

.dropdown-content div input[type="text"],
.dropdown-content div input[type="number"],
.dropdown-content div textarea {
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
  width: 100%;
  box-sizing: border-box;
  margin-bottom: 10px;
  font-family: "Eras ITC", sans-serif;
}

.dropdown-content div textarea {
  min-height: 80px;
  font-family: "Eras ITC", sans-serif;
}

.edit-delete-list-container {
  max-height: 300px;
  overflow-y: auto;
  margin-top: 15px;
  border-top: 1px solid #eee;
  padding-top: 15px;
  font-family: "Eras ITC", sans-serif;
}

.edit-delete-list {
  list-style: none;
  padding: 0;
  font-family: "Eras ITC", sans-serif;
}

.edit-delete-list li {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 5px;
  padding-bottom: 5px;
  border-bottom: 1px solid #f0f0f0;
}

.edit-delete-list li:last-child {
  border-bottom: none;
}

.edit-delete-list li span {
  flex-grow: 1;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.edit-delete-list li button {
  padding: 5px 8px;
  border-radius: 3px;
  cursor: pointer;
  font-size: 0.9em;
}

.edit-delete-list li .delete-button {
  background-color: #dc3545;
  color: white;
  border: none;
}

.product-list {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 20px;
  margin-top: 20px;
  padding: 20px;
  font-family: "Eras ITC", sans-serif;
}

/* Estilos para el diálogo de edición se mantienen */
</style>