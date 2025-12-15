<template>
     <header class="w-full bg-[url('/images/newYear.3940986.png')] bg-contain bg-no-repeat
  bg-center h-40 text-white shadow-md relative overflow-hidden">
    <div class="max-w-7xl mx-auto px-4 py-4 flex items-center justify-between">
      <!-- Left Section -->
      <div class="flex items-center space-x-6">
        <!-- Burger Menu -->
        <!-- <button class="text-2xl">
          <span class="material-icons">menu</span>
        </button> -->

        <!-- Logo -->
        <div class="flex items-center space-x-2 text-[#003049]">
          <img src="../public/images/logo.png" alt="Logo" class="h-10" />
         
        </div>
      </div>

      <!-- Search Bar -->
      <div class="flex-1 mx-10 mt-5">
        <div class="flex rounded-full overflow-hidden shadow-sm">
          <input
            type="text"
            placeholder="Поиск"
            class="flex-1 px-4 py-2 text-gray-700 focus:outline-none"
          />
           <button class="px-4 text-white bg-[#003049] border-l">по скидкам</button>
        </div>
      </div>

      <!-- Location / Lang -->
      <div class="flex items-center space-x-4">
        <button class="bg-white text-gray-800 px-4 py-2 rounded-full flex items-center space-x-2 shadow">
        <NuxtLink to="/login" class="login-btn text-[#003049]">Login</NuxtLink>
        </button>
        <button class="bg-white text-gray-800 px-4 py-2 rounded-full flex items-center space-x-2 shadow">
          <span>RU</span>
        </button>
      </div>
    </div>

    <!-- Bottom Nav -->
    <nav class=" text-gray-800 py-4 mt-5 shadow-md">
      <div class="max-w-7xl mx-auto px-4 flex space-x-8 text-sm font-medium">
           <NuxtLink :to="{ name: 'glav' }" class="hover:text-[#C1121F] text-[#003049]">
         Главная
        </NuxtLink>
          <NuxtLink :to="{ name: 'catalog' }" class="hover:text-[#C1121F] text-[#003049]">
         Каталог
        </NuxtLink>
        <a href="#" class="hover:text-[#C1121F] text-[#003049]">Доставка</a>
         <NuxtLink :to="{ name: 'profile' }" class="hover:text-[#C1121F] text-[#003049]">
         Профиль
        </NuxtLink>
         <a href="#" class="hover:text-[#C1121F] text-[#003049]">О компании</a>
        <NuxtLink :to="{ name: 'contact' }" class="hover:text-[#C1121F] text-[#003049]">
         Контакты
        </NuxtLink>
      </div>
    </nav>
  </header>
  <div class="min-h-screen bg-white px-4 md:px-8 py-6">
  
    <div class="mb-6">
      <h1 class="text-2xl md:text-3xl font-bold text-gray-900">Каталог скидок</h1>
      <p class="text-sm text-gray-600 mt-2 max-w-xl">
        Все скидки Magnum в одном месте! Ежедневная экономия в любимом магазине.
      </p>
    </div>

  
    <!-- Info Bar -->
    <div class="flex flex-wrap items-center justify-between gap-4 mb-6">
      <p class="text-sm text-gray-700">Категории товаров: <span class="font-semibold">186 товаров</span></p>
      <div class="flex gap-3">
       
      </div>
    </div>

    <div class="grid grid-cols-1 md:grid-cols-[240px_1fr] gap-6">
      <!-- Categories -->
  <aside class="space-y-3">
  <div v-for="(category, index) in [...categories, {name:'Другие товары'}]" :key="index">
    <h4 class="font-bold mb-2">{{ category.name }}</h4>
    <div class="grid grid-cols-2 gap-3">
      <div v-for="product in productsByCategory(category)" :key="product.id" class="border rounded-xl p-3">
        <img :src="product.image" :alt="product.name" class="w-full h-24 object-contain mb-2" />
        <h5 class="text-sm font-semibold">{{ product.name }}</h5>
        <p class="text-xs line-through text-gray-400">{{ product.start_price }} тг</p>
        <p class="text-lg font-bold bg-yellow-400 inline-block px-2 rounded">{{ product.final_price }} тг</p>
      </div>
    </div>
  </div>
</aside>


      <!-- Products -->
     <div class="grid grid-cols-2 md:grid-cols-4 gap-5">
  <div
    v-for="product in products"
    :key="product.id"
    class="border rounded-2xl p-4 relative flex flex-col bg-white"
  >
    <!-- Discount -->
    <span
      v-if="discountPercent(product)"
      class="absolute top-3 left-3 bg-pink-600 text-white text-sm px-3 py-1 rounded-full"
    >
      -{{ discountPercent(product) }}%
    </span>

    <!-- Date -->
    <p class="absolute top-3 right-3 text-xs text-gray-400">
      до {{ formatDate(product.action_end) }}
    </p>

    <!-- Image -->
    <img
      :src="product.image"
      :alt="product.name"
      class="h-40 w-full object-contain bg-gray-100 rounded-xl mb-4"
    />

    <!-- Title -->
    <h3 class="text-sm font-semibold mb-2 line-clamp-2">
      {{ product.name }}
    </h3>

    <!-- Price -->
    <div class="mt-auto">
      <p class="text-xs line-through text-gray-400">
        {{ product.start_price }} тг
      </p>
      <p class="bg-yellow-400 inline-block px-4 py-2 rounded-xl font-bold text-lg">
        {{ product.final_price }} тг
      </p>
    </div>
  </div>
</div>

    </div>
  </div>

  <footer class="relative bg-[#C1121F] ">

    <div class="absolute inset-0 pointer-events-none opacity-40  [background-size:20px_20px]"></div>

    <div class="relative max-w-7xl mx-auto px-6 py-16">
    
      <div class="mb-12">
       <div class="flex items-center space-x-2 text-[#003049]">
          <img src="../public/images/logo.png" alt="Logo" class="h-10" />
         
        </div>
        <div class="text-white font-bold tracking-wide mt-2">
          НАПОЛНЯЕМ ЖИЗНЬ
        </div>
      </div>

      <!-- Footer grid -->
      <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-12 text-gray-800">
        <!-- Покупателям -->
        <div>
          <h3 class="text-white font-bold mb-4 fs-4">Быстрые ссылки</h3>
          <ul class="space-y-3 text-[#003049]">
            <li class="hover:text-[#003049] text-white">Каталог скидок</li>
            <li class="hover:text-[#003049] text-white">Foodi Chef</li>
            <li class="hover:text-[#003049] text-white">Доставка</li>
            <li class="hover:text-[#003049] text-white">Контакты</li>
          </ul>
        </div>

        <!-- О компании -->
        <div>
          <h3 class="text-white font-semibold mb-4">Быстрые ссылки</h3>
          <ul class="space-y-3 text-[#003049]">
            <li class="hover:text-[#003049] text-white">Магазин Foodi</li>
             <li class="hover:text-[#003049] text-white">Корзина</li>
            <li class="hover:text-[#003049] text-white">Профиль</li>
          </ul>
        </div>

        <!-- Сотрудничество -->
        <div>
          <h3 class="text-white font-semibold mb-4">Свяжитесь с нами</h3>
          <ul class="space-y-3">
            <li class="hover:text-[#003049] text-white"><a href="https://twitter.com" target="_blank" rel="noopener" class="w-6 h-6">+7 7766248255</a></li>
            <li class="hover:text-[#003049] text-white">Рекламодателям</li>
            <li class="hover:text-[#003049] text-white">Арендаторам</li>
          </ul>
        </div>

        <!-- Support & social -->
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
    <img src="/images/icons8-whatsapp-logo-50.png" alt="Twitter" class="w-full h-full object-contain" />
  </a>
<a href="https://twitter.com" target="_blank" rel="noopener" class="w-6 h-6">
    <img src="/images/icons8-email-50.png" alt="Twitter" class="w-full h-full object-contain" />
  </a>
  <a href="https://twitter.com" target="_blank" rel="noopener" class="w-6 h-6">
    <img src="/images/icons8-tiktok-50.png" alt="Twitter" class="w-full h-full object-contain" />
  </a>
    <a href="https://twitter.com" target="_blank" rel="noopener" class="w-6 h-6">
    <img src="/images/icons8-instagram-logo-50.png" alt="Twitter" class="w-full h-full object-contain" />
  </a>

  <a href="https://instagram.com" target="_blank" rel="noopener" class="w-6 h-6">
   
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
    </div>
  </footer>
  
</template>

<script setup>
const categories = [
  { name: "Фрукты, овощи", ids: [29, 30, 31, 32] , image: "https://magnum.kz:1337/uploads/Frutky_i_ovoshhi_35bebc3cdc.png" },
  { name: "Мясо",ids: [27, 28] , image: "https://magnum.kz:1337/uploads/myaso_24f87a2b63.png" },
  { name: "Гастрономия", ids: [34, 58, 59, 81, 92], image: "https://magnum.kz:1337/uploads/Gastronomiya_785ab8a5c8.png" },
  { name: "Безалкогольные напитки",ids: [36, 37, 38, 39, 41, 65, 72, 79] , image: "https://magnum.kz:1337/uploads/Bezalkogolnye_napitki_804131c63d.png" },
  { name: "Кондитерские изделия",ids: [46, 47, 48, 49, 50, 62, 69, 70, 73, 76, 80, 82, 84, 85, 86, 87, 89, 90, 97, 98, 99] , image: "https://magnum.kz:1337/uploads/konditerskie_izdeliya_590a0ea83b.png" },
  { name: "Бакалея",ids: [45, 61, 64, 67, 91, 94, 95], image: "https://magnum.kz:1337/uploads/bakaleya_3110e86aae.png" },
  { name: "Молочные продукты", ids: [33, 35, 40, 57, 60, 68, 71, 74] ,image: "https://magnum.kz:1337/uploads/Molochnye_produkty_0fc46bfbd0.png" },
  { name: "Собственное производство",ids: [44], image: "https://magnum.kz:1337/uploads/Sobstvennoe_proizvodstvo_432b6600ca.png" },
  { name: "Чай, Кофе, Какао",ids: [51, 52, 63, 66], image: "https://magnum.kz:1337/uploads/Chaj_kofe_kakao_6013945351.png" },
  { name: "Консервы", ids: [64, 77, 92],image: "https://magnum.kz:1337/uploads/konservy_fe0bd212c3.png" },
  { name: "Замороженные продукты",ids: [58], image: "https://magnum.kz:1337/uploads/Zamorozhennye_produkty_ca04644ef7.png" },
  { name: "Бытовая химия",ids: [42, 43, 83, 88, 96], image: "https://magnum.kz:1337/uploads/Bytovaya_himiya_4e6942dd39.png" },
  { name: "Средства гигиены",ids: [54, 55, 56, 75, 78, 93, 100], image: "https://magnum.kz:1337/uploads/Sredstva_gigieny_75f280ceb3.png" },
  { name: "Товары для дома",ids: Array.from({length: 17}, (_, i) => i + 1), image: "https://magnum.kz:1337/uploads/Tovary_dlya_doma_c55163f947.png" },
  { name: "Детские товары", ids: Array.from({length: 9}, (_, i) => i + 18) ,image: "https://magnum.kz:1337/uploads/Detskie_tovary_5ada265732.png" },
];



// fetch products
const { data: products } = await useFetch(
  'https://67cbeea23395520e6af6ab52.mockapi.io/categorysale',
  { default: () => [] }
)

// функция: категория бойынша фильтр
const productsByCategory = (category) => {
  const catProducts = products.value.filter(p => category.ids.includes(p.id))
  const otherProducts = products.value.filter(p => !categories.flatMap(c => c.ids).includes(p.id))
  if(category.name === "Другие товары") return otherProducts
  return catProducts
}

// қосымша функциялар
const discountPercent = (product) => {
  if (!product.start_price || !product.final_price) return null
  return Math.round(((product.start_price - product.final_price) / product.start_price) * 100)
}

const formatDate = (date) => {
  const d = new Date(date)
  return `${String(d.getDate()).padStart(2,'0')}.${String(d.getMonth()+1).padStart(2,'0')}`
}






</script>
