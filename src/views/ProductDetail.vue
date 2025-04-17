<template>
  <div class="min-h-screen bg-gray-50 py-12">
    <div class="max-w-7xl mx-auto px-4">
      <div v-if="product" class="bg-white rounded-lg shadow-sm overflow-hidden">
        <div class="grid grid-cols-1 md:grid-cols-2 gap-8 p-8">
          <!-- Product Image -->
          <div class="relative">
            <img
              :src="product.image"
              :alt="product.name"
              class="w-full h-[600px] object-cover rounded-lg"
            />
          </div>

          <!-- Product Info -->
          <div class="flex flex-col">
            <h1 class="text-3xl font-bold mb-4">{{ product.name }}</h1>
            <p class="text-2xl font-bold text-gray-900 mb-6">
              ${{ product.price }}
            </p>
            <p class="text-gray-600 mb-8">{{ product.description }}</p>

            <!-- Size Selection -->
            <div class="mb-8">
              <h3 class="text-lg font-semibold mb-4">選擇尺寸</h3>
              <div class="flex flex-wrap gap-2">
                <button
                  v-for="size in product.sizes"
                  :key="size"
                  class="px-4 py-2 border rounded-lg hover:border-black transition"
                  :class="{ 'border-black': selectedSize === size }"
                  @click="selectedSize = size"
                >
                  {{ size }}
                </button>
              </div>
            </div>

            <!-- Color Selection -->
            <div class="mb-8">
              <h3 class="text-lg font-semibold mb-4">選擇顏色</h3>
              <div class="flex flex-wrap gap-2">
                <button
                  v-for="color in product.colors"
                  :key="color"
                  class="px-4 py-2 border rounded-lg hover:border-black transition"
                  :class="{ 'border-black': selectedColor === color }"
                  @click="selectedColor = color"
                >
                  {{ color }}
                </button>
              </div>
            </div>

            <!-- Add to Cart -->
            <button
              class="w-full bg-black text-white py-4 rounded-lg hover:bg-gray-800 transition"
              @click="addToCart"
            >
              加入購物車
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import { useRoute } from "vue-router";
import { products } from "../data/products";

const route = useRoute();
const product = ref(null);
const selectedSize = ref("");
const selectedColor = ref("");

onMounted(() => {
  const productId = parseInt(route.params.id);
  product.value = products.find((p) => p.id === productId);
});

const addToCart = () => {
  if (!selectedSize.value || !selectedColor.value) {
    alert("請選擇尺寸和顏色");
    return;
  }
  // 這裡可以添加購物車邏輯
  alert("已加入購物車！");
};
</script>
