<template>
  <div class="container mx-auto">
    <ShoppingCart title="Your Current Order" />
  </div>
</template>

<script>
import ShoppingCart from './components/ShoppingCart.vue'

export default {
  name: 'App',
  components: {
    ShoppingCart
  }
}
</script>