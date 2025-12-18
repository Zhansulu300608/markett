<template>
  <AppHeader />
  <slot />
 <SnowEffect />

  <slot />
  <div class="min-h-screen bg-gray-50">
    
    <div class="max-w-7xl mx-auto px-4 py-4 flex justify-end">
      <button
        @click="logout"
        class="flex items-center gap-2 text-sm font-medium text-red-500 hover:text-red-600"
      >
        Выход
      </button>
    </div>

  
    <main
      class="max-w-7xl mx-auto px-4 pb-10 flex flex-col md:flex-row gap-8"
    >
     
      <aside
        class="w-full md:w-72 bg-white rounded-2xl shadow-sm p-6 flex flex-col items-center md:items-stretch"
      >
        <div
          class="w-24 h-24 md:w-28 md:h-28 rounded-full bg-[#FDF0D5] flex items-center ml-5 md:ml-16 justify-center text-3xl font-bold text-[#003049] mb-4"
        >
          {{ user.name?.charAt(0) || "?" }}
        </div>

        <div class="text-center md:text-center mb-4 w-full">
          <p class="font-semibold text-lg break-words">{{ user.name }}</p>
          <p class="text-sm text-gray-500 break-words">{{ user.email }}</p>
        </div>

        <span
          class="block text-center md:text-left px-4 py-1 text-xs font-semibold text-[#003049] bg-[#FDF0D5] rounded-full mb-6 w-full"
        >
          {{ user.role === "admin" ? "Админ" : "Покупатель" }}
        </span>

       

       <nav class="space-y-2 w-full">
  <NuxtLink
    to="/profile"
    class="block px-4 py-3 rounded-xl bg-[#003049] text-white hover:bg-[#FDF0D5] hover:text-[#003049] font-medium text-center"
  >
    Профиль
  </NuxtLink>
  <NuxtLink
    v-if="user.role !== 'admin'"
    to="/order"
    class="block px-4 py-3 rounded-xl bg-[#003049] text-white hover:bg-[#FDF0D5] hover:text-[#003049] text-center"
  >
    Мои заказы
  </NuxtLink>

  <NuxtLink
    v-if="user.role === 'admin'"
    to="/admin/dashboard"
    class="block px-4 py-3 rounded-xl text-red-600 hover:bg-gray-100 font-semibold text-center"
  >
    Админ панель
  </NuxtLink>
</nav>

      </aside>

    
      <section
        class="flex-1 bg-white rounded-2xl shadow-sm p-6 md:p-8 max-w-full md:max-w-none"
      >
        <h1 class="text-2xl md:text-3xl font-bold mb-6 md:mb-8">
          Личные данные
        </h1>

        <form
          class="grid grid-cols-1 md:grid-cols-2 gap-6 max-w-full md:max-w-3xl"
          @submit.prevent="saveProfile"
        >
          <div>
            <label class="block text-sm text-gray-600 mb-1">Имя</label>
            <input
              v-model="form.name"
              type="text"
              class="w-full border rounded-lg px-4 py-2.5 focus:ring-2 focus:ring-orange-400"
            />
          </div>

          <div>
            <label class="block text-sm text-gray-600 mb-1">Email</label>
            <input
              v-model="form.email"
              type="email"
              class="w-full border rounded-lg px-4 py-2.5 focus:ring-2 focus:ring-orange-400"
            />
          </div>

          <div>
            <label class="block text-sm text-gray-600 mb-1">Телефон</label>
            <input
              v-model="form.phone"
              type="tel"
              class="w-full border rounded-lg px-4 py-2.5 focus:ring-2 focus:ring-orange-400"
            />
          </div>

          <div>
            <label class="block text-sm text-gray-600 mb-1">Адрес</label>
            <input
              v-model="form.address"
              type="text"
              class="w-full border rounded-lg px-4 py-2.5 focus:ring-2 focus:ring-orange-400"
            />
          </div>

          <div class="md:col-span-2">
            <button
              type="submit"
              :disabled="saving"
              class="px-6 py-3 bg-[#003049] text-white rounded-lg hover:bg-[#FDF0D5] hover:text-[#003049] disabled:opacity-50 w-full"
            >
              {{ saving ? "Сохранение..." : "Сохранить изменения" }}
            </button>
          </div>
        </form>
      </section>
    </main>
  </div>

 
  <footer class="bg-[#C1121F] py-16">
    <div
      class="max-w-7xl mx-auto px-6 text-white grid grid-cols-1 md:grid-cols-4 gap-12"
    >
      <div>
        <img src="/images/logo.png" class="h-10 mb-4" />
        <p class="font-bold">НАПОЛНЯЕМ ЖИЗНЬ</p>
      </div>
      <div>
        <h3 class="font-bold mb-4">Быстрые ссылки</h3>
        <ul class="space-y-2">
          <li>
            <NuxtLink to="/" class="hover:text-[#003049]">Главная</NuxtLink>
          </li>
          <li>
            <NuxtLink to="/catalog" class="hover:text-[#003049]">Каталог</NuxtLink>
          </li>
          <li>
            <NuxtLink to="/dashbord" class="hover:text-[#003049]">Дашборд</NuxtLink>
          </li>
          <li>
            <NuxtLink to="/contacts" class="hover:text-[#003049]">Контакты</NuxtLink>
          </li>
        </ul>
      </div>

      <div>
        <h3 class="font-bold mb-4">Контакты</h3>
        <p>+7 776 624 82 55</p>
        <p>+7 776 777 77 09</p>
      </div>
      <div>
        <h3 class="text-white font-semibold mb-4">Служба поддержки</h3>
        <p
          class="text-lg mb-6 hover:text-[#003049] text-white"
        >
          <span class="font-semibold hover:text-[#003049] text-white">7766</span>
          | звонок бесплатный
        </p>

        <h3 class="text-white font-semibold mb-4">Мы в социальных сетях</h3>

        <div class="flex flex-wrap gap-3 mb-8">
          <a
            href="https://twitter.com"
            target="_blank"
            rel="noopener"
            class="w-6 h-6"
          >
            <img
              src="/images/icons8-phone-50.png"
              alt="Twitter"
              class="w-full h-full object-contain"
            />
          </a>
          <a
            href="https://twitter.com"
            target="_blank"
            rel="noopener"
            class="w-6 h-6"
          >
            <img
              src="/images/icons8-whatsapp-logo-50.png"
              alt="Twitter"
              class="w-full h-full object-contain"
            />
          </a>
          <a
            href="https://twitter.com"
            target="_blank"
            rel="noopener"
            class="w-6 h-6"
          >
            <img
              src="/images/icons8-email-50.png"
              alt="Twitter"
              class="w-full h-full object-contain"
            />
          </a>
          <a
            href="https://twitter.com"
            target="_blank"
            rel="noopener"
            class="w-6 h-6"
          >
            <img
              src="/images/icons8-tiktok-50.png"
              alt="Twitter"
              class="w-full h-full object-contain"
            />
          </a>
          <a
            href="https://twitter.com"
            target="_blank"
            rel="noopener"
            class="w-6 h-6"
          >
            <img
              src="/images/icons8-instagram-logo-50.png"
              alt="Twitter"
              class="w-full h-full object-contain"
            />
          </a>
          <a
            href="https://instagram.com"
            target="_blank"
            rel="noopener"
            class="w-6 h-6"
          ></a>
        </div>

        <h3
          class="text-white font-semibold mb-2 hover:text-[#003049] text-white"
        >
          Служба доставки
        </h3>
        <p class="text-lg hover:text-[#003049] text-white">
          <span class="font-semibold">7772</span> | звонок бесплатный
        </p>
      </div>
    </div>
  </footer>
</template>

<script setup lang="ts">
import { reactive, ref, onMounted } from "vue";
import { useRouter } from "vue-router";
import axios from "axios";
import SnowEffect from '~/components/SnowEffect.vue'
definePageMeta({ name: "profile" });
const router = useRouter();
const saving = ref(false);

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
  if (!token) return router.push("/profile");

  try {
    const res = await axios.get(`${API_URL}/me`, {
      headers: { Authorization: `Bearer ${token}` },
    });

    const profile = res.data?.data || res.data;

    Object.assign(user, profile);
    Object.assign(form, profile);

    localStorage.setItem("user", JSON.stringify(profile));
  } catch (err: any) {
    console.error("Ошибка при загрузке профиля:", err.response?.data || err.message);
    logout();
  }
};

const saveProfile = async () => {
  const token = localStorage.getItem("token");
  if (!token) return router.push("/profile");

  saving.value = true;
  try {
    const res = await axios.put(`${API_URL}/me`, form, {
      headers: { Authorization: `Bearer ${token}` },
    });

    const updatedUser = res.data?.data || res.data;
    Object.assign(user, updatedUser);
    Object.assign(form, updatedUser);
    localStorage.setItem("user", JSON.stringify(updatedUser));

    alert("Данные сохранены");
  } catch (err: any) {
    console.error("Ошибка при сохранении:", err.response?.data || err.message);
    alert("Ошибка при сохранении: " + (err.response?.data?.message || err.message));
  } finally {
    saving.value = false;
  }
};
const logout = () => {
  localStorage.removeItem("token");
  localStorage.removeItem("user");
  router.push("/login");
};

onMounted(loadProfile);
</script>
