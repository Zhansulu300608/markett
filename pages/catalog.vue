<template>
  <!-- HEADER -->
  <header
    class="w-full bg-[url('/images/newYear.3940986.png')] bg-contain bg-no-repeat bg-center h-40 text-white shadow-md relative overflow-hidden"
  >
    <div class="max-w-7xl mx-auto px-4 py-4 flex items-center justify-between">
      <!-- Left -->
      <div class="flex items-center space-x-6">
        <div class="flex items-center space-x-2 text-[#003049]">
          <img src="/images/logo.png" alt="Logo" class="h-10" />
        </div>
      </div>

      <!-- Search -->
      <div class="flex-1 mx-10 mt-5">
        <div class="flex rounded-full overflow-hidden shadow-sm">
          <input
            type="text"
            placeholder="Поиск"
            class="flex-1 px-4 py-2 text-gray-700 focus:outline-none"
          />
          <button class="px-4 text-white bg-[#003049] border-l">
            по скидкам
          </button>
        </div>
      </div>

      <!-- Actions -->
      <div class="flex items-center space-x-4">
        <button class="bg-white px-4 py-2 rounded-full shadow">
          <NuxtLink to="/login" class="text-[#003049]">Login</NuxtLink>
        </button>
        <button class="bg-white px-4 py-2 rounded-full shadow text-gray-800">
          RU
        </button>
      </div>
    </div>

    <!-- NAV -->
    <nav class="text-gray-800 py-4 mt-5 shadow-md">
      <div class="max-w-7xl mx-auto px-4 flex space-x-8 text-sm font-medium">
        <NuxtLink to="/" class="text-[#003049] hover:text-[#C1121F]">
          Главная
        </NuxtLink>
        <NuxtLink to="/catalog" class="text-[#003049] hover:text-[#C1121F]">
          Каталог
        </NuxtLink>
        
        <NuxtLink to="/profile" class="text-[#003049] hover:text-[#C1121F]">
          Профиль
        </NuxtLink>
        <NuxtLink :to="{ name: 'profile' }" class="hover:text-[#C1121F] text-[#003049]">
         Дашборд
        </NuxtLink>
        <NuxtLink to="/contact" class="text-[#003049] hover:text-[#C1121F]">
          Контакты
        </NuxtLink>
      </div>
    </nav>
  </header>

  <!-- CONTENT -->
  <main class="min-h-screen bg-white px-4 md:px-8 py-6">
    <div class="mb-6">
      <h1 class="text-3xl font-bold">Каталог скидок</h1>
      <p class="text-sm text-gray-600 mt-2">
        Все скидки Magnum в одном месте!
      </p>
    </div>

    <div class="grid grid-cols-1 md:grid-cols-[240px_1fr] gap-6">
      <!-- Categories -->
      <aside class="space-y-3">
        <div
          v-for="(category, i) in categories"
          :key="i"
          @click="activeCategory = category.name"
          :class="[
            'flex items-center gap-3 rounded-xl p-4 cursor-pointer',
            activeCategory === category.name ? 'bg-gray-300' : 'bg-gray-100 hover:bg-gray-200' 
          ] " 
        >
          <span class="text-sm font-medium">{{ category.name }}</span>
          <img
            :src="category.image"
            :alt="category.name"
            class="w-16 h-16 object-contain"
          />
        </div>
      </aside>

      <!-- Products -->
      <section class="grid grid-cols-2 md:grid-cols-4 gap-3">
    <div
  v-for="product in filteredProducts"
  :key="product.id"
  @click="openModal(product)"
  class="border rounded-2xl p-4 relative flex flex-col h-[340px] cursor-pointer"
>

  <span
    v-if="discountPercent(product)"
    class="absolute top-3 left-3 bg-[#C1121F] text-white text-sm px-3 py-1 rounded-full"
  >
    -{{ discountPercent(product) }}%
  </span>

  <p class="absolute top-3 right-3 text-xs text-gray-400">
    до {{ formatDate(product.action_end) }}
  </p>

  <img
    :src="product.image"
    :alt="product.name"
    class="h-36 w-full object-contain bg-gray-100 rounded-xl mb-3"
  />

  <h3 class="text-sm font-semibold line-clamp-2 min-h-[40px]">
    {{ product.name }}
  </h3>

  <div class="mt-auto pt-3">
    <p class="text-xs line-through text-gray-400">
      {{ product.start_price }} тг
    </p>
    <p class="bg-yellow-400 px-4 py-2 rounded-xl font-bold text-lg inline-block">
      {{ product.final_price }} тг
    </p>
  </div>
</div>

      </section>
    </div>
    <!-- MODAL -->
<div
  v-if="showModal"
  class="fixed inset-0 bg-black/40 z-50 flex items-center justify-center"
>
  <div class="bg-white rounded-3xl w-full max-w-3xl p-6 relative">

    <!-- Close -->
    <button
      @click="closeModal"
      class="absolute top-4 right-4 text-xl text-gray-400 hover:text-black"
    >
      ✕
    </button>

    <div class="grid grid-cols-1 md:grid-cols-2 gap-6">

      <!-- Image -->
      <img
        :src="selectedProduct.image"
        class="w-full h-64 object-contain bg-gray-100 rounded-xl"
      />

      <!-- Info -->
      <div class="flex flex-col">
        <span class="bg-pink-600 text-white text-sm px-3 py-1 rounded-full w-fit mb-2">
          -{{ discountPercent(selectedProduct) }}%
        </span>

        <p class="text-xs text-gray-500 mb-2">
          действует до {{ formatDate(selectedProduct.action_end) }}
        </p>

        <h2 class="text-xl font-bold mb-4">
          {{ selectedProduct.name }}
        </h2>

        <p class="text-sm text-gray-600 mb-4">
          Акция действует при наличии товара на полке.
        </p>

        <div class="mb-6">
          <p class="text-sm line-through text-gray-400">
            {{ selectedProduct.start_price }} тг
          </p>
          <p class="text-2xl font-bold bg-yellow-400 inline-block px-4 py-2 rounded-xl">
            {{ selectedProduct.final_price }} тг
          </p>
        </div>

        <!-- BUTTON -->
        <button
          @click="addToOrders(selectedProduct)"
          class="mt-auto bg-[#003049] text-white py-3 rounded-xl hover:bg-[#00263a]"
        >
          Мои заказы
        </button>
      </div>
    </div>
  </div>
</div>

  </main>

  <!-- FOOTER -->
  <footer class="bg-[#C1121F] py-16">
    <div class="max-w-7xl mx-auto px-6 text-white grid grid-cols-1 md:grid-cols-4 gap-12">
      <div>
        <img src="/images/logo.png" class="h-10 mb-4" />
        <p class="font-bold">НАПОЛНЯЕМ ЖИЗНЬ</p>
      </div>
      <div>
        <h3 class="font-bold mb-4">Быстрые ссылки</h3>
        <ul class="space-y-2">
          <li>Каталог скидок</li>
          <li>Доставка</li>
          <li>Контакты</li>
        </ul>
      </div>
      <div>
        <h3 class="font-bold mb-4">Контакты</h3>
        <p>+7 776 624 82 55</p>
      </div>
      <div>
        <h3 class="font-bold mb-4">Поддержка</h3>
        <p>7766 | бесплатно</p>
      </div>
    </div>
  </footer>
</template>
<script setup>import { ref, computed } from 'vue'
import { useFetch } from '#app' // Nuxt 3 үшін

const activeCategory = ref('Барлығы')
const categories = [
   { name: "Кондитерские изделия", image: "https://magnum.kz:1337/uploads/konditerskie_izdeliya_590a0ea83b.png" },
    { name: "Товары для дома", image: "https://magnum.kz:1337/uploads/Tovary_dlya_doma_c55163f947.png" },
    { name: "Детские товары", image: "https://magnum.kz:1337/uploads/Detskie_tovary_5ada265732.png" },
     { name: "Бакалея", image: "https://magnum.kz:1337/uploads/bakaleya_3110e86aae.png" },
  { name: "Молочные продукты", image: "https://magnum.kz:1337/uploads/Molochnye_produkty_0fc46bfbd0.png" },
  { name: "Фрукты, овощи", image: "https://magnum.kz:1337/uploads/Frutky_i_ovoshhi_35bebc3cdc.png" },
  { name: "Мясо", image: "https://magnum.kz:1337/uploads/myaso_24f87a2b63.png" },
  { name: "Гастрономия", image: "https://magnum.kz:1337/uploads/Gastronomiya_785ab8a5c8.png" },
  { name: "Безалкогольные напитки", image: "https://magnum.kz:1337/uploads/Bezalkogolnye_napitki_804131c63d.png" },
  { name: "Чай, Кофе, Какао", image: "https://magnum.kz:1337/uploads/Chaj_kofe_kakao_6013945351.png" },
  { name: "Консервы", image: "https://magnum.kz:1337/uploads/konservy_fe0bd212c3.png" },
    { name: "Средства гигиены", image: "https://magnum.kz:1337/uploads/Sredstva_gigieny_75f280ceb3.png" },
  { name: "Бытовая химия", image: "https://magnum.kz:1337/uploads/Bytovaya_himiya_4e6942dd39.png" },
  { name: "Другие товары", image: "https://magnum.kz:1337/uploads/Drugie_tovary_d07556ae4d.png" },
]
// Екі API-дан мәліметтерді жүктейміз
const { data: data1, pending: pending1, error: error1 } = await useFetch('https://67cbeea23395520e6af6ab52.mockapi.io/categorysale')
const { data: data2, pending: pending2, error: error2 } = await useFetch('https://6940519c993d68afba6bb782.mockapi.io/market')

// Барлық өнімдер
const products = computed(() => {
  if (!data1.value && !data2.value) return []
  return [...(data1.value || []), ...(data2.value || [])]
})

// pending және error біріктіру
const pending = computed(() => pending1.value || pending2.value)
const error = computed(() => error1.value || error2.value)

// Ескі categoryMap және productsByCategory, егер фильтрация керек болса
const categoryMap = {
  'Фрукты, овощи': [29, 30, 31, 32],
  'Мясо': [27, 28],
  'Гастрономия': [34, 58, 59, 81, 92],
  'Безалкогольные напитки': [36, 37, 38, 39, 41, 65, 72, 79],
  'Кондитерские изделия': [46, 47, 48, 49, 50, 62, 69, 70, 73, 76, 80, 82, 84, 85, 86, 87, 89, 90, 97, 98, 99],
  'Бакалея': [45, 61, 64, 67, 91, 94, 95],
  'Молочные продукты': [33, 35, 40, 57, 60, 68, 71, 74],
  'Собственное производство': [44],
  'Чай, Кофе, Какао': [51, 52, 63, 66],
  'Консервы': [64, 77, 92],
  'Замороженные продукты': [58],
  'Бытовая химия': [42, 43, 83, 88, 96],
  'Средства гигиены': [54, 55, 56, 75, 78, 93, 100],
  'Товары для дома': Array.from({ length: 17 }, (_, i) => i + 1),
  'Детские товары': Array.from({ length: 9 }, (_, i) => i + 18),
}

const productsByCategory = {
  'Фрукты, овощи': [114, 115, 116],
  'Мясо': [112, 119],
  'Гастрономия': [117, 118, 113],
  'Безалкогольные напитки': [300, 600, 601],
  'Кондитерские изделия': [306, 307, 416],
  'Бакалея': [411, 412, 413],
  'Молочные продукты': [204, 503, 400],
  'Собственное производство': [110],
  'Чай, Кофе, Какао': [201, 207, 508],
  'Консервы': [304, 305, 209],
  'Замороженные продукты': [],
  'Бытовая химия': [203, 308, 603],
  'Средства гигиены': [202, 415, 605],
  'Товары для дома': [109, 602, 604],
  'Детские товары': [416],
  'Другие товары': [101, 102, 103, 104, 105, 106, 107, 108, 109, 110, 111]
}

// Фильтрация қосылған filteredProducts
const filteredProducts = computed(() => {
  if (!products.value) return []

  // Егер 'Барлығы' болса — барлық өнімді қайтару (фильтрсіз)
  if (activeCategory.value === 'Барлығы') {
    return products.value
  }

  // Әйтпесе, екі картадан да ID-ларды аламыз
  const idsFromCategoryMap = categoryMap[activeCategory.value] || []
  const idsFromProductsByCategory = productsByCategory[activeCategory.value] || []

  // Біріккен, қайталанбайтын ID жиыны
  const allowedIdsSet = new Set([...idsFromCategoryMap, ...idsFromProductsByCategory])

  // Тауарларды сүзу
  return products.value.filter(product =>
    allowedIdsSet.has(Number(product.id))
  )
})

const discountPercent = (product) => {
  if (!product.start_price || !product.final_price) return 0
  return Math.round(((product.start_price - product.final_price) / product.start_price) * 100)
}

const formatDate = (dateStr) => {
  if (!dateStr) return ''
  const date = new Date(dateStr)
  return date.toLocaleDateString('ru-RU', { day: 'numeric', month: 'short' })
}
const showModal = ref(false)
const selectedProduct = ref(null)

const openModal = (product) => {
  selectedProduct.value = product
  showModal.value = true
}

const closeModal = () => {
  showModal.value = false
  selectedProduct.value = null
}

const addToOrders = (product) => {
  const orders = JSON.parse(localStorage.getItem('orders') || '[]')

  // duplicate болмас үшін
  const exists = orders.find(item => item.id === product.id)
  if (!exists) {
    orders.push(product)
    localStorage.setItem('orders', JSON.stringify(orders))
  }

  closeModal()
  navigateTo('/order')
}

</script>
