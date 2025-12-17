<template>
  <header
    class="w-full bg-[url('/images/newYear.3940986.png')] bg-contain h-40 bg-no-repeat bg-center shadow-md relative"
  >
    <!-- Десктоп хэдер - өзгеріссіз -->
    <div
      class="hidden md:flex max-w-7xl mx-auto px-4 py-4 flex items-center justify-between "
    >
      <!-- Logo -->
      <NuxtLink to="/" class="flex items-center space-x-2">
        <img src="/images/logo.png" alt="Logo" class="h-10" />
      </NuxtLink>

      <!-- Search -->
      <form @submit.prevent="search" class="flex-1 mx-10 mt-5">
        <div class="flex rounded-full overflow-hidden shadow-sm bg-white">
          <input
            v-model="searchText"
            type="text"
            placeholder="Поиск товара..."
            class="flex-1 px-4 py-2 text-gray-700 focus:outline-none"
          />
          <button type="submit" class="px-4 text-white bg-[#003049]">
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

    <!-- Десктоп навигация - өзгеріссіз -->
    <nav
      class="hidden md:block text-gray-800 py-4 mt-5 shadow-md bg-white/80 backdrop-blur"
    >
      <div class="max-w-7xl mx-auto px-4 flex space-x-8 text-sm font-medium">
        <NuxtLink to="/">Главная</NuxtLink>
        <NuxtLink to="/catalog">Каталог</NuxtLink>
        <NuxtLink to="/dashbord">Дашборд</NuxtLink>
        <NuxtLink to="/profile">Профиль</NuxtLink>
        <NuxtLink to="/contacts">Контакты</NuxtLink>
      </div>
    </nav>

    <!-- Мобильдік хэдер -->
    <div
      class="flex md:hidden items-center justify-between h-10 px-4"
    >
      <!-- Logo -->
      <NuxtLink to="/" class="flex items-center space-x-2">
        <img src="/images/logoo.png" alt="Logo" class="h-10" />
      </NuxtLink>

      <!-- Burger button -->
      <button
        @click="sidebarOpen = !sidebarOpen"
        class="text-gray-800 focus:outline-none"
        aria-label="Toggle menu"
      >
        <svg
          v-if="!sidebarOpen"
          xmlns="http://www.w3.org/2000/svg"
          class="h-8 w-8"
          fill="none"
          viewBox="0 0 24 24"
          stroke="currentColor"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M4 6h16M4 12h16M4 18h16"
          />
        </svg>
        <svg
          v-else
          xmlns="http://www.w3.org/2000/svg"
          class="h-8 w-8"
          fill="none"
          viewBox="0 0 24 24"
          stroke="currentColor"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M6 18L18 6M6 6l12 12"
          />
        </svg>
      </button>
    </div>

    <!-- Мобильдік меню (Sidebar) -->
    <transition name="fade">
      <div
        v-if="sidebarOpen"
        class="fixed inset-0 bg-black bg-opacity-50 z-40"
        @click="sidebarOpen = false"
      ></div>
    </transition>

    <transition name="slide">
      <nav
        v-if="sidebarOpen"
        class="fixed top-0 left-0 h-full w-64 bg-white shadow-lg z-50 p-6 flex flex-col space-y-6"
      >
        <!-- Close button -->
        <button
          @click="sidebarOpen = false"
          class="self-end text-gray-800 focus:outline-none mb-6"
          aria-label="Close menu"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            class="h-8 w-8"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M6 18L18 6M6 6l12 12"
            />
          </svg>
        </button>

        <!-- Search -->
        <form
          @submit.prevent="search"
          class="flex rounded-full overflow-hidden shadow-sm bg-gray-100"
        >
          <input
            v-model="searchText"
            type="text"
            placeholder="Поиск товара..."
            class="flex-1 px-4 py-2 text-gray-700 focus:outline-none bg-transparent"
          />
          <button type="submit" class="px-4 text-white bg-[#003049]">
            Поиск
          </button>
        </form>

        <!-- Navigation links -->
        <NuxtLink
          v-for="link in navLinks"
          :key="link.path"
          :to="link.path"
          class="text-gray-800 text-lg font-medium"
          @click="sidebarOpen = false"
        >
          {{ link.name }}
        </NuxtLink>

        <!-- Auth -->
        <div>
          <NuxtLink
            v-if="isAuth"
            to="/profile"
            class="flex items-center gap-2 bg-[#C1121F] text-white px-4 py-2 rounded-full shadow"
            @click="sidebarOpen = false"
          >
            <div
              class="w-8 h-8 rounded-full flex items-center justify-center font-bold"
            >
              {{ user.name?.charAt(0).toUpperCase() }}
            </div>
            <span>{{ user.name }}</span>
          </NuxtLink>

          <NuxtLink
            v-else
            to="/login"
            class="bg-[#003049] text-white px-4 py-2 rounded-full shadow"
            @click="sidebarOpen = false"
          >
            Login
          </NuxtLink>
        </div>
      </nav>
    </transition>
  </header>
</template>

<script setup lang="ts">
import { ref, onMounted } from "vue";
import { useRouter } from "vue-router";

const router = useRouter();

const searchText = ref("");
const user = ref<any>({});
const isAuth = ref(false);
const sidebarOpen = ref(false);

const navLinks = [
  { path: "/", name: "Главная" },
  { path: "/catalog", name: "Каталог" },
  { path: "/dashbord", name: "Дашборд" },
  { path: "/profile", name: "Профиль" },
  { path: "/contacts", name: "Контакты" },
];

const search = () => {
  if (!searchText.value.trim()) return;

  router.push({
    path: "/catalog",
    query: { search: searchText.value },
  });
  sidebarOpen.value = false;
};

onMounted(() => {
  const token = localStorage.getItem("token");
  const savedUser = localStorage.getItem("user");

  if (token && savedUser) {
    user.value = JSON.parse(savedUser);
    isAuth.value = true;
  }
});
</script>

<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
.slide-enter-active {
  transition: transform 0.3s ease;
}
.slide-enter-from {
  transform: translateX(-100%);
}
.slide-leave-active {
  transition: transform 0.3s ease;
}
.slide-leave-to {
  transform: translateX(-100%);
}
</style>
