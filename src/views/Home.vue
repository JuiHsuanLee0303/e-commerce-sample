<template>
  <div class="min-h-screen">
    <!-- Hero Section -->
    <div class="relative h-[600px] bg-gray-100">
      <div class="absolute inset-0 bg-black/30"></div>
      <div class="relative h-full flex items-center justify-center text-center">
        <div class="max-w-3xl px-4">
          <h1 class="text-5xl font-bold text-white mb-4">時尚新生活</h1>
          <p class="text-xl text-white mb-8">探索最新時尚單品，打造專屬風格</p>
          <router-link
            to="/products"
            class="bg-white text-black px-8 py-3 rounded-full hover:bg-gray-100 transition"
          >
            立即選購
          </router-link>
        </div>
      </div>
    </div>

    <!-- Featured Categories -->
    <div class="max-w-7xl mx-auto px-4 py-16">
      <h2 class="text-3xl font-bold text-center mb-12">熱門分類</h2>
      <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
        <div class="group relative h-80 overflow-hidden rounded-lg">
          <img
            src="https://images.unsplash.com/photo-1434389677669-e08b4cac3105?ixlib=rb-1.2.1&auto=format&fit=crop&w=800&q=80"
            alt="上衣"
            class="w-full h-full object-cover group-hover:scale-110 transition duration-300"
          />
          <div
            class="absolute inset-0 bg-black/20 group-hover:bg-black/40 transition"
          ></div>
          <div class="absolute bottom-0 left-0 right-0 p-6">
            <h3 class="text-2xl font-bold text-white">上衣</h3>
          </div>
        </div>
        <div class="group relative h-80 overflow-hidden rounded-lg">
          <img
            src="https://images.unsplash.com/photo-1475180098004-ca77a66827be?ixlib=rb-1.2.1&auto=format&fit=crop&w=800&q=80"
            alt="褲裝"
            class="w-full h-full object-cover group-hover:scale-110 transition duration-300"
          />
          <div
            class="absolute inset-0 bg-black/20 group-hover:bg-black/40 transition"
          ></div>
          <div class="absolute bottom-0 left-0 right-0 p-6">
            <h3 class="text-2xl font-bold text-white">褲裝</h3>
          </div>
        </div>
        <div class="group relative h-80 overflow-hidden rounded-lg">
          <img
            src="https://images.unsplash.com/photo-1583496661160-fb5886a0aaaa?ixlib=rb-1.2.1&auto=format&fit=crop&w=800&q=80"
            alt="裙裝"
            class="w-full h-full object-cover group-hover:scale-110 transition duration-300"
          />
          <div
            class="absolute inset-0 bg-black/20 group-hover:bg-black/40 transition"
          ></div>
          <div class="absolute bottom-0 left-0 right-0 p-6">
            <h3 class="text-2xl font-bold text-white">裙裝</h3>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup></script>
