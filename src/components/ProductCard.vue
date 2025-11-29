<template>
  <div class="product-card">
    <img :src="productImageUrl" alt="Product Image">
    <router-link :to="`/product/${product.id}`">
      <h2>{{ product.name }}</h2>
    </router-link>
      <p class="description">{{ shortDescription }}</p>
      <div class="product-details">
        <div class="product-price">
          <p class="price">${{ product.price }}</p>
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
    },
    shortDescription() {
      const text = this.product.description;
      const maxLength = 100; // Adjust this number to make cards taller/shorter

      // If text is short enough, just show it all
      if (text.length <= maxLength) return text;

      // Cut the string at the limit
      let trimmed = text.substr(0, maxLength);

      // Attempt to cut at the last period (.) to keep a full sentence
      const lastPeriod = trimmed.lastIndexOf('.');

      if (lastPeriod > 0) {
        // If we found a period, cut right after it
        return trimmed.substring(0, lastPeriod + 1);
      }

      // Fallback: If the first sentence is huge (no periods found),
      // just cut at the last space so we don't chop a word in half.
      return trimmed.substr(0, Math.min(trimmed.length, trimmed.lastIndexOf(" ")));
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
  padding: 10px; /* Reduced padding */
  border-radius: 8px;
  height: 100%;
  display: flex;
  flex-direction: column;
  background: white;
  justify-content: space-between; 
}

img {
  width: 100%;
  height: 140px; 
  object-fit: contain;
  margin-bottom: 8px;
}

h2 {
  font-size: 1.1rem;
  margin: 5px 0;
  line-height: 1.2;
}

.description {
  font-size: 0.85rem;
  color: #555;
  margin-bottom: 10px;
  line-height: 1.3;
  flex-grow: 1;
}

.product-controls {
  display: flex;
  gap: 5px;
  margin-top: auto;
}

.quantity-input {
  width: 50px;
  padding: 5px;
}

button {
  flex: 1; /* Makes button fill remaining space */
  padding: 8px;
  background-color: #0046be;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background-color: #003da6;
}
</style>