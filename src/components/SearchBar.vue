<template>
  <div class="search-container">
    <AutoComplete
      v-model="searchTerm"
      :suggestions="suggestions"
      @complete="searchProducts"
      @item-select="goToProductPage"
      placeholder="Search Best Buy"
      :dropdown="true"
      :forceSelection="false"
      field="name"
      class="p-inputtext-lg"
    />
  </div>
</template>

<script>
// NOTE: PrimeVue components are often imported and registered locally
// in the components object, or registered globally in main.js.
import AutoComplete from 'primevue/autocomplete';

export default {
  name: 'SearchBar',
  components: {
    AutoComplete
  },
  props: {
    // Pass the full product list down from App.vue
    products: {
      type: Array,
      required: true,
      default: () => []
    }
  },
  data() {
    return {
      searchTerm: '',
      suggestions: [] // This holds the dynamically filtered subset
    };
  },
  methods: {
    searchProducts(event) {
      // 1. Debouncing is handled internally by the component, 
      //    This method only runs after the debounce interval (if any is configured).
      
      const query = event.query.toLowerCase();

      if (!query.trim()) {
        this.suggestions = [];
      } else {
        // 2. Perform local filtering against the full products list
        this.suggestions = this.products.filter(product =>
          product.name.toLowerCase().includes(query)
        ).slice(0, 10); // Limit results
      }
    },
    goToProductPage(event) {
      // Event.value is the selected product object
      const productId = event.value.id;
      this.$router.push(`/product/${productId}`);
      this.searchTerm = ''; // Clear search bar after navigating
    }
  }
};
</script>

<style scoped>
/* Scoped styles are now only for layout/positioning, not for dropdown appearance */
.search-container {
  position: relative;
  width: 100%;
  max-width: 500px;
  margin: 0 20px;
}

/* Ensure the input box looks integrated */
.p-autocomplete {
    width: 100%;
}
</style>