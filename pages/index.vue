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
      <div class="min-h-screen">
    <div class="container mx-auto px-4 py-8">
      <Carousel />
    </div>
  </div>
<div class="mx-auto px-4 py-4 max-w-7xl ">

  <div class="relative flex items-center justify-between py-6 px-3 md:px-8 -mt-32">

    
    <h2 class="text-4xl md:text-4xl  font-bold text-[#003049] text-start">
      Каталог скидок
    </h2>

   
    <NuxtLink
      to="/catalog"
      class="flex items-center gap-1 text-[#C1121F] hover:text-pink-700 font-medium transition-colors"
    >
      Смотреть все
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
          d="M9 5l7 7-7 7"
        />
      </svg>
    </NuxtLink>

 
  </div>

    
   <div class="flex gap-2 overflow-x-auto mb-6">
  <button
    v-for="cat in categories"
    :key="cat"
    @click="activeCategory = cat"
    class="px-4 py-2 rounded-full border text-sm whitespace-nowrap transition"
    :class="activeCategory === cat
      ? 'bg-[#C1121F] text-white border-[#C1121F]'
      : 'hover:bg-gray-100'"
  >
    {{ cat }}
  </button>
</div>


    <div v-if="pending" class="text-center py-10">
      Жүктелуде...
    </div>

    <div v-else-if="error" class="text-center text-red-500">
      Қате кетті
    </div>


   <div
  v-else
  class="grid grid-cols-2 md:grid-cols-4 lg:grid-cols-5 gap-4"
>
  <ProductCard
    v-for="item in filteredProducts"
    :key="item.id"
    :product="item"
  />
</div>


  </div>
     <section class="max-w-7xl mx-auto px-6 py-14 relative min-h-[600px]">
    <h2 class="text-4xl font-extrabold mb-12  text-[#003049]">FOODI Chef</h2>

    <div class="relative overflow-hidden">
      <!-- Карточкалар контейнері -->
      <div
        class="flex transition-transform duration-700 ease-in-out gap-6"
        :style="{ transform: `translateX(-${currentIndex * (cardWidth + gap)}px)` }"
        ref="carousel"
      >
        <div
          v-for="(item, index) in items"
          :key="index"
          class="relative rounded-xl flex-shrink-0 bg-gradient-to-br p-10 text-white shadow-lg"
          :class="item.bgColor"
          :style="{ width: cardWidth + 'px', minHeight: '320px' }"
        >
          <!-- Жоғарғы белгі -->
          <span
            class="absolute top-4 left-4 px-5 py-1 text-white font-semibold rounded -rotate-6 shadow-lg"
            :class="item.labelBgColor"
          >
            {{ item.label }}
          </span>

          <!-- Мәтін -->
          <p
            class="mt-24 text-xl font-semibold leading-relaxed transition-opacity transition-transform duration-500"
            :class="currentIndex === index ? 'opacity-100 translate-y-0' : ''"
          >
            {{ item.text }}
          </p>

          <!-- Сурет -->
          <img
            :src="item.image"
            :alt="item.label"
            class="absolute bottom-0 right-0 w-48 rounded-br-xl transition-opacity transition-transform duration-500"
            :class="currentIndex === index ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-8 pointer-events-none'"
            loading="lazy"
          />

          <!-- Ішкі батырма -->
          <button
            aria-label="Next"
            class="absolute bottom-6 left-6 w-14 h-14 border-2 border-white rounded-full flex items-center justify-center text-white hover:bg-white hover:text-current transition"
            @click="next"
          >
            &rarr;
          </button>
        </div>
      </div>

      <!-- Навигация стрелкалары -->
      <button
        class="absolute top-1/2 left-2 -translate-y-1/2 bg-white bg-opacity-80 hover:bg-opacity-100 rounded-full p-3 text-gray-800 shadow-lg transition"
        @click="prev"
        aria-label="Previous"
      >
        &#8592;
      </button>
      <button
        class="absolute top-1/2 right-2 -translate-y-1/2 bg-white bg-opacity-80 hover:bg-opacity-100 rounded-full p-3 text-gray-800 shadow-lg transition"
        @click="next"
        aria-label="Next"
      >
        &#8594;
      </button>
    </div>
  </section>

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
import { ref, onMounted, onBeforeUnmount } from "vue";
import { computed } from "vue";
import Carousel from '~/components/Carousel.vue'
definePageMeta({ name: "glav" })
import { navigateTo } from 'nuxt/app' 

const cardWidth = 390; // px
const gap = 24; // px
const currentIndex = ref(0);

// Келесі карточкаға өту
const next = () => {
  currentIndex.value =
    currentIndex.value < items.value.length - 1 ? currentIndex.value + 1 : 0;
};

// Алдыңғы карточкаға өту
const prev = () => {
  currentIndex.value =
    currentIndex.value > 0 ? currentIndex.value - 1 : items.value.length - 1;
};

// Автоматты айналдыру (3 секунд сайын)
let interval = null;
onMounted(() => {
  interval = setInterval(() => {
    next();
  }, 3000); // 3000ms = 3 секунд
});

onUnmounted(() => {
  clearInterval(interval);
});

const items = ref([
  {
    label: "Кондитерская",
    text: "Симфония вкусов – от шоколада до ягод",
    bgColor: "bg-pink-400",
    labelBgColor: "bg-purple-700",
    image: "/images/cake.png",
  },
  {
    label: "Кафетерий",
    text: "Вкусно не значит дорого",
    bgColor: "bg-orange-500",
    labelBgColor: "bg-pink-600",
    image: "/images/kafet.png",
  },
 
   {
    label: "Пекарня",
    text: "Свежая выпечка на каждый день",
    bgColor: "bg-red-500",
    labelBgColor: "bg-blue-800",
    image: "/images/pekar.png",
  },
   {
    label: "Кулинария",
    text: "Все самое вкусное и сбальансированное",
    bgColor: "bg-violet-500",
    labelBgColor: "bg-green-600",
    image: "/images/kulinar.png",
  },
  {
    label: "Полуфабрикаты",
    text: "Вкусно не значит долго",
    bgColor: "bg-blue-400",
    labelBgColor: "bg-orange-500",
    image: "/images/pekar.png",
  },
   {
    label: "Кондитерская",
    text: "Симфония вкусов – от шоколада до ягод",
    bgColor: "bg-pink-400",
    labelBgColor: "bg-purple-700",
    image: "/images/cake.png",
  },
  {
    label: "Кафетерий",
    text: "Вкусно не значит дорого",
    bgColor: "bg-orange-500",
    labelBgColor: "bg-pink-600",
    image: "/images/kafet.png",
  },
 
   {
    label: "Пекарня",
    text: "Свежая выпечка на каждый день",
    bgColor: "bg-red-500",
    labelBgColor: "bg-blue-800",
    image: "/images/pekar.png",
  },
   {
    label: "Кулинария",
    text: "Все самое вкусное и сбальансированное",
    bgColor: "bg-violet-500",
    labelBgColor: "bg-green-600",
    image: "/images/kulinar.png",
  },
  {
    label: "Полуфабрикаты",
    text: "Вкусно не значит долго",
    bgColor: "bg-blue-400",
    labelBgColor: "bg-orange-500",
    image: "/images/pekar.png",
  },
   {
    label: "Кондитерская",
    text: "Симфония вкусов – от шоколада до ягод",
    bgColor: "bg-pink-400",
    labelBgColor: "bg-purple-700",
    image: "/images/cake.png",
  },
  {
    label: "Кафетерий",
    text: "Вкусно не значит дорого",
    bgColor: "bg-orange-500",
    labelBgColor: "bg-pink-600",
    image: "/images/kafet.png",
  },
 
   {
    label: "Пекарня",
    text: "Свежая выпечка на каждый день",
    bgColor: "bg-red-500",
    labelBgColor: "bg-blue-800",
    image: "/images/pekar.png",
  },
   {
    label: "Кулинария",
    text: "Все самое вкусное и сбальансированное",
    bgColor: "bg-violet-500",
    labelBgColor: "bg-green-600",
    image: "/images/kulinar.png",
  },
  {
    label: "Полуфабрикаты",
    text: "Вкусно не значит долго",
    bgColor: "bg-blue-400",
    labelBgColor: "bg-orange-500",
    image: "/images/pekar.png",
  },
   {
    label: "Кондитерская",
    text: "Симфония вкусов – от шоколада до ягод",
    bgColor: "bg-pink-400",
    labelBgColor: "bg-purple-700",
    image: "/images/cake.png",
  },
  {
    label: "Кафетерий",
    text: "Вкусно не значит дорого",
    bgColor: "bg-orange-500",
    labelBgColor: "bg-pink-600",
    image: "/images/kafet.png",
  },
 
   {
    label: "Пекарня",
    text: "Свежая выпечка на каждый день",
    bgColor: "bg-red-500",
    labelBgColor: "bg-blue-800",
    image: "/images/pekar.png",
  },
   {
    label: "Кулинария",
    text: "Все самое вкусное и сбальансированное",
    bgColor: "bg-violet-500",
    labelBgColor: "bg-green-600",
    image: "/images/kulinar.png",
  },
  {
    label: "Полуфабрикаты",
    text: "Вкусно не значит долго",
    bgColor: "bg-blue-400",
    labelBgColor: "bg-orange-500",
    image: "/images/pekar.png",
  },
   {
    label: "Кондитерская",
    text: "Симфония вкусов – от шоколада до ягод",
    bgColor: "bg-pink-400",
    labelBgColor: "bg-purple-700",
    image: "/images/cake.png",
  },
  {
    label: "Кафетерий",
    text: "Вкусно не значит дорого",
    bgColor: "bg-orange-500",
    labelBgColor: "bg-pink-600",
    image: "/images/kafet.png",
  },
 
   {
    label: "Пекарня",
    text: "Свежая выпечка на каждый день",
    bgColor: "bg-red-500",
    labelBgColor: "bg-blue-800",
    image: "/images/pekar.png",
  },
   {
    label: "Кулинария",
    text: "Все самое вкусное и сбальансированное",
    bgColor: "bg-violet-500",
    labelBgColor: "bg-green-600",
    image: "/images/kulinar.png",
  },
  {
    label: "Полуфабрикаты",
    text: "Вкусно не значит долго",
    bgColor: "bg-blue-400",
    labelBgColor: "bg-orange-500",
    image: "/images/pekar.png",
  },
]);

const activeCategory = ref('Барлығы')

const categoryMap = {
  'Фрукты, овощи': [29, 30, 31, 32],
  'Мясо': [27, 28],
  'Гастрономия': [34, 58, 59, 81, 92],
  'Безалкогольные напитки': [36, 37, 38, 39, 41, 65, 72, 79],
  'Кондитерские изделия': [46, 47, 48, 49, 50, 62, 69, 70, 73, 76],
  'Бакалея': [45, 61, 64, 67, 91, 94, 95],
  'Молочные продукты': [33, 35, 40, 57, 60, 68, 71, 74],
  'Собственное производство': [44],
  'Чай': [51, 52, 63, 66],
  'Консервы': [64, 77, 92],
  'Замороженные продукты': [58],
  'Бытовая химия': [42, 43, 83, 88, 96],
  'Средства гигиены': [54, 55, 56, 75, 78, 93, 100],
  'Товары для дома': Array.from({ length: 17 }, (_, i) => i + 1),
  'Детские товары': Array.from({ length: 9 }, (_, i) => i + 18)
}

const { data: products, pending, error } = await useFetch(
  'https://67cbeea23395520e6af6ab52.mockapi.io/categorysale'
)

const filteredProducts = computed(() => {
  if (!products.value) return []

  // Барлығы → тек 8 товар
  if (activeCategory.value === 'Барлығы') {
    return products.value.slice(0, 10)
  }

  const allowedIds = categoryMap[activeCategory.value]

  if (!allowedIds) return []

  return products.value.filter(item =>
    allowedIds.includes(Number(item.id))
  )
})
const categories = [
  'Барлығы',
  'Фрукты, овощи',
  'Мясо',
  'Гастрономия',
  'Безалкогольные напитки',
  'Кондитерские изделия',
  'Бакалея',
  'Молочные продукты',
  'Собственное производство',
  'Чай',
  'Консервы',
  'Замороженные продукты',
  'Бытовая химия',
  'Средства гигиены',
  'Товары для дома',
  'Детские товары'
]

</script>


<style scoped>
@keyframes float-0 { 0%, 100% { transform: translateY(0); } 50% { transform: translateY(-5px); } }
@keyframes float-1 { 0%, 100% { transform: translateY(0); } 50% { transform: translateY(-8px); } }
@keyframes float-2 { 0%, 100% { transform: translateY(0); } 50% { transform: translateY(-6px); } }

</style>
