<!--App.vue-->

<template>
  <div class="app">
    <ProductFilterNav
      :products="products"
      @filtered-products="updateFilteredProducts"
    />
    <InStockFilter />
    <div class="product-list">
      <div
        v-for="product in filteredProducts"
        :key="product.id"
        class="product-card"
      >
        <img :src="product.image" alt="Product Image" />
        <h3>{{ product.name }}</h3>
        <p>{{ product.description }}</p>
        <div class="product-info">
          <p class="price">{{ product.price }}</p>
          <button>Add to Cart</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import ProductFilterNav from './components/ProductFilterNav.vue';
import products from './data/products.json';


export default {
  name: 'App',
  components: {
    ProductFilterNav,
  },
  data() {
    return {
      colors: [],
      sizes: [],
      products,
      filteredProducts:[],
    };
  },
  created() {
    this.filteredProducts = this.products;
  },
  methods: {
    updateFilteredProducts(newProducts) {
      this.filteredProducts = newProducts;
    },
  },
};
</script>

<style scoped>
.app {
  max-width: 1024px;
  margin: 0 auto;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  color: #606569;
}

.product-list {
  display: flex;
  flex-wrap: wrap;
}

.product-card {
  width: 250px;
  margin: 20px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
  border-radius: 5px;
  overflow: hidden;
}

.product-card img {
  width: 100%;
}

.product-card h3 {
  font-size: 20px;
  margin: 10px;
}

.product-card p {
  margin: 10px;
  font-size: 16px;
  line-height: 1.5;
}

.product-info {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin: 10px;
}

.price {
  font-size: 20px;
  font-weight: bold;
}
</style>
