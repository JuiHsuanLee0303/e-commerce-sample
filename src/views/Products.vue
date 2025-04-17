<template>
  <div class="min-h-screen bg-white py-12">
    <div class="max-w-7xl mx-auto px-4">
      <!-- Filters -->
      <div class="mb-12">
        <div class="flex flex-wrap gap-6">
          <!-- Category Filter -->
          <div class="w-full md:w-auto">
            <label
              class="block text-xs font-medium text-gray-500 uppercase tracking-wider mb-2"
              >分類</label
            >
            <select
              v-model="selectedCategory"
              class="w-full md:w-48 px-4 py-2 border-0 border-b-2 border-gray-200 focus:border-black focus:outline-none focus:ring-0 bg-transparent"
            >
              <option value="">所有分類</option>
              <option value="上衣">上衣</option>
              <option value="褲裝">褲裝</option>
              <option value="裙裝">裙裝</option>
            </select>
          </div>

          <!-- Price Filter -->
          <div class="w-full md:w-auto">
            <label
              class="block text-xs font-medium text-gray-500 uppercase tracking-wider mb-2"
              >價格範圍</label
            >
            <select
              v-model="selectedPrice"
              class="w-full md:w-48 px-4 py-2 border-0 border-b-2 border-gray-200 focus:border-black focus:outline-none focus:ring-0 bg-transparent"
            >
              <option value="">所有價格</option>
              <option value="0-300">$300以下</option>
              <option value="300-600">$300-$600</option>
              <option value="600-1000">$600-$1000</option>
              <option value="1000+">$1000以上</option>
            </select>
          </div>

          <!-- Size Filter -->
          <div class="w-full md:w-auto">
            <label
              class="block text-xs font-medium text-gray-500 uppercase tracking-wider mb-2"
              >尺寸</label
            >
            <select
              v-model="selectedSize"
              class="w-full md:w-48 px-4 py-2 border-0 border-b-2 border-gray-200 focus:border-black focus:outline-none focus:ring-0 bg-transparent"
            >
              <option value="">所有尺寸</option>
              <option value="XS">XS</option>
              <option value="S">S</option>
              <option value="M">M</option>
              <option value="L">L</option>
              <option value="XL">XL</option>
            </select>
          </div>

          <!-- Sort -->
          <div class="w-full md:w-auto">
            <label
              class="block text-xs font-medium text-gray-500 uppercase tracking-wider mb-2"
              >排序方式</label
            >
            <select
              v-model="sortBy"
              class="w-full md:w-48 px-4 py-2 border-0 border-b-2 border-gray-200 focus:border-black focus:outline-none focus:ring-0 bg-transparent"
            >
              <option value="default">預設排序</option>
              <option value="price-asc">價格由低到高</option>
              <option value="price-desc">價格由高到低</option>
              <option value="name-asc">名稱 A-Z</option>
              <option value="name-desc">名稱 Z-A</option>
            </select>
          </div>
        </div>

        <!-- Active Filters -->
        <div v-if="hasActiveFilters" class="mt-6 flex flex-wrap gap-2">
          <div
            v-for="filter in activeFilters"
            :key="filter.type"
            class="bg-gray-100 px-3 py-1 rounded-full text-xs uppercase tracking-wider flex items-center gap-2"
          >
            <span>{{ filter.label }}</span>
            <button
              @click="clearFilter(filter.type)"
              class="text-gray-500 hover:text-black transition-colors"
            >
              ×
            </button>
          </div>
          <button
            @click="clearAllFilters"
            class="text-xs text-gray-500 hover:text-black transition-colors uppercase tracking-wider"
          >
            清除所有篩選
          </button>
        </div>
      </div>

      <!-- Products Grid -->
      <div v-if="filteredProducts.length === 0" class="text-center py-20">
        <p class="text-gray-500 mb-6 uppercase tracking-wider">
          沒有符合條件的商品
        </p>
        <button
          @click="clearAllFilters"
          class="inline-block bg-black text-white px-8 py-3 rounded-none hover:bg-gray-800 transition uppercase tracking-wider text-sm"
        >
          清除篩選條件
        </button>
      </div>

      <div v-else class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-8">
        <div
          v-for="product in filteredProducts"
          :key="product.id"
          class="group relative"
        >
          <router-link :to="'/product/' + product.id" class="block">
            <div class="relative overflow-hidden mb-4">
              <img
                :src="product.image"
                :alt="product.name"
                class="w-full h-80 object-cover transition-transform duration-500 group-hover:scale-105"
              />
              <div
                class="absolute inset-0 bg-black opacity-0 group-hover:opacity-10 transition-opacity duration-300"
              ></div>
            </div>
            <div class="text-center">
              <h3 class="text-lg font-light mb-1">{{ product.name }}</h3>
              <p class="text-gray-500 text-sm mb-2">
                {{ product.description }}
              </p>
              <p class="text-xl font-light">${{ product.price }}</p>
            </div>
          </router-link>

          <!-- Quick Actions -->
          <div
            class="absolute top-4 right-4 flex flex-col gap-2 opacity-0 group-hover:opacity-100 transition-opacity duration-300"
          >
            <button
              @click.prevent="toggleWishlist(product)"
              class="bg-white p-2 rounded-full text-gray-600 hover:text-red-500 transition-colors shadow-sm"
            >
              <svg
                v-if="isInWishlist(product)"
                xmlns="http://www.w3.org/2000/svg"
                class="h-5 w-5"
                viewBox="0 0 20 20"
                fill="currentColor"
              >
                <path
                  fill-rule="evenodd"
                  d="M3.172 5.172a4 4 0 015.656 0L10 6.343l1.172-1.171a4 4 0 115.656 5.656L10 17.657l-6.828-6.829a4 4 0 010-5.656z"
                  clip-rule="evenodd"
                />
              </svg>
              <svg
                v-else
                xmlns="http://www.w3.org/2000/svg"
                class="h-5 w-5"
                fill="none"
                viewBox="0 0 24 24"
                stroke="currentColor"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="2"
                  d="M4.318 6.318a4.5 4.5 0 000 6.364L12 20.364l7.682-7.682a4.5 4.5 0 00-6.364-6.364L12 7.636l-1.318-1.318a4.5 4.5 0 00-6.364 0z"
                />
              </svg>
            </button>
            <button
              @click.prevent="quickAddToCart(product)"
              class="bg-white p-2 rounded-full text-gray-600 hover:text-black transition-colors shadow-sm"
            >
              <svg
                xmlns="http://www.w3.org/2000/svg"
                class="h-5 w-5"
                fill="none"
                viewBox="0 0 24 24"
                stroke="currentColor"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="2"
                  d="M3 3h2l.4 2M7 13h10l4-8H5.4M7 13L5.4 5M7 13l-2.293 2.293c-.63.63-.184 1.707.707 1.707H17m0 0a2 2 0 100 4 2 2 0 000-4zm-8 2a2 2 0 11-4 0 2 2 0 014 0z"
                />
              </svg>
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";
import { products } from "../data/products";
import { useRoute } from "vue-router";

const route = useRoute();
const selectedCategory = ref("");
const selectedPrice = ref("");
const selectedSize = ref("");
const sortBy = ref("default");
const wishlist = ref(new Set());

// 從 URL 查詢參數中獲取搜索關鍵字
const searchQuery = computed(() => route.query.search || "");

const hasActiveFilters = computed(() => {
  return (
    selectedCategory.value ||
    selectedPrice.value ||
    selectedSize.value ||
    searchQuery.value
  );
});

const activeFilters = computed(() => {
  const filters = [];
  if (selectedCategory.value) {
    filters.push({
      type: "category",
      label: `分類: ${selectedCategory.value}`,
    });
  }
  if (selectedPrice.value) {
    filters.push({ type: "price", label: `價格: ${selectedPrice.value}` });
  }
  if (selectedSize.value) {
    filters.push({ type: "size", label: `尺寸: ${selectedSize.value}` });
  }
  if (searchQuery.value) {
    filters.push({ type: "search", label: `搜尋: ${searchQuery.value}` });
  }
  return filters;
});

const filteredProducts = computed(() => {
  let result = [...products];

  // 搜索過濾
  if (searchQuery.value) {
    const query = searchQuery.value.toLowerCase();
    result = result.filter(
      (product) =>
        product.name.toLowerCase().includes(query) ||
        product.description.toLowerCase().includes(query)
    );
  }

  // 分類過濾
  if (selectedCategory.value) {
    result = result.filter(
      (product) => product.category === selectedCategory.value
    );
  }

  // 尺寸過濾
  if (selectedSize.value) {
    result = result.filter((product) =>
      product.sizes.includes(selectedSize.value)
    );
  }

  // 價格過濾
  if (selectedPrice.value) {
    const [min, max] = selectedPrice.value.split("-").map(Number);
    result = result.filter((product) => {
      if (max) {
        return product.price >= min && product.price <= max;
      }
      return product.price >= min;
    });
  }

  // 排序
  switch (sortBy.value) {
    case "price-asc":
      result.sort((a, b) => a.price - b.price);
      break;
    case "price-desc":
      result.sort((a, b) => b.price - a.price);
      break;
    case "name-asc":
      result.sort((a, b) => a.name.localeCompare(b.name));
      break;
    case "name-desc":
      result.sort((a, b) => b.name.localeCompare(a.name));
      break;
  }

  return result;
});

const clearFilter = (type) => {
  switch (type) {
    case "category":
      selectedCategory.value = "";
      break;
    case "price":
      selectedPrice.value = "";
      break;
    case "size":
      selectedSize.value = "";
      break;
  }
};

const clearAllFilters = () => {
  selectedCategory.value = "";
  selectedPrice.value = "";
  selectedSize.value = "";
  sortBy.value = "default";
};

const toggleWishlist = (product) => {
  if (wishlist.value.has(product.id)) {
    wishlist.value.delete(product.id);
  } else {
    wishlist.value.add(product.id);
  }
};

const isInWishlist = (product) => {
  return wishlist.value.has(product.id);
};

const quickAddToCart = (product) => {
  alert("商品已加入購物車！");
};
</script>
