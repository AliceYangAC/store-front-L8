<template>
  <div class="filter-menu">
    <h3>Shop by Category</h3>
    <ul>
      <li :class="{ active: selectedCategory === '' }">
        <button @click="selectCategory('')">All Products</button>
      </li>
      
      <li v-for="category in categories" :key="category" :class="{ active: selectedCategory === category }">
        <button @click="selectCategory(category)">
          {{ category }}
        </button>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'FilterMenu',
  props: {
    categories: Array,
    selectedCategory: String
  },
  emits: ['filterUpdate'],
  methods: {
    selectCategory(category) {
      this.$emit('filterUpdate', category);
    }
  }
}
</script>

<style scoped>
.filter-menu {
  background-color: #f4f4f4;
  padding: 15px;
  border-radius: 8px;
  min-width: 200px;
  text-align: left;
}

h3 {
  margin-top: 0;
  font-size: 1.1rem;
  border-bottom: 2px solid #ddd;
  padding-bottom: 10px;
  margin-bottom: 10px;
}

ul {
  list-style: none;
  padding: 0;
  margin: 0;
  display: flex;
  flex-direction: column;
}

li {
  margin: 0;
}

button {
  background: none;
  border: none;
  color: #0046be; 
  cursor: pointer;
  padding: 8px 5px;
  text-align: left;
  width: 100%;
  font-size: 1rem;
  border-radius: 4px;
  transition: all 0.2s;
}

button:hover {
  background-color: #e0e0e0;
  text-decoration: underline;
}

li.active button {
  background-color: #0046be;
  color: white;
  font-weight: bold;
}
</style>