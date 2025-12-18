<template>
  <AppHeader />
 <SnowEffect />

  <slot />
  <div class="flex flex-col md:flex-row min-h-screen bg-gray-100">

  
    <aside class="w-full md:w-64 bg-slate-800 text-white p-6">
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

  
    <main class="flex-1 bg-white p-6">
      <h1 class="text-2xl font-bold mb-6">Избранное</h1>

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
            alt="product image"
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


  <footer class="bg-[#C1121F] py-16">
    <div class="max-w-7xl mx-auto px-6 text-white grid grid-cols-1 md:grid-cols-4 gap-12">
      <div>
        <img src="/images/logo.png" class="h-10 mb-4" alt="Logo" />
        <p class="font-bold">НАПОЛНЯЕМ ЖИЗНЬ</p>
      </div>
      <div>
        <h3 class="font-bold mb-4">Быстрые ссылки</h3>
        <ul class="space-y-2">
          <li>
            <NuxtLink to="/" class="hover:text-[#003049]">Главная</NuxtLink>
          </li>
          <li>
            <NuxtLink to="/catalog" class="hover:text-[#003049]">Каталог</NuxtLink>
          </li>
          <li>
            <NuxtLink to="/dashbord" class="hover:text-[#003049]">Дашборд</NuxtLink>
          </li>
          <li>
            <NuxtLink to="/contacts" class="hover:text-[#003049]">Контакты</NuxtLink>
          </li>
        </ul>
      </div>

      <div>
        <h3 class="font-bold mb-4">Контакты</h3>
        <p>+7 776 624 82 55</p>
        <p>+7 776 777 77 09</p>
      </div>
      <div>
        <h3 class="text-white font-semibold mb-4">Служба поддержки</h3>
        <p class="text-lg mb-6 hover:text-[#003049] text-white">
          <span class="font-semibold hover:text-[#003049] text-white">7766</span> | звонок бесплатный
        </p>

        <h3 class="text-white font-semibold mb-4">
          Мы в социальных сетях
        </h3>

        <div class="flex flex-wrap gap-3 mb-8">
          <a href="https://twitter.com" target="_blank" rel="noopener" class="w-6 h-6">
            <img src="/images/icons8-phone-50.png" alt="Twitter" class="w-full h-full object-contain" />
          </a>
          <a href="https://twitter.com" target="_blank" rel="noopener" class="w-6 h-6">
            <img src="/images/icons8-whatsapp-logo-50.png" alt="WhatsApp" class="w-full h-full object-contain" />
          </a>
          <a href="https://twitter.com" target="_blank" rel="noopener" class="w-6 h-6">
            <img src="/images/icons8-email-50.png" alt="Email" class="w-full h-full object-contain" />
          </a>
          <a href="https://twitter.com" target="_blank" rel="noopener" class="w-6 h-6">
            <img src="/images/icons8-tiktok-50.png" alt="TikTok" class="w-full h-full object-contain" />
          </a>
          <a href="https://twitter.com" target="_blank" rel="noopener" class="w-6 h-6">
            <img src="/images/icons8-instagram-logo-50.png" alt="Instagram" class="w-full h-full object-contain" />
          </a>
        </div>

        <h3 class="text-white font-semibold mb-2 hover:text-[#003049] text-white">
          Служба доставки 
        </h3>
        <p class="text-lg hover:text-[#003049] text-white">
          <span class="font-semibold">7772</span> | звонок бесплатный
        </p>
      </div>
    </div>
  </footer>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import SnowEffect from '~/components/SnowEffect.vue'
const favorites = ref([])

onMounted(() => {
  const stored = localStorage.getItem('favorites')
  favorites.value = stored ? JSON.parse(stored) : []
})

const removeFavorite = (id) => {
  favorites.value = favorites.value.filter(item => item.id !== id)
  localStorage.setItem('favorites', JSON.stringify(favorites.value))
}
</script>
