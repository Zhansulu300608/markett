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
          <button class="px-4 text-gray-600 border-l">по скидкам</button>
        </div>
      </div>

      <!-- Location / Lang -->
      <div class="flex items-center space-x-4">
        <button class="bg-white text-gray-800 px-4 py-2 rounded-full flex items-center space-x-2 shadow">
        <NuxtLink to="/login" class="login-btn">Login</NuxtLink>
        </button>
        <button class="bg-white text-gray-800 px-4 py-2 rounded-full flex items-center space-x-2 shadow">
          <span>RU</span>
        </button>
      </div>
    </div>

    <!-- Bottom Nav -->
    <nav class=" text-gray-800 py-4 mt-5 shadow-md">
      <div class="max-w-7xl mx-auto px-4 flex space-x-8 text-sm font-medium">
         <a href="#" class="hover:text-[#C1121F]">Главная</a>
        <a href="#" class="hover:text-[#C1121F]">Каталог</a>
        <a href="#" class="hover:text-[#C1121F]">Доставка</a>
        <a href="#" class="hover:text-[#C1121F]">Профиль</a>
         <a href="#" class="hover:text-[#C1121F]">О компании</a>
        <a href="#" class="hover:text-[#C1121F]">Контакты</a>
      </div>
    </nav>
  </header>

  <div data-v-9aedfe68="" class="home-banner__hashtags"><a data-v-9aedfe68="" href="https://magnum.kz/stocks/milka" target="_blank" class="home-banner__hashtags-btn">
        #Выиграй автомобиль с Milka
      </a><a data-v-9aedfe68="" href="https://magnum.kz/stocks/podarok-barni" target="_blank" class="home-banner__hashtags-btn">
        #Подарок от Barni
      </a><a data-v-9aedfe68="" href="https://partyclub.kz/ru/stocks/105" target="_blank" class="home-banner__hashtags-btn">
        #+21 Бонусы от Reyka
      </a></div>
      <div class="min-h-screen bg-gray-50">
    <div class="container mx-auto px-4 py-8">
      <Carousel />
    </div>
  </div>
</template>

<script setup>
import Carousel from '~/components/Carousel.vue'
const hashtags = [
  '#Выиграй автомобиль с Milka',
  '#Подарок от Barni',
  '#+21 Бонусы от Reyka'
]
</script>

<style scoped>
@keyframes float-0 { 0%, 100% { transform: translateY(0); } 50% { transform: translateY(-5px); } }
@keyframes float-1 { 0%, 100% { transform: translateY(0); } 50% { transform: translateY(-8px); } }
@keyframes float-2 { 0%, 100% { transform: translateY(0); } 50% { transform: translateY(-6px); } }
</style>
