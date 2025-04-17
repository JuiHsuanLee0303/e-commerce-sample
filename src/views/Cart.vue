<template>
  <div class="min-h-screen bg-gray-50 py-12">
    <div class="max-w-7xl mx-auto px-4">
      <h1 class="text-3xl font-bold mb-8">購物車</h1>

      <!-- Empty Cart -->
      <div v-if="cartItems.length === 0" class="text-center py-12">
        <p class="text-gray-600 mb-4">您的購物車是空的</p>
        <router-link
          to="/products"
          class="inline-block bg-black text-white px-6 py-3 rounded-lg hover:bg-gray-800 transition"
        >
          繼續購物
        </router-link>
      </div>

      <!-- Cart Items -->
      <div v-else class="bg-white rounded-lg shadow-sm overflow-hidden">
        <div class="divide-y">
          <div
            v-for="item in cartItems"
            :key="item.id"
            class="p-6 flex items-center gap-6"
          >
            <img
              :src="item.image"
              :alt="item.name"
              class="w-24 h-24 object-cover rounded-lg"
            />
            <div class="flex-1">
              <h3 class="text-lg font-semibold">{{ item.name }}</h3>
              <p class="text-gray-600">
                尺寸：{{ item.size }} | 顏色：{{ item.color }}
              </p>
              <p class="text-gray-900 font-bold mt-2">${{ item.price }}</p>
            </div>
            <div class="flex items-center gap-4">
              <div class="flex items-center border rounded-lg">
                <button
                  class="px-3 py-1 hover:bg-gray-100"
                  @click="updateQuantity(item, -1)"
                >
                  -
                </button>
                <span class="px-3 py-1">{{ item.quantity }}</span>
                <button
                  class="px-3 py-1 hover:bg-gray-100"
                  @click="updateQuantity(item, 1)"
                >
                  +
                </button>
              </div>
              <button
                class="text-red-500 hover:text-red-600"
                @click="removeItem(item)"
              >
                刪除
              </button>
            </div>
          </div>
        </div>

        <!-- Cart Summary -->
        <div class="p-6 bg-gray-50">
          <div class="flex justify-between items-center mb-4">
            <span class="text-lg">總計</span>
            <span class="text-2xl font-bold">${{ total }}</span>
          </div>
          <button
            class="w-full bg-black text-white py-4 rounded-lg hover:bg-gray-800 transition"
            @click="checkout"
          >
            結帳
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";

// 模擬購物車數據
const cartItems = ref([
  {
    id: 1,
    name: "簡約純棉T恤",
    price: 299,
    image:
      "https://images.unsplash.com/photo-1521572163474-6864f9cf17ab?ixlib=rb-1.2.1&auto=format&fit=crop&w=800&q=80",
    size: "M",
    color: "白色",
    quantity: 1,
  },
  {
    id: 2,
    name: "修身牛仔褲",
    price: 599,
    image:
      "https://images.unsplash.com/photo-1542272604-787c3835535d?ixlib=rb-1.2.1&auto=format&fit=crop&w=800&q=80",
    size: "30",
    color: "深藍",
    quantity: 1,
  },
]);

const total = computed(() => {
  return cartItems.value.reduce(
    (sum, item) => sum + item.price * item.quantity,
    0
  );
});

const updateQuantity = (item, change) => {
  const newQuantity = item.quantity + change;
  if (newQuantity > 0) {
    item.quantity = newQuantity;
  }
};

const removeItem = (item) => {
  const index = cartItems.value.indexOf(item);
  if (index > -1) {
    cartItems.value.splice(index, 1);
  }
};

const checkout = () => {
  alert("感謝您的購買！");
  cartItems.value = [];
};
</script>
