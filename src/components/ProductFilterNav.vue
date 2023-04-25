<!--ProductFilterNav.vue-->
<template>
    <div>
        <p>
        <strong>Show only available products:</strong>
        <input type="checkbox" v-model="showOnlyAvailable" />
      </p>
      <p>
        <strong>Sort by:</strong>
        <select v-model="sortBy">
          <option value="price-asc">Price (Low to High)</option>
          <option value="price-desc">Price (High to Low)</option>
          <option value="relevance">Relevance</option>
        </select>
      </p>
      <p>
        <strong>Brand:</strong>
        <select v-model="brandFilter">
          <option value="">All Brands</option>
          <option value="Nike">Nike</option>
          <option value="Adidas">Adidas</option>
          <option value="Converse">Converse</option>
          <option value="Crocs">Crocs</option>
        </select>
      </p>
      <p>{{ filteredProducts.length }} Results</p>
    </div>
  </template>
  
  <script>
  export default {
    name: "ProductFilterNav",
    props: {
      products: Array,
    },
    data() {
      return {
        search: "",
        showOnlyAvailable: false,
        sortBy: "price-asc",
        brandFilter: "",
        filteredProducts: [],
      };
    },
    watch: {
      search: "filterProducts",
      showOnlyAvailable: "filterProducts",
      sortBy: "filterProducts",
      brandFilter: "filterProducts",
    },
    created() {
      this.filterProducts();
    },
    methods: {
      filterProducts() {
        let products = this.products.slice();
  
        if (this.showOnlyAvailable) {
          products = products.filter((product) => product.isAvailable);
        }
  
        if (this.brandFilter) {
          products = products.filter((product) => product.brand === this.brandFilter);
        }
  
        if (this.sortBy === "price-asc") {
          products.sort((a, b) => parseFloat(a.price.slice(1)) - parseFloat(b.price.slice(1)));
        } else if (this.sortBy === "price-desc") {
          products.sort((a, b) => parseFloat(b.price.slice(1)) - parseFloat(a.price.slice(1)));
        } else if (this.sortBy === "relevance") {
          products.sort((a, b) => {
            const availableA = a.isAvailable ? 0 : 1;
            const availableB = b.isAvailable ? 0 : 1;
            return (
              availableA - availableB ||
              (a.rank !== null ? a.rank : Infinity) - (b.rank !== null ? b.rank : Infinity)
            );
          });
        }
  
        this.filteredProducts = products;
        this.$emit("filtered-products", this.filteredProducts);
      },
    },
  };
  </script>
  