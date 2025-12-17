<template>
  <header
    class="w-full bg-[url('/images/newYear.3940986.png')] bg-contain h-40 bg-no-repeat bg-center shadow-md relative"
  >

    <div
      class="hidden md:flex max-w-7xl mx-auto px-4 py-4 items-center justify-between"
    >
   
      <NuxtLink to="/" class="flex items-center space-x-2">
        <img src="/images/logo.png" alt="Logo" class="h-10" />
      </NuxtLink>

    
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

   
    <nav
      class="hidden md:block text-gray-800 py-4 mt-5 shadow-md bg-white/80 backdrop-blur"
    >
      <div class="max-w-7xl mx-auto px-4 flex space-x-8 text-sm font-medium">
        <NuxtLink
          v-for="link in filteredNavLinks"
          :key="link.path"
          :to="link.path"
        >
          {{ link.name }}
        </NuxtLink>
      </div>
    </nav>


    <div class="flex md:hidden items-center justify-between h-10 px-4">
      <NuxtLink to="/">
        <img src="/images/logoo.png" alt="Logo" class="h-10" />
      </NuxtLink>

      <button
        @click="sidebarOpen = !sidebarOpen"
        class="text-gray-800"
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
        <button
          @click="sidebarOpen = false"
          class="self-end text-gray-800"
        >
          ✕
        </button>

        
        <form
          @submit.prevent="search"
          class="flex rounded-full overflow-hidden bg-gray-100"
        >
          <input
            v-model="searchText"
            type="text"
            placeholder="Поиск товара..."
            class="flex-1 px-4 py-2 bg-transparent"
          />
          <button class="px-4 bg-[#003049] text-white">Поиск</button>
        </form>

  
        <NuxtLink
          v-for="link in filteredNavLinks"
          :key="link.path"
          :to="link.path"
          class="text-lg font-medium"
          @click="sidebarOpen = false"
        >
          {{ link.name }}
        </NuxtLink>

  
        <NuxtLink
          v-if="isAuth"
          to="/profile"
          class="bg-[#C1121F] text-white px-4 py-2 rounded-full text-center"
        >
          {{ user.name }}
        </NuxtLink>

        <NuxtLink
          v-else
          to="/login"
          class="bg-[#003049] text-white px-4 py-2 rounded-full text-center"
        >
          Login
        </NuxtLink>
      </nav>
    </transition>
  </header>
</template>

<script setup lang="ts">
import { ref, computed, onMounted } from "vue";
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

const filteredNavLinks = computed(() => {
  return navLinks.filter(link => {
    if (link.path === "/dashbord" && user.value?.role === "admin") {
      return false;
    }
    return true;
  });
});

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
  transition: opacity 0.3s;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.slide-enter-active,
.slide-leave-active {
  transition: transform 0.3s;
}
.slide-enter-from,
.slide-leave-to {
  transform: translateX(-100%);
}
</style>
