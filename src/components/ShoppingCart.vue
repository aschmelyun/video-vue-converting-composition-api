<template>
  <div class="flex mb-10">
    <div class="w-full px-10 py-10">
      <div class="flex justify-between border-b pb-8">
        <h1 class="font-semibold text-2xl">{{ title }}</h1>
        <h2 class="font-semibold text-2xl">{{ products.length }} Items</h2>
      </div>
      <div class="flex mt-10 mb-5">
        <h3 class="font-semibold text-gray-600 text-xs uppercase w-2/5">Product Details</h3>
        <h3 class="font-semibold text-center text-gray-600 text-xs uppercase w-1/5 text-center">Quantity</h3>
        <h3 class="font-semibold text-center text-gray-600 text-xs uppercase w-1/5 text-center">Price</h3>
        <h3 class="font-semibold text-center text-gray-600 text-xs uppercase w-1/5 text-center">Total</h3>
      </div>
      <div class="flex items-center hover:bg-gray-100 -mx-8 px-6 py-5" v-for="(product, index) in products" :key="index">
        <div class="flex w-2/5">
          <div class="w-20">
            <img class="h-24 w-24 object-cover" :src="product.image">
          </div>
          <div class="flex flex-col ml-4 mt-4 flex-grow">
            <span class="font-bold text-sm">{{ product.name }}</span>
            <span class="text-sm mt-2">{{ product.description }}</span>
          </div>
        </div>
        <div class="flex justify-center w-1/5">
          <button @click="decreaseQuantity(index)">-</button>
          <input class="mx-2 border text-center w-8" type="text" :value="product.quantity">
          <button @click="increaseQuantity(index)">+</button>
        </div>
        <span class="text-center w-1/5 font-semibold text-sm">${{ product.price }}</span>
        <span class="text-center w-1/5 font-semibold text-sm">${{ (product.quantity * product.price).toFixed(2) }}</span>
      </div>
    </div>

    <div id="summary" class="w-1/4 px-8 py-10">
      <h1 class="font-semibold text-2xl border-b pb-8">Order Summary</h1>
      <div class="flex justify-between mt-10 mb-5" v-for="(product, index) in products" :key="index">
        <span class="font-semibold text-sm uppercase">{{ product.name }} x {{ product.quantity }}</span>
        <span class="font-semibold text-sm">${{ (product.quantity * product.price).toFixed(2) }}</span>
      </div>
      <div class="border-t mt-8">
        <div class="flex font-semibold justify-between py-6 text-sm uppercase">
          <span>Total cost</span>
          <span>${{ totalCost }}</span>
        </div>
        <button class="bg-indigo-500 font-semibold hover:bg-indigo-600 py-3 text-sm text-white uppercase w-full">Checkout</button>
      </div>
    </div>
  </div>
</template>

<!-- Composition API -->
<script setup>
import { ref, reactive, defineProps, computed } from 'vue'

// Props
const props = defineProps({
  title: String
})

// Data Attributes
const products = ref([
  {
    name: 'A Cool Shirt',
    description: 'It has a relaxed fit that feels light and airy.',
    image: 'https://images.unsplash.com/photo-1596755094514-f87e34085b2c?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=988&q=80',
    price: 19.99,
    quantity: 1
  },
  {
    name: 'Some Neat Pants',
    description: 'Made from durable fabric and dyed charcoal gray.',
    image: 'https://images.unsplash.com/photo-1624378441864-6eda7eac51cb?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=988&q=80',
    price: 34.99,
    quantity: 1
  },
  {
    name: 'Sparkling Water',
    description: 'Tastes like TV static but you love it anyway.',
    image: 'https://images.unsplash.com/photo-1614714029749-9868938bf2a2?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=987&q=80',
    price: 4.49,
    quantity: 1
  }
])

// Methods
const decreaseQuantity = (index) => {
  products.value[index].quantity -= 1

  if (products.value[index].quantity === 0) {
    products.value.splice(index, 1)
  }
}

const increaseQuantity = (index) => {
  products.value[index].quantity += 1
}

// Computed Properties
const totalCost = computed(() => {
  return products.value.reduce((acc, product) => acc += (product.price * product.quantity), 0).toFixed(2)
})

// User Stuff
const user = reactive({
  name: '',
  email: ''
})

const userString = computed(() => {
  return user.name + ' ' + user.email
})
</script>

<!-- Options API -->
<!-- <script>
export default {
  props: {
    title: String
  },
  data() {
    return {
      products: [
        {
          name: 'A Cool Shirt',
          description: 'It has a relaxed fit that feels light and airy.',
          image: 'https://images.unsplash.com/photo-1596755094514-f87e34085b2c?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=988&q=80',
          price: 19.99,
          quantity: 1
        },
        {
          name: 'Some Neat Pants',
          description: 'Made from durable fabric and dyed charcoal gray.',
          image: 'https://images.unsplash.com/photo-1624378441864-6eda7eac51cb?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=988&q=80',
          price: 34.99,
          quantity: 1
        },
        {
          name: 'Sparkling Water',
          description: 'Tastes like TV static but you love it anyway.',
          image: 'https://images.unsplash.com/photo-1614714029749-9868938bf2a2?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=987&q=80',
          price: 4.49,
          quantity: 1
        }
      ],
      user: {
        name: '',
        email: ''
      }
    }
  },
  methods: {
    decreaseQuantity(index) {
      this.products[index].quantity -= 1

      if (this.products[index].quantity === 0) {
        this.products.splice(index, 1)
      }
    },
    increaseQuantity(index) {
      this.products[index].quantity += 1
    }
  },
  computed: {
    totalCost() {
      return this.products.reduce((acc, product) => acc += (product.price * product.quantity), 0).toFixed(2)
    },
    userInfo() {
      return this.user.name + ' ' + this.user.email;
    }
  }
}
</script> -->