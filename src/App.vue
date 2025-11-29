<template>
  <TopNav :cartItemCount="cartItemCount"/>
  
  <div class="main-container">
    <aside class="sidebar">
      <FilterMenu 
        :categories="uniqueCategories" 
        :selectedCategory="currentCategory"
        @filterUpdate="updateFilter"
      />
    </aside>

    <main class="content">
      <router-view
        :products="filteredProducts"
        :cartItems="cartItems"
        @addToCart="addToCart"
        @removeFromCart="removeFromCart"
        @submitOrder="submitOrder"
      ></router-view>
    </main>
  </div>
</template>

<script>
import TopNav from './components/TopNav.vue'
import FilterMenu from './components/FilterMenu.vue' // <--- IMPORT THE NEW COMPONENT

export default {
  name: 'App',
  components: {
    TopNav,
    FilterMenu // <--- REGISTER THE COMPONENT
  },
  data() {
    return {
      cartItems: [],
      products: [],
      currentCategory: '' // <--- STATE FOR SELECTED CATEGORY
    }
  },
  computed: {
    cartItemCount() {
      return this.cartItems.reduce((total, item) => {
        return total + item.quantity
      }, 0)
    },
    // <--- LOGIC TO GET LIST OF CATEGORIES FROM PRODUCTS
    uniqueCategories() {
      const categories = this.products.map(p => p.category).filter(c => c);
      return [...new Set(categories)].sort();
    },
    // <--- LOGIC TO FILTER THE PRODUCTS LIST
    filteredProducts() {
      if (this.currentCategory === '') {
        return this.products;
      }
      return this.products.filter(p => p.category === this.currentCategory);
    }
  },
  mounted() {
    this.getProducts()
  },
  methods: {
    // <--- METHOD TO HANDLE CATEGORY CLICK
    updateFilter(category) {
      this.currentCategory = category;
      if (this.$route.path !== '/') {
        this.$router.push('/');
      }
    },
    getProducts() {
      fetch('/products')
        .then(response => response.json())
        .then(products => {
          console.log('success getting proxy products')
          this.products = products
        })
        .catch(error => {
          console.log(error)
          alert('Error occurred while fetching products')
        })
    },
    addToCart({ productId, quantity }) {
      const existingCartItem = this.cartItems.find(
        item => item.product.id == productId
      )
      if (existingCartItem) {
        existingCartItem.quantity += quantity
      } else {
        const product = this.products.find(product => product.id == productId)
        this.cartItems.push({ product, quantity })
      }
    },
    removeFromCart(index) {
      this.cartItems.splice(index, 1)
    },
    submitOrder() {
      const order = {
        customerId: Math.floor(Math.random() * 10000000000).toString(),
        items: this.cartItems.map(item => {
          return {
            productId: item.product.id,
            quantity: item.quantity,
            price: item.product.price
          }
        })
      }

      console.log(JSON.stringify(order));

      fetch(`/order`, {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(order)
      })
        .then(response => {
          console.log(response)
          if (!response.ok) {
            alert('Error occurred while submitting order')
          } else {
            this.cartItems = []
            alert('Order submitted successfully')
          }
        })
        .catch(error => {
          console.log(error)
          alert('Error occurred while submitting order')
        })
    }
  },
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;700&display=swap');

body {
  background-color: white;
  background-size: cover;
  background-position: center;
  background-attachment: fixed;
  margin: 0;
  padding: 0;
}

#app {
  font-family: 'Roboto', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 120px; /* Adjust for fixed header */
}

/* --- NEW LAYOUT STYLES FOR SIDEBAR --- */
.main-container {
  display: flex; /* Creates side-by-side layout */
  max-width: 1400px;
  margin: 0 auto;
  padding: 20px;
  gap: 20px;
  align-items: flex-start;
}

.sidebar {
  flex: 0 0 250px; /* Fixed width sidebar */
  position: sticky;
  top: 140px; /* Stick below the header */
}

.content {
  flex: 1; /* Takes remaining space */
  min-width: 0; /* Prevents flex items from overflowing */
}

/* Responsive: Stack them on mobile */
@media (max-width: 768px) {
  .main-container {
    flex-direction: column;
  }
  .sidebar {
    width: 100%;
    position: static;
    margin-bottom: 20px;
  }
}
/* ------------------------------------- */

footer {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  background-color: #0a5620;
  color: #fff;
  padding: 1rem;
  margin: 0;
}

/* ... Keep your existing global styles below if needed ... */
nav {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

ul {
  display: flex;
  list-style: none;
  margin: 0;
  padding: 0;
}

li {
  margin: 0 1rem;
}

a {
  color: #fff;
  text-decoration: none;
}

button {
  padding: 10px;
  background-color: #005f8b;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  height: 42px;
}

.quantity-input {
  width: 50px;
  height: 30px;
  border: 1px solid #ccc;
  border-radius: 5px;
  padding: 5px;
  margin-right: 10px;
}

.checkout-button {
  margin-top: 20px;
  padding: 10px 20px;
  background-color: #007acc;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.checkout-button:hover {
  background-color: #005f8b;
}
</style>