<template>
  <AppHeader />

  <div class="flex min-h-screen bg-gray-100">

    <!-- SIDEBAR -->
    <aside class="w-64 bg-slate-800 text-white p-6">
      <nav class="space-y-4">
        <NuxtLink to="/dashbord" class="block hover:bg-slate-700 px-4 py-2 rounded">
          Dashboard
        </NuxtLink>

        <NuxtLink to="/order" class="block hover:bg-slate-700 px-4 py-2 rounded">
          Мои заказы
        </NuxtLink>

        <NuxtLink to="/favourties" class="block hover:bg-slate-700 px-4 py-2 rounded">
          Избранное 
        </NuxtLink>
      </nav>

      <button class="mt-10 w-full bg-red-500 py-2 rounded">
        Logout
      </button>
    </aside>

    <!-- CONTENT -->
    <main class="flex-1 bg-white p-6">
      <h1 class="text-2xl font-bold mb-6">Избранное ❤️</h1>

      <div v-if="favorites.length === 0" class="text-gray-400">
        Избранных товаров пока нет
      </div>

      <div v-else class="grid grid-cols-2 md:grid-cols-4 gap-4">
        <div
          v-for="product in favorites"
          :key="product.id"
          class="border rounded-2xl p-4 flex flex-col"
        >
          <img
            :src="product.image"
            class="h-32 object-contain bg-gray-100 rounded-xl mb-3"
          />

          <h3 class="text-sm font-semibold mb-2 line-clamp-2">
            {{ product.name }}
          </h3>

          <p class="text-xs line-through text-gray-400">
            {{ product.start_price }} тг
          </p>

          <p class="font-bold bg-yellow-400 inline-block px-3 py-1 rounded-xl">
            {{ product.final_price }} тг
          </p>
<button
  @click="removeFavorite(product.id)"
  class="mt-2 flex items-center justify-center gap-2
         bg-red-500 text-white text-sm font-medium
         py-2 rounded-xl
         hover:bg-red-600
         transition-all duration-200"
>
  <span>Удалить</span>

</button>

        </div>
      </div>
    </main>

  </div>
</template>
<script setup>
import { ref, onMounted } from 'vue'

const favorites = ref([])

onMounted(() => {
  favorites.value = JSON.parse(localStorage.getItem('favorites') || '[]')
})

const removeFavorite = (id) => {
  favorites.value = favorites.value.filter(item => item.id !== id)
  localStorage.setItem('favorites', JSON.stringify(favorites.value))
}
</script>
