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
  <div class="min-h-screen bg-gray-50 py-10">
    <div class="max-w-7xl mx-auto px-4 flex gap-8">

      <!-- SIDEBAR -->
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


      <!-- CONTENT -->
      <main class="flex-1 bg-white rounded-2xl shadow-sm p-8">
 <div class="max-w-6xl mx-auto p-6">
  <h1 class="text-2xl font-bold mb-6">Мои заказы</h1>

  <!-- Егер заказ жоқ -->
  <div v-if="orders.length === 0" class="text-gray-500">
    Заказов пока нет
  </div>

  <!-- Егер заказ бар -->
  <div v-else>
    <!-- TOTAL -->
    <div class="mb-6 flex justify-between items-center">
      <p class="text-lg font-semibold">
        Общая сумма:
        <span class="text-orange-600">
          {{ totalPrice }} тг
        </span>
      </p>
    </div>

    <!-- GRID -->
    <div class="grid grid-cols-2 md:grid-cols-4 gap-6">
      <div
        v-for="item in orders"
        :key="item.id"
        class="border rounded-xl p-4 relative"
      >
        <!-- DELETE -->
        <button
          @click="removeOrder(item.id)"
          class="absolute top-2 right-2 text-gray-400 hover:text-red-600"
        >
          ✕
        </button>

        <!-- Фото -->
        <img
          :src="item.image || '/images/no-image.png'"
          alt="product"
          class="h-32 w-full object-contain mb-3 bg-gray-100 rounded"
        />

        <!-- Аты -->
        <p class="text-sm font-semibold line-clamp-2">
          {{ item.name || 'Без названия' }}
        </p>

        <!-- Бағасы -->
        <p class="font-bold mt-2">
          {{ item.final_price ?? 0 }} тг
        </p>
      </div>
    </div>
  </div>
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




<script setup lang="ts">
import { reactive, ref, onMounted } from "vue"
import { useRouter } from "vue-router"
import axios from "axios"
definePageMeta({ name: "profile-orders" })



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
  if (!token) return router.push("/profile");

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





const orders = ref<any[]>([])

onMounted(() => {
  const saved = localStorage.getItem('orders')
  orders.value = saved ? JSON.parse(saved) : []
})

// DELETE
const removeOrder = (id: string | number) => {
  orders.value = orders.value.filter(item => item.id !== id)
  localStorage.setItem('orders', JSON.stringify(orders.value))
}

// TOTAL PRICE
const totalPrice = computed(() => {
  return orders.value.reduce(
    (sum, item) => sum + Number(item.final_price || 0),
    0
  )
})


</script>

