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
  <div class="min-h-screen bg-gray-50 py-10">
    <div class="max-w-7xl mx-auto px-4 flex gap-8">

      <!-- SIDEBAR -->
      <aside class="w-72 bg-white rounded-2xl shadow-sm p-6">
         <!-- Avatar -->
        <div class="w-28 h-28 rounded-full overflow-hidden bg-gray-200 mb-4 ml-16">
          <!-- Replace with <NuxtImg> or <img> -->
          <div class="w-full h-full bg-gray-300" />
        </div>

        <!-- Name & email -->
        <div class="text-center mb-4">
          <p class="font-semibold text-lg"></p>
          <p class="text-sm text-gray-500">zhansulu0808@gmail.com</p>
        </div>

        <!-- Role badge -->
        <span
          class="inline-flex items-center ml-16 justify-center px-4 py-1 text-xs font-semibold text-orange-600 bg-orange-100 rounded-full mb-6"
        >
          Покупатель
        </span>

        <!-- Stats -->
        <div
          class="w-full mb-6 rounded-xl border bg-gray-50 px-4 py-3 flex justify-between items-center"
        >
          <div class="text-xs text-gray-500">Всего заказов</div>
          <div class="text-2xl font-semibold">7</div>
        </div>

        <!-- Menu -->
   <nav class="mt-6 space-y-2">
          <NuxtLink
            to="/profile"
            class="flex items-center gap-3 px-4 py-3 rounded-xl text-gray-600 hover:bg-gray-100"
          >
            Профиль
          </NuxtLink>

          <NuxtLink
            to="/order"
            class="flex items-center gap-3 px-4 py-3 rounded-xl bg-orange-50 text-orange-600 font-medium"
          >
            Мои заказы
          </NuxtLink>
        </nav>
      </aside>

      <!-- CONTENT -->
      <main class="flex-1 bg-white rounded-2xl shadow-sm p-8">
        <h1 class="text-2xl font-semibold mb-6">
          История заказов
        </h1>

        <div class="space-y-4">
          <OrderCard
            date="15 января 2024 г."
            sum="12500 ₽"
            status="completed"
          />
          <OrderCard
            date="20 февраля 2024 г."
            sum="8000 ₽"
            status="processing"
          />
          <OrderCard
            date="10 марта 2024 г."
            sum="23000 ₽"
            status="completed"
          />
          <OrderCard
            date="01 апреля 2024 г."
            sum="5000 ₽"
            status="cancelled"
          />
          <OrderCard
            date="05 мая 2024 г."
            sum="15000 ₽"
            status="processing"
          />
        </div>
      </main>

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
// import OrderCard from '@/components/OrderCard.vue'
definePageMeta({ name: "profile-orders" })
</script>
