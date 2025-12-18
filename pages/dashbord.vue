<template>
  <AppHeader />
 <SnowEffect />

  <slot />
  <div class="flex flex-col md:flex-row min-h-screen bg-gray-100">

    <div class="md:hidden p-4 bg-slate-800 text-white flex justify-between items-center">
      <button @click="sidebarOpen = !sidebarOpen" aria-label="Переключить меню">
        <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M4 6h16M4 12h16M4 18h16"
          />
        </svg>
      </button>
      <h1 class="text-lg font-bold">Панель управления</h1>
    </div>

    <aside
      :class="[
        'bg-slate-800 text-white p-6 md:block',
        sidebarOpen ? 'block w-full' : 'hidden',
        'md:w-64'
      ]"
    >
      <nav class="space-y-4">
        <NuxtLink to="/dashbord" class="block hover:bg-slate-700 px-4 py-2 rounded">Панель управления</NuxtLink>
        <NuxtLink to="/order" class="block hover:bg-slate-700 px-4 py-2 rounded">Мои заказы</NuxtLink>
        <NuxtLink to="/favourties" class="block hover:bg-slate-700 px-4 py-2 rounded">Избранное</NuxtLink>
      </nav>

      <button @click="logout" class="mt-10 w-full bg-red-500 py-2 rounded">Выйти</button>
    </aside>

    <main class="flex-1 p-6 md:p-8">
      <div class="mb-6">
        <h1 class="text-3xl font-bold">Добро пожаловать, {{ user?.name }} 👋</h1>
        <p class="text-gray-500">Это ваша персональная панель управления</p>
      </div>

      <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-8">
        <div class="bg-white p-6 rounded shadow">
          <h3 class="text-gray-500">Мои заказы</h3>
          <p class="text-3xl font-bold">{{ orders.length }}</p>
        </div>

        <div class="bg-white p-6 rounded shadow">
          <h3 class="text-gray-500">Избранное</h3>
          <p class="text-3xl font-bold">{{ favorites.length }}</p>
        </div>

        <div class="bg-white p-6 rounded shadow">
          <h2 class="text-xl font-bold mb-4">Мой профиль</h2>
          <p><b>Имя:</b> {{ user?.name }}</p>
          <p><b>Email:</b> {{ user?.email }}</p>
          <p><b>Роль:</b> {{ user?.role }}</p>
        </div>
      </div>

      <div class="bg-white p-6 rounded shadow mb-8">
        <div class="flex justify-between mb-6">
          <h2 class="text-xl font-bold">Товары</h2>
          <button
            @click="addProduct"
            class="bg-blue-600 text-white px-4 py-2 rounded"
          >
            + Добавить товар
          </button>
        </div>

        <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
          <div
            v-for="product in products"
            :key="product.id"
            class="border rounded-2xl p-4 relative flex flex-col h-[340px] cursor-pointer shadow-sm"
          >
            <span
              v-if="product.discount"
              class="absolute top-3 left-3 bg-[#C1121F] text-white text-sm px-3 py-1 rounded-full"
            >
              -{{ product.discount }}%
            </span>

            <p class="absolute top-3 right-3 text-xs text-gray-400">
              до {{ product.action_end }}
            </p>

            <img
              :src="product.image"
              :alt="product.name"
              class="h-36 w-full object-contain bg-gray-100 rounded-xl mb-3"
            />

            <h3 class="text-sm font-semibold line-clamp-2 min-h-[40px]">
              {{ product.name }}
            </h3>

            <div class="mt-auto pt-3">
              <p class="text-xs line-through text-gray-400">
                {{ product.start_price }} ₸
              </p>
              <p class="bg-yellow-400 px-4 py-2 rounded-xl font-bold text-lg inline-block">
                {{ product.final_price }} ₸
              </p>
            </div>

            <div class="flex justify-between mt-3">
              <button
                @click.stop="editProduct(product)"
                class="bg-yellow-400 px-3 py-1 rounded"
              >
                Редактировать
              </button>
              <button
                @click.stop="deleteProduct(product.id)"
                class="bg-red-500 text-white px-3 py-1 rounded"
              >
                Удалить
              </button>
            </div>
          </div>

          <p
            v-if="products.length === 0"
            class="text-gray-500 col-span-full text-center"
          >
            Пока нет товаров
          </p>
        </div>
      </div>
    </main>

    <div
      v-if="showModal"
      class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50"
    >
      <div class="bg-white p-6 rounded w-96">
        <h2 class="text-xl font-bold mb-4">
          {{ editingProduct ? 'Редактировать товар' : 'Добавить товар' }}
        </h2>

        <input v-model="productForm.name" placeholder="Название"
          class="w-full p-2 border rounded mb-2" />

        <input v-model.number="productForm.start_price" type="number"
          placeholder="Начальная цена"
          class="w-full p-2 border rounded mb-2" />

        <input v-model.number="productForm.final_price" type="number"
          placeholder="Конечная цена"
          class="w-full p-2 border rounded mb-2" />

        <input v-model="productForm.image" placeholder="URL изображения"
          class="w-full p-2 border rounded mb-2" />

        <input v-model="productForm.action_start" placeholder="Начало акции"
          class="w-full p-2 border rounded mb-2" />

        <input v-model.number="productForm.action_end" type="number"
          placeholder="Конец акции"
          class="w-full p-2 border rounded mb-4" />

        <div class="flex justify-end gap-2">
          <button @click="showModal=false" class="border px-4 py-2 rounded">
            Отмена
          </button>
          <button @click="saveProduct"
            class="bg-blue-600 text-white px-4 py-2 rounded">
            {{ editingProduct ? 'Обновить' : 'Создать' }}
          </button>
        </div>
      </div>
    </div>
  </div>

  <footer class="bg-[#C1121F] py-16">
    <div class="max-w-7xl mx-auto px-6 text-white grid grid-cols-1 md:grid-cols-4 gap-12">
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
            <NuxtLink to="/dashbord" class="hover:text-[#003049]">Панель управления</NuxtLink>
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
        <p class="text-lg mb-6 hover:text-[#003049] text-white">
          <span class="font-semibold hover:text-[#003049] text-white">7766</span> | звонок бесплатный
        </p>

        <h3 class="text-white font-semibold mb-4">
          Мы в социальных сетях
        </h3>

        <div class="flex flex-wrap gap-3 mb-8">
          <a href="https://twitter.com" target="_blank" rel="noopener" class="w-6 h-6">
            <img src="/images/icons8-phone-50.png" alt="Телефон" class="w-full h-full object-contain" />
          </a>
          <a href="https://twitter.com" target="_blank" rel="noopener" class="w-6 h-6">
            <img src="/images/icons8-whatsapp-logo-50.png" alt="WhatsApp" class="w-full h-full object-contain" />
          </a>
          <a href="https://twitter.com" target="_blank" rel="noopener" class="w-6 h-6">
            <img src="/images/icons8-email-50.png" alt="Email" class="w-full h-full object-contain" />
          </a>
          <a href="https://twitter.com" target="_blank" rel="noopener" class="w-6 h-6">
            <img src="/images/icons8-tiktok-50.png" alt="TikTok" class="w-full h-full object-contain" />
          </a>
          <a href="https://twitter.com" target="_blank" rel="noopener" class="w-6 h-6">
            <img src="/images/icons8-instagram-logo-50.png" alt="Instagram" class="w-full h-full object-contain" />
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
  </footer>
</template>

<script setup lang="ts">
import { ref, onMounted } from "vue";
import axios from "axios";
import { useRouter } from "vue-router";
import SnowEffect from '~/components/SnowEffect.vue'
const router = useRouter();
const API = "https://6940519c993d68afba6bb782.mockapi.io/market";

const user = ref<any>(null);

const orders = ref<any[]>([]);
const orderForm = ref({ title: "", status: "Pending" });
const editingOrder = ref<any>(null);

const products = ref<any[]>([]);
const productForm = ref({
  name: "",
  start_price: 0,
  final_price: 0,
  image: "",
  action_start: "",
  action_end: ""
});
const editingProduct = ref<any>(null);

const showModal = ref(false);

const favorites = ref<any[]>([]);

const sidebarOpen = ref(false);

onMounted(async () => {
  const savedUser = localStorage.getItem("user");
  if (!savedUser) return router.push("/login");

  user.value = JSON.parse(savedUser);

  fetchOrders();
  fetchProducts();

  favorites.value = [
    { id: 1, title: "Product A" },
    { id: 2, title: "Product B" },
    { id: 3, title: "Product C" }
  ];
});

const fetchOrders = async () => {
  try {
    const res = await axios.get(`${API}?userId=${user.value.id}&type=order`);
    orders.value = res.data;
  } catch (err) {
    console.error("Ошибка получения заказов", err);
  }
};

const addOrder = () => {
  editingOrder.value = null;
  orderForm.value = { title: "", status: "Pending" };
  showModal.value = true;
};

const editOrder = (order: any) => {
  editingOrder.value = order;
  orderForm.value = { ...order };
  showModal.value = true;
};

const saveOrder = async () => {
  try {
    if (editingOrder.value) {
      const res = await axios.put(`${API}/${editingOrder.value.id}`, {
        ...orderForm.value,
        userId: user.value.id,
        type: "order"
      });
      const idx = orders.value.findIndex(o => o.id === editingOrder.value.id);
      orders.value[idx] = res.data;
    } else {
      const res = await axios.post(API, { ...orderForm.value, userId: user.value.id, type: "order" });
      orders.value.push(res.data);
    }
    showModal.value = false;
  } catch (err) {
    console.error("Ошибка сохранения заказа", err);
  }
};

const deleteOrder = async (id: number) => {
  try {
    await axios.delete(`${API}/${id}`);
    orders.value = orders.value.filter(o => o.id !== id);
  } catch (err) {
    console.error("Ошибка удаления заказа", err);
  }
};

const fetchProducts = async () => {
  try {
    const res = await axios.get(`${API}?type=product`);
    products.value = res.data;
  } catch (err) {
    console.error("Ошибка получения товаров", err);
  }
};

const addProduct = () => {
  editingProduct.value = null;
  productForm.value = {
    name: "",
    start_price: 0,
    final_price: 0,
    image: "",
    action_start: "",
    action_end: ""
  };
  showModal.value = true;
};

const editProduct = (product: any) => {
  editingProduct.value = product;
  productForm.value = { ...product };
  showModal.value = true;
};

const deleteProduct = async (id: number) => {
  try {
    await axios.delete(`${API}/${id}`);
    products.value = products.value.filter(p => p.id !== id);
  } catch (err) {
    console.error("Ошибка удаления товара", err);
  }
};

const logout = () => {
  localStorage.removeItem("user");
  localStorage.removeItem("token");
  router.push("/login");
};

const saveProduct = async () => {
  try {
    if (editingProduct.value) {
      const res = await axios.put(`${API}/${editingProduct.value.id}`, {
        ...productForm.value,
        type: "product"
      });
      const idx = products.value.findIndex(p => p.id === editingProduct.value.id);
      products.value[idx] = res.data;
    } else {
      const res = await axios.post(API, { ...productForm.value, type: "product" });
      products.value.push(res.data);
    }

    showModal.value = false;

    router.push("/catalog");

  } catch (err) {
    console.error("Ошибка сохранения товара", err);
  }
};
</script>
