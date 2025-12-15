<template>
  <header class="w-full bg-[url('/images/newYear.3940986.png')] bg-contain bg-no-repeat bg-center h-40 text-white shadow-md relative overflow-hidden">
    <div class="max-w-7xl mx-auto px-4 py-4 flex items-center justify-between">
      <!-- Logo -->
      <div class="flex items-center space-x-2 text-[#003049]">
        <img src="../public/images/logo.png" alt="Logo" class="h-10" />
      </div>

      <!-- Search Bar -->
      <div class="flex-1 mx-10 mt-5">
        <div class="flex rounded-full overflow-hidden shadow-sm">
          <input type="text" placeholder="Поиск" class="flex-1 px-4 py-2 text-gray-700 focus:outline-none" />
         <button class="px-4 text-white bg-[#003049] border-l">по скидкам</button>
        </div>
      </div>

      <!-- Login / Lang -->
      <div class="flex items-center space-x-4">
        <NuxtLink to="/login" class="bg-white text-gray-800 px-4 py-2 rounded-full shadow">Login</NuxtLink>
        <button class="bg-white text-gray-800 px-4 py-2 rounded-full shadow">RU</button>
      </div>
    </div>

    <!-- Navigation -->
    <nav class="text-gray-800 py-4 mt-5 shadow-md">
      <div class="max-w-7xl mx-auto px-4 flex space-x-8 text-sm font-medium">
        <NuxtLink :to="{ name: 'glav' }" class="hover:text-[#C1121F] text-[#003049]">Главная</NuxtLink>
        <NuxtLink :to="{ name: 'catalog' }" class="hover:text-[#C1121F] text-[#003049]">Каталог</NuxtLink>
        <a href="#" class="hover:text-[#C1121F] text-[#003049]">Доставка</a>
        <NuxtLink :to="{ name: 'profile' }" class="hover:text-[#C1121F] text-[#003049]">Профиль</NuxtLink>
        <a href="#" class="hover:text-[#C1121F] text-[#003049]">О компании</a>
        <NuxtLink :to="{ name: 'contact' }" class="hover:text-[#C1121F] text-[#003049]">Контакты</NuxtLink>
      </div>
    </nav>
  </header>

  <div class="min-h-screen bg-gray-50">
    <!-- Logout -->
    <div class="max-w-7xl mx-auto px-4 py-4 flex justify-end">
      <button @click="logout" class="flex items-center gap-2 text-sm font-medium text-red-500 hover:text-red-600">Выход</button>
    </div>

    <main class="max-w-7xl mx-auto px-4 flex gap-8 pb-10">
      <!-- Sidebar -->
      <aside class="w-72 bg-white rounded-2xl shadow-sm p-6">
        <div class="w-28 h-28 rounded-full bg-orange-100 flex items-center justify-center text-3xl font-bold text-orange-600 mx-auto mb-4">
          {{ user.name?.charAt(0) || "?" }}
        </div>

        <div class="text-center mb-4">
          <p class="font-semibold text-lg">{{ user.name }}</p>
          <p class="text-sm text-gray-500">{{ user.email }}</p>
        </div>

        <!-- Показываем роль пользователя -->
        <span class="block text-center px-4 py-1 text-xs font-semibold text-orange-600 bg-orange-100 rounded-full mb-6">
          {{ user.role === "admin" ? "Админ" : "Покупатель" }}
        </span>

        <div class="rounded-xl border bg-gray-50 px-4 py-3 flex justify-between items-center mb-6">
          <span class="text-xs text-gray-500">Всего заказов</span>
          <span class="text-2xl font-semibold">7</span>
        </div>

        <nav class="space-y-2">
          <NuxtLink to="/profile" class="block px-4 py-3 rounded-xl bg-orange-50 text-orange-600 font-medium">Профиль</NuxtLink>
          <NuxtLink to="/order" class="block px-4 py-3 rounded-xl text-gray-600 hover:bg-gray-100">Мои заказы</NuxtLink>

          <!-- Только для админа -->
          <NuxtLink
            v-if="user.role === 'admin'"
            to="/admin"
            class="block px-4 py-3 rounded-xl text-red-600 hover:bg-gray-100 font-semibold"
          >
            Админ панель
          </NuxtLink>
        </nav>
      </aside>

      <!-- Content -->
      <section class="flex-1 bg-white rounded-2xl shadow-sm p-8">
        <h1 class="text-3xl font-bold mb-8">Личные данные</h1>

        <form class="grid grid-cols-1 md:grid-cols-2 gap-6 max-w-3xl" @submit.prevent="saveProfile">
          <div>
            <label class="block text-sm text-gray-600 mb-1">Имя</label>
            <input v-model="form.name" type="text" class="w-full border rounded-lg px-4 py-2.5 focus:ring-2 focus:ring-orange-400" />
          </div>

          <div>
            <label class="block text-sm text-gray-600 mb-1">Email</label>
            <input v-model="form.email" type="email" class="w-full border rounded-lg px-4 py-2.5 focus:ring-2 focus:ring-orange-400" />
          </div>

          <div>
            <label class="block text-sm text-gray-600 mb-1">Телефон</label>
            <input v-model="form.phone" type="tel" class="w-full border rounded-lg px-4 py-2.5 focus:ring-2 focus:ring-orange-400" />
          </div>

          <div>
            <label class="block text-sm text-gray-600 mb-1">Адрес</label>
            <input v-model="form.address" type="text" class="w-full border rounded-lg px-4 py-2.5 focus:ring-2 focus:ring-orange-400" />
          </div>

          <div class="md:col-span-2">
            <button type="submit" :disabled="saving" class="px-6 py-3 bg-orange-500 text-white rounded-lg hover:bg-orange-600 disabled:opacity-50">
              {{ saving ? "Сохранение..." : "Сохранить изменения" }}
            </button>
          </div>
        </form>
      </section>
    </main>
  </div>
</template>

<script setup lang="ts">
import { reactive, ref, onMounted } from "vue"
import { useRouter } from "vue-router"
import axios from "axios"

const router = useRouter()
const saving = ref(false)
const user = reactive({
  name: "",
  email: "",
  role: "user",
});

const form = reactive({
  name: "",
  email: "",
  phone: "",
  address: "",
});
const API_URL = "https://medical-backend-54hp.onrender.com/api/auth";
const loadProfile = async () => {
  const token = localStorage.getItem("token");
  if (!token) return router.push("/login");

  try {
    const res = await axios.get(`${API_URL}/me`, {
      headers: { Authorization: `Bearer ${token}` },
    });

    // 👇 нақты user объектіні аламыз
    const profile = res.data.data || res.data.data?.user;

    Object.assign(user, profile);
    Object.assign(form, profile);

    localStorage.setItem("user", JSON.stringify(profile));
  } catch (err) {
    console.error(err);
    logout();
  }
};


// Сақтау функциясы
const saveProfile = async () => {
  const token = localStorage.getItem("token");
  if (!token) return;

  saving.value = true;
  try {
    const { data } = await axios.put(`${API_URL}/update`, form, {
      headers: { Authorization: `Bearer ${token}` },
    });

    // Обновлениелерді localStorage-қа жазамыз
    Object.assign(user, data);
    localStorage.setItem("user", JSON.stringify(data));

    alert("Данные сохранены");
  } catch (err) {
    console.error(err);
    alert("Ошибка при сохранении");
  } finally {
    saving.value = false;
  }
};

onMounted(loadProfile);
const logout = () => {
  localStorage.removeItem("token");
  localStorage.removeItem("user");
  router.push("/login");
};
</script>
