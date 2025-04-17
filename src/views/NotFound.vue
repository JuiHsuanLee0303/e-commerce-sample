<template>
  <div
    class="min-h-screen bg-gray-50 flex items-center justify-center py-12 px-4"
  >
    <div class="max-w-md w-full text-center">
      <h1 class="text-9xl font-bold text-gray-900 mb-4">404</h1>
      <p class="text-xl text-gray-600 mb-8">很抱歉，找不到該頁面</p>
      <router-link
        to="/"
        class="inline-block bg-black text-white px-6 py-3 rounded-lg hover:bg-gray-800 transition"
      >
        返回首頁
      </router-link>
    </div>
  </div>
</template>
