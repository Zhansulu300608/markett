<template>
  <div class="flex h-screen bg-gray-50 overflow-hidden">
    <div
      v-if="sidebarOpen"
      @click="sidebarOpen = false"
      class="fixed inset-0 bg-black/50 z-40 md:hidden"
    />

    <aside
      class="fixed md:static z-50 md:z-auto inset-y-0 left-0 w-64 bg-[#003049] flex flex-col transform transition-transform duration-300
      md:translate-x-0"
      :class="sidebarOpen ? 'translate-x-0' : '-translate-x-full'"
    >
      <div class="p-6 flex items-center space-x-3">
        <div class="w-8 h-8 bg-blue-600 rounded-lg flex items-center justify-center">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-white" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
              d="M20 7l-8-4-8 4m16 6l-8 4m8-4v10l-8 4m0-10L4 13m8-4v10l8 4" />
          </svg>
        </div>
        <span class="font-bold text-lg text-white">Admin</span>
      </div>

      <nav class="flex-1 px-4 space-y-2">
        <NuxtLink to="/admin/dashboard" class="menu-link">
          <span>Dashboard</span>
        </NuxtLink>

        <NuxtLink to="/admin/products" class="menu-link">
          <span>Продукты</span>
        </NuxtLink>

        <NuxtLink to="/admin/category" class="menu-link">
          <span>Категории</span>
        </NuxtLink>

        <NuxtLink to="/profile" class="menu-link">
          <span>Профиль</span>
        </NuxtLink>
      </nav>

      <div class="p-6 border-t border-white/10">
        <NuxtLink to="/" class="text-white text-sm hover:text-blue-300">
          Назад на сайт
        </NuxtLink>
      </div>
    </aside>

    <div class="flex-1 flex flex-col">
      <header class="md:hidden flex items-center justify-between px-4 py-3 bg-white shadow">
        <button @click="sidebarOpen = true">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-gray-800" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
              d="M4 6h16M4 12h16M4 18h16" />
          </svg>
        </button>
     
      </header>

      
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
const sidebarOpen = ref(false)
</script>

<style scoped>
.menu-link {
  @apply flex items-center px-4 py-3 rounded-lg text-white hover:bg-blue-600 transition;
}
</style>
