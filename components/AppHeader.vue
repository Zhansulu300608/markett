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
      <form @submit.prevent="doSearch"
        class="flex-1 mx-10 mt-5"
      >
        <div class="flex rounded-full overflow-hidden shadow-sm bg-white">
          <input
            v-model="searchText"
            type="text"
            placeholder="Поиск товара..."
            class="flex-1 px-4 py-2 text-gray-700 focus:outline-none"
          />
          <button
            type="submit"
            class="px-4 text-white bg-[#003049]"
          >
            Поиск
          </button>
        </div>
      </form>

      <!-- Auth -->
      <div class="flex items-center gap-4">
        <NuxtLink
          v-if="isAuth"
          to="/profile"
          class="flex items-center gap-2 bg-white px-4 py-2 rounded-full shadow"
        >
          <div
            class="w-8 h-8 rounded-full bg-[#C1121F] text-white flex items-center justify-center font-bold"
          >
            {{ user.name?.charAt(0).toUpperCase() }}
          </div>
          <span class="hidden sm:block text-gray-800 font-medium">
            {{ user.name }}
          </span>
        </NuxtLink>

        <NuxtLink
          v-else
          to="/login"
          class="bg-white text-gray-800 px-4 py-2 rounded-full shadow"
        >
          Login
        </NuxtLink>
      </div>
    </div>

    <!-- Navigation -->
    <nav class="text-gray-800 py-4 mt-5 shadow-md bg-white/80 backdrop-blur">
      <div class="max-w-7xl mx-auto px-4 flex space-x-8 text-sm font-medium">
        <NuxtLink to="/">Главная</NuxtLink>
        <NuxtLink to="/catalog">Каталог</NuxtLink>
        <NuxtLink to="/dashbord">Дашборд</NuxtLink>
        <NuxtLink to="/profile">Профиль</NuxtLink>
        <NuxtLink to="/contacts">Контакты</NuxtLink>
      </div>
    </nav>
  </header>
</template>

<script setup lang="ts">
import { ref, onMounted } from "vue";
import { useRouter } from "vue-router";



const searchText = ref("");
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
const search = ref('')
const router = useRouter()

const doSearch = () => {
  if (!search.value.trim()) return
  router.push({
    path: '/catalog',
    query: { search: search.value }
  })
}

</script>
