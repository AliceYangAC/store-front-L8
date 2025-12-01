<template>
  <div class="search-container">
    <AutoComplete
      v-model="searchTerm"
      :suggestions="suggestions"
      @complete="searchProducts"
      @item-select="goToProductPage"
      @keyup.enter="selectFirstSuggestion"
      placeholder="Search Best Buy"
      :dropdown="false"                     
      :forceSelection="false"
      field="name"
      class="p-autocomplete-custom"
    >
      <template #inputgroupappend>
        <button class="search-btn" @click="selectFirstSuggestion">
          <svg width="20" height="20" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path d="M15.5 14H14.71L14.43 13.73C15.41 12.59 16 11.11 16 9.5C16 5.91 13.09 3 9.5 3C5.91 3 3 5.91 3 9.5C3 13.09 5.91 16 9.5 16C11.11 16 12.59 15.41 13.73 14.43L14 14.71V15.5L19 20.49L20.49 19L15.5 14ZM9.5 14C7.01 14 5 11.99 5 9.5C5 7.01 7.01 5 9.5 5C11.99 5 14 7.01 14 9.5C14 11.99 11.99 14 9.5 14Z" fill="currentColor"/>
          </svg>
        </button>
      </template>
    </AutoComplete>
  </div>
</template>

<script>
import AutoComplete from 'primevue/autocomplete';

export default {
  name: 'SearchBar',
  components: {
    AutoComplete
  },
  props: {
    products: {
      type: Array,
      required: true,
      default: () => []
    }
  },
  data() {
    return {
      searchTerm: '',
      suggestions: []
    };
  },
  methods: {
    searchProducts(event) {
      const query = event.query.toLowerCase();

      if (!query.trim()) {
        this.suggestions = [];
      } else {
        // Filtering is correct: based on product name
        this.suggestions = this.products.filter(product =>
          product.name.toLowerCase().includes(query)
        ).slice(0, 10);
      }
    },
    selectFirstSuggestion() {
      // If user presses Enter or clicks the search button
      if (this.suggestions.length > 0) {
        // Navigate to the first result
        this.goToProductPage({ value: this.suggestions[0] });
      } else if (this.searchTerm.trim() !== '') {
        // If no suggestion matches, navigate to a full search results page (or clear the input)
        this.$router.push({ path: '/', query: { search: this.searchTerm } });
      }
    },
    goToProductPage(event) {
      const productId = event.value.id;
      this.$router.push(`/product/${productId}`);
      this.searchTerm = '';
    }
  }
};
</script>

<style>

.search-container {
  position: relative;
  width: 100%;
  max-width: 500px;
}

.p-autocomplete-custom .p-inputtext {
  border-radius: 20px;
  padding-right: 40px !important; 
}

.p-autocomplete-custom .p-inputgroup-addon {
  border: none;
  background: transparent;
}

.search-btn {
  background: transparent;
  border: none;
  cursor: pointer;
  color: #333; 
  position: absolute;
  right: 0;
  top: 50%;
  transform: translateY(-50%);
  height: 100%;
  padding: 0 10px;
  z-index: 3; 
}

.search-btn:hover {
  color: #0046BE; 
}

.p-autocomplete .p-inputtext {
  border-color: #0046BE !important; 
  box-shadow: none !important;
}
</style>