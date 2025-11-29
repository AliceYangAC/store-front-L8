<template>
  <div class="product-card">
    <img :src="productImageUrl" alt="Product Image">
    <router-link :to="`/product/${product.id}`">
      <h2>{{ product.name }}</h2>
    </router-link>
      <p>{{ product.description }}</p>
      <div class="product-details">
        <div class="product-price">
          <p class="price">{{ product.price }}</p>
        </div>
        <div class="product-controls">
          <input type="number" v-model="quantity" min="1" class="quantity-input" />
          <button @click="addToCart">Add to Cart</button>
        </div>
      </div>
    </div>
</template>

<script>
export default {
  name: 'ProductCard',
  props: ['product'],
  data() {
    return {
      quantity: 1
    }
  },

  computed: {
    productImageUrl() {
      // Placeholder check
      if (this.product.image === '/placeholder.png') return this.product.image;
      
      // Full URL check
      // if (this.product.image && this.product.image.startsWith('http')) {
      //   return this.product.image;
      // }

      // Backend Proxy Logic
      let baseUrl = process.env.VUE_APP_PRODUCT_SERVICE_URL || 'http://localhost:3002';
      
      if (baseUrl.endsWith('/') && this.product.image.startsWith('/')) {
        baseUrl = baseUrl.slice(0, -1);
      }
      
      return `${baseUrl}${this.product.image}`;
    }
  },
  methods: {
    incrementQuantity() {
      this.quantity++
    },
    decrementQuantity() {
      if (this.quantity > 1) {
        this.quantity--
      }
    },
    addToCart() {
      this.$emit('addToCart', {
        productId: this.product.id,
        quantity: this.quantity
      })
    }
  }
}
</script>

<style scoped>
  .product-card {
    border: 1px solid #ddd;
    padding: 15px;
    border-radius: 8px;
    height: 100%;
    display: flex;
    flex-direction: column;
    background: white;
  }

  img {
    width: 100%;     
    height: 200px;   
    object-fit: contain; 
    margin-bottom: 10px;
  }
</style>