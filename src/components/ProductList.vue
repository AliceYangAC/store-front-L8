<template>
  <div class="product-carousel-container">
    <div class="carousel-header">
      <h2>Featured Products</h2>
      <div class="carousel-controls">
        <button @click="scrollLeft" class="nav-btn">&lt;</button>
        <button @click="scrollRight" class="nav-btn">&gt;</button>
      </div>
    </div>

    <div class="carousel-track" ref="carousel">
      <div class="carousel-item" v-for="product in products" :key="product.id">
        <ProductCard
          :product="product"
          @addToCart="addToCart"
        />
      </div>
    </div>
    
  </div>
</template>

<script>
  import ProductCard from '../components/ProductCard'

  export default {
    name: 'ProductList',
    props: ['products'],
    components: {
      ProductCard
    },
    methods: {
      addToCart({ productId, quantity }) {
        this.$emit('addToCart', {
          productId: productId,
          quantity: quantity
        })
      },
      scrollLeft() {
        // Scroll left by 300px (approx width of one card)
        this.$refs.carousel.scrollBy({
          left: -320,
          behavior: 'smooth'
        });
      },
      scrollRight() {
        // Scroll right by 300px
        this.$refs.carousel.scrollBy({
          left: 320,
          behavior: 'smooth'
        });
      }
    }
  }
</script>

<style scoped>
.product-carousel-container {
  padding: 10px;
  max-width: 100%;
  overflow: hidden;
}

.carousel-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 15px;
}

.carousel-controls {
  display: flex;
  gap: 10px;
}

.nav-btn {
  background-color: #0046be; /* Best Buy Blue */
  color: white;
  border: none;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  font-size: 1.2rem;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background-color 0.2s;
}

.nav-btn:hover {
  background-color: #002a75;
}

/* The magic happening here */
.carousel-track {
  display: flex;
  gap: 20px;
  overflow-x: auto; /* Enable horizontal scrolling */
  scroll-behavior: smooth;
  scroll-snap-type: x mandatory; /* Forces items to snap into place */
  padding-bottom: 20px; /* Space for scrollbar if visible */
  
  /* Hide scrollbar for clean look in Webkit browsers (Chrome/Safari) */
  -ms-overflow-style: none;  /* IE and Edge */
  scrollbar-width: none;  /* Firefox */
}

/* Hide scrollbar for Chrome, Safari and Opera */
.carousel-track::-webkit-scrollbar {
  display: none;
}

.carousel-item {
  /* Prevent items from shrinking */
  flex: 0 0 auto;
  width: 300px; /* Fixed width for each card */
  scroll-snap-align: start; /* Snap to the left edge */
}
</style>