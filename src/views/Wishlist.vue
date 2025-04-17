<template>
  <div class="min-h-screen bg-gray-50 py-12">
    <div class="max-w-7xl mx-auto px-4">
      <h1 class="text-3xl font-bold mb-8">願望清單</h1>

      <!-- Empty Wishlist -->
      <div v-if="wishlistItems.length === 0" class="text-center py-12">
        <p class="text-gray-600 mb-4">您的願望清單是空的</p>
        <router-link
          to="/products"
          class="inline-block bg-black text-white px-6 py-3 rounded-lg hover:bg-gray-800 transition"
        >
          去購物
        </router-link>
      </div>

      <!-- Wishlist Items -->
      <div v-else class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
        <div
          v-for="item in wishlistItems"
          :key="item.id"
          class="bg-white rounded-lg overflow-hidden shadow-sm hover:shadow-md transition group relative"
        >
          <router-link :to="'/product/' + item.id">
            <img
              :src="item.image"
              :alt="item.name"
              class="w-full h-64 object-cover"
            />
            <div class="p-4">
              <h3 class="text-lg font-semibold mb-2">{{ item.name }}</h3>
              <p class="text-gray-600 mb-2">{{ item.description }}</p>
              <p class="text-xl font-bold">${{ item.price }}</p>
            </div>
          </router-link>

          <!-- Action Buttons -->
          <div class="absolute top-2 right-2 flex gap-2">
            <button
              @click.prevent="removeFromWishlist(item)"
              class="bg-white/80 hover:bg-white p-2 rounded-full text-gray-600 hover:text-red-500 transition"
            >
              ❌
            </button>
            <button
              @click.prevent="addToCart(item)"
              class="bg-white/80 hover:bg-white p-2 rounded-full text-gray-600 hover:text-black transition"
            >
              🛒
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import { products } from "../data/products";

// 模擬願望清單數據
const wishlistItems = ref([products[0], products[2]]);

const removeFromWishlist = (item) => {
  const index = wishlistItems.value.indexOf(item);
  if (index > -1) {
    wishlistItems.value.splice(index, 1);
  }
};

const addToCart = (item) => {
  alert("商品已加入購物車！");
  removeFromWishlist(item);
};
</script>
