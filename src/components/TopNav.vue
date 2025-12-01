<template>
  <nav>
    <div class="logo">
      <a href="/">
        <img src="/best-buy-logo.png" alt="Best Buy Logo">
      </a>
    </div>
    
    <SearchBar :products="products" /> 

    <button class="hamburger" @click="toggleNav">
      <span class="hamburger-icon"></span>
    </button>
    <ul class="nav-links" :class="{ 'nav-links--open': isNavOpen }">
      <li><router-link to="/" @click="closeNav">Products</router-link></li>
      <li><router-link to="/cart" @click="closeNav">Cart ({{ cartItemCount }})</router-link></li>
    </ul>
  </nav>
</template>
<script>
import SearchBar from './SearchBar.vue';

export default {
  name: 'TopNav',
  components: {
    SearchBar 
  },
  props: {
    cartItemCount: Number,
    products: {           
      type: Array,
      default: () => []
    }
  },
  methods: {
    toggleNav() {
      this.isNavOpen = !this.isNavOpen
    },
    closeNav() {
      this.isNavOpen = false
    }
  }
}
</script>

<style scoped>
nav {
  display: flex;
  /* CHANGED: Use space-around/space-evenly to center the search bar */
  justify-content: flex-start; 
  align-items: center;
  background-color: #0046BE;
  color: #fff;
  padding-top: 0.5rem;
  padding-left: 1rem;
  padding-right: 1rem;
  padding-bottom: 0.25rem;
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 100; /* Ensure nav is above all content */
}

/* NEW: Style for the SearchBar wrapper */
.search-container {
    /* Allows the search bar to grow and take up maximum space */
    flex-grow: 1; 
    /* Ensures minimum spacing from the logo */
    margin: 0 20px; 
    max-width: 500px; /* Limits size on very wide screens */
}

nav img {
  width: 120px;
  height: auto;
}

.nav-links {
  /* Ensure links don't shrink and are pushed to the right */
  flex-shrink: 0; 
  display: flex;
  list-style: none;
  font-size: 1.2rem;
  font-weight: bold;
}

.hamburger {
  display: none;
  background: none;
  border: none;
  cursor: pointer;
  padding: 0;
  margin: 0;
  
  /* FIX ALIGNMENT ISSUE: Remove old magic number (-40px) 
     and rely on align-items: center in 'nav' */
  margin-top: 0; 
}

.hamburger-icon {
  display: block;
  width: 20px;
  height: 2px;
  background-color: #fff;
  position: relative;
  top: 50%;
  transform: translateY(-50%);
}

.hamburger-icon::before,
.hamburger-icon::after {
  content: '';
  display: block;
  width: 20px;
  height: 2px;
  background-color: #fff;
  position: absolute;
  left: 0;
}

.hamburger-icon::before {
  top: -6px;
}

.hamburger-icon::after {
  bottom: -6px;
}

@media (max-width: 768px) {
  .search-container {
    order: 3; 
    margin: 0;
    width: 100%;
    max-width: none;
    position: absolute;
    top: 100%;
    left: 0;
    padding: 0.5rem 1rem;
    background-color: #0046BE; 
  }

  .nav-links {
    display: none;
    position: absolute;
    top: calc(100% + 50px); 
    left: 0;
    right: 0;
    background-color: #333;
    padding: 1rem;
  }
  
  .nav-links a {
  color: white; /* Ensure links are explicitly white */
  text-decoration: none;
}
  .nav-links--open {
    display: block;
  }

  .nav-links--open li {
    padding: 0.5rem 0;
  }

  .hamburger {
    display: block;
  }
}
</style>