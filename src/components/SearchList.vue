<template>
  <div class="search-results-container">
    <div class="list-header">
      <div class="search-title">
        <h2>Search Results for "{{ currentSearchQuery }}"</h2>
        <span class="product-count">{{ filteredResults.length }} items</span>
      </div>
      
      <router-link to="/">
        <button class="button">← Back to All Products</button>
      </router-link>
    </div>

    <div class="product-grid">
      <div class="grid-item" v-for="product in filteredResults" :key="product.id">
        <ProductCard
          :product="product"
          @addToCart="addToCart"
        />
      </div>
    </div>

    <div v-if="!filteredResults.length">
        <p>No products found matching "{{ currentSearchQuery }}".</p>
    </div>
  </div>
</template>

<script>
import ProductCard from './ProductCard.vue' 

export default {
  name: 'SearchList',
  components: {
    ProductCard
  },
  props: {
    products: {
      type: Array,
      required: true,
      default: () => []
    }
  },
  computed: {
    currentSearchQuery() {
      return this.$route.query.q || ''; 
    },
    
    filteredResults() {
      const query = this.currentSearchQuery.toLowerCase().trim();
      
      if (!query) {
        return [];
      }
      
      return this.products.filter(product =>
        product.name.toLowerCase().includes(query)
      );
    }
  },
  methods: {
    
    addToCart(event) {
        this.$emit('addToCart', event);
    }
  }
}
</script>

<style scoped>
.search-results-container {
    max-width: 100%;
    box-sizing: border-box; 
    padding: 0 20px;
}

.list-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20px;
    padding-bottom: 10px;
    border-bottom: 1px solid #eee;
}

.search-title h2 {
    margin-top: 0;
    font-size: 1.5rem;
}

.product-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr); 
    gap: 20px; 
    align-items: stretch;
}

@media (max-width: 1024px) {
  .product-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 600px) {
  .product-grid {
    grid-template-columns: 1fr;
  }
}
</style>