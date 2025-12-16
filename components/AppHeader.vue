<template>
  <header
    class="w-full bg-[url('/images/newYear.3940986.png')] bg-contain bg-no-repeat bg-center h-40 shadow-md relative"
  >
    <div class="max-w-7xl mx-auto px-4 py-4 flex items-center justify-between">
      
      <!-- Logo -->
      <NuxtLink to="/" class="flex items-center space-x-2">
        <img src="/images/logo.png" alt="Logo" class="h-10" />
      </NuxtLink>

      <!-- Search -->
      <div class="flex-1 mx-10 mt-5">
        <div class="flex rounded-full overflow-hidden shadow-sm bg-white">
          <input
            type="text"
            placeholder="Поиск"
            class="flex-1 px-4 py-2 text-gray-700 focus:outline-none"
          />
          <button class="px-4 text-white bg-[#003049]">
            по скидкам
          </button>
        </div>
      </div>

      <!-- Auth / Lang -->
      <div class="flex items-center gap-4">
        
        <!-- ЕСЛИ ЛОГИН БОЛСА -->
        <NuxtLink
          v-if="isAuth"
          to="/profile"
          class="flex items-center gap-2 bg-white px-4 py-2 rounded-full shadow hover:bg-gray-100 transition"
        >
          <!-- User Icon -->
          <div
            class="w-8 h-8 rounded-full bg-orange-500 text-white flex items-center justify-center font-bold"
          >
            {{ user.name?.charAt(0).toUpperCase() }}
          </div>

          <span class="hidden sm:block text-gray-800 font-medium">
            {{ user.name }}
          </span>
        </NuxtLink>

        <!-- ЕСЛИ ЛОГИН ЖОҚ -->
        <NuxtLink
          v-else
          to="/login"
          class="bg-white text-gray-800 px-4 py-2 rounded-full shadow hover:bg-gray-100"
        >
          Login
        </NuxtLink>

   
      </div>
    </div>

    <!-- Navigation -->
    <nav class="text-gray-800 py-4 mt-5 shadow-md bg-white/80 backdrop-blur">
      <div class="max-w-7xl mx-auto px-4 flex space-x-8 text-sm font-medium">
        <NuxtLink to="/" class="hover:text-[#C1121F] text-[#003049]">Главная</NuxtLink>
        <NuxtLink to="/catalog" class="hover:text-[#C1121F] text-[#003049]">Каталог</NuxtLink>
         <NuxtLink to="/dashbord" class="hover:text-[#C1121F] text-[#003049]">Дашборд</NuxtLink>
        <NuxtLink to="/profile" class="hover:text-[#C1121F] text-[#003049]">Профиль</NuxtLink>
        <NuxtLink to="/contacts" class="hover:text-[#C1121F] text-[#003049]">Контакты</NuxtLink>
      </div>
    </nav>
  </header>
</template>

<script setup lang="ts">
import { ref, onMounted } from "vue";

const user = ref<any>({});
const isAuth = ref(false);

onMounted(() => {
  const token = localStorage.getItem("token");
  const savedUser = localStorage.getItem("user");

  if (token && savedUser) {
    user.value = JSON.parse(savedUser);
    isAuth.value = true;
  }
});
</script>
