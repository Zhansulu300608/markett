<template>
    <AppHeader />
  <div class="flex min-h-screen bg-gray-100">

    <!-- SIDEBAR -->
    <aside class="w-64 bg-slate-800 text-white p-6">
      <nav class="space-y-4">
        <NuxtLink to="/dashbord" class="block hover:bg-slate-700 px-4 py-2 rounded">Dashboard</NuxtLink>
        <NuxtLink to="/order" class="block hover:bg-slate-700 px-4 py-2 rounded">Мои заказы</NuxtLink>
        <NuxtLink to="/favourties" class="block hover:bg-slate-700 px-4 py-2 rounded">Избранное</NuxtLink>
      </nav>

      <button @click="logout" class="mt-10 w-full bg-red-500 py-2 rounded">Logout</button>
    </aside>

    <!-- MAIN -->
    <main class="flex-1 p-8">

      <!-- HEADER -->
      <div class="mb-6">
        <h1 class="text-3xl font-bold">Welcome, {{ user?.name }} 👋</h1>
        <p class="text-gray-500">Here is your personal dashboard</p>
      </div>

      <!-- STATS -->
      <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-8">
        <div class="bg-white p-6 rounded shadow">
          <h3 class="text-gray-500">My Orders</h3>
          <p class="text-3xl font-bold">{{ orders.length }}</p>
        </div>

        <div class="bg-white p-6 rounded shadow">
          <h3 class="text-gray-500">Favorites</h3>
          <p class="text-3xl font-bold">{{ favorites.length }}</p>
        </div>

        <div class="bg-white p-6 rounded shadow">
          <h2 class="text-xl font-bold mb-4">My Profile</h2>
          <p><b>Name:</b> {{ user?.name }}</p>
          <p><b>Email:</b> {{ user?.email }}</p>
          <p><b>Role:</b> {{ user?.role }}</p>
        </div>
      </div>

      <!-- ORDERS TABLE -->
  <!-- PRODUCTS -->
<div class="bg-white p-6 rounded shadow mb-8">
  <div class="flex justify-between mb-6">
    <h2 class="text-xl font-bold">Products</h2>
    <button
      @click="addProduct"
      class="bg-blue-600 text-white px-4 py-2 rounded"
    >
      + Add Product
    </button>
  </div>

  <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
    <div
      v-for="product in products"
      :key="product.id"
      class="border rounded-2xl p-4 relative flex flex-col h-[340px] cursor-pointer shadow-sm"
    >
      <!-- Discount label -->
      <span
        v-if="product.discount"
        class="absolute top-3 left-3 bg-[#C1121F] text-white text-sm px-3 py-1 rounded-full"
      >
        -{{ product.discount }}%
      </span>

      <!-- Action end date -->
      <p class="absolute top-3 right-3 text-xs text-gray-400">
        до {{ product.action_end }}
      </p>

      <!-- Product image -->
      <img
        :src="product.image"
        :alt="product.name"
        class="h-36 w-full object-contain bg-gray-100 rounded-xl mb-3"
      />

      <!-- Product name -->
      <h3 class="text-sm font-semibold line-clamp-2 min-h-[40px]">
        {{ product.name }}
      </h3>

      <!-- Prices -->
      <div class="mt-auto pt-3">
        <p class="text-xs line-through text-gray-400">
          {{ product.start_price }} ₸
        </p>
        <p class="bg-yellow-400 px-4 py-2 rounded-xl font-bold text-lg inline-block">
          {{ product.final_price }} ₸
        </p>
      </div>

      <!-- Edit / Delete buttons -->
      <div class="flex justify-between mt-3">
        <button
          @click.stop="editProduct(product)"
          class="bg-yellow-400 px-3 py-1 rounded"
        >
          Edit
        </button>
        <button
          @click.stop="deleteProduct(product.id)"
          class="bg-red-500 text-white px-3 py-1 rounded"
        >
          Delete
        </button>
      </div>
    </div>

    <!-- No products message -->
    <p
      v-if="products.length === 0"
      class="text-gray-500 col-span-full text-center"
    >
      No products yet
    </p>
  </div>
</div>



    </main>

 <!-- MODAL -->
<div
  v-if="showModal"
  class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50"
>
  <div class="bg-white p-6 rounded w-96">
    <h2 class="text-xl font-bold mb-4">
      {{ editingProduct ? 'Edit Product' : 'Add Product' }}
    </h2>

    <input v-model="productForm.name" placeholder="Name"
      class="w-full p-2 border rounded mb-2" />

    <input v-model.number="productForm.start_price" type="number"
      placeholder="Start price"
      class="w-full p-2 border rounded mb-2" />

    <input v-model.number="productForm.final_price" type="number"
      placeholder="Final price"
      class="w-full p-2 border rounded mb-2" />

    <input v-model="productForm.image" placeholder="Image URL"
      class="w-full p-2 border rounded mb-2" />

    <input v-model="productForm.action_start" placeholder="Action start"
      class="w-full p-2 border rounded mb-2" />

    <input v-model.number="productForm.action_end" type="number"
      placeholder="Action end"
      class="w-full p-2 border rounded mb-4" />

    <div class="flex justify-end gap-2">
      <button @click="showModal=false" class="border px-4 py-2 rounded">
        Cancel
      </button>
      <button @click="saveProduct"
        class="bg-blue-600 text-white px-4 py-2 rounded">
        {{ editingProduct ? 'Update' : 'Create' }}
      </button>
    </div>
  </div>
</div>

  </div>
</template><script setup lang="ts">
import { ref, onMounted } from "vue";
import axios from "axios";
import { useRouter } from "vue-router";

const router = useRouter();
const API = "https://67cbeea23395520e6af6ab52.mockapi.io/create";

const user = ref<any>(null);

// Orders
const orders = ref<any[]>([]);
const orderForm = ref({ title: "", status: "Pending" });
const editingOrder = ref<any>(null);

// Products
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

const showModal = ref(false); // Shared modal for simplicity

// Favorites (автоматты)
const favorites = ref<any[]>([]);

onMounted(async () => {
  const savedUser = localStorage.getItem("user");
  if (!savedUser) return router.push("/login");

  user.value = JSON.parse(savedUser);

  // Orders & Products fetch
  fetchOrders();
  fetchProducts();

  // Favorites автоматты (мысал)
  favorites.value = [
    { id: 1, title: "Product A" },
    { id: 2, title: "Product B" },
    { id: 3, title: "Product C" }
  ];
});

// --- Orders CRUD ---
const fetchOrders = async () => {
  try {
    const res = await axios.get(`${API}?userId=${user.value.id}&type=order`);
    orders.value = res.data;
  } catch (err) {
    console.error("Orders fetch error", err);
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
    console.error("Save order error", err);
  }
};

const deleteOrder = async (id: number) => {
  try {
    await axios.delete(`${API}/${id}`);
    orders.value = orders.value.filter(o => o.id !== id);
  } catch (err) {
    console.error("Delete order error", err);
  }
};

// --- Products CRUD ---
const fetchProducts = async () => {
  try {
    const res = await axios.get(`${API}?type=product`);
    products.value = res.data;
  } catch (err) {
    console.error("Products fetch error", err);
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
  } catch (err) {
    console.error("Save product error", err);
  }
};

const deleteProduct = async (id: number) => {
  try {
    await axios.delete(`${API}/${id}`);
    products.value = products.value.filter(p => p.id !== id);
  } catch (err) {
    console.error("Delete product error", err);
  }
};

// --- Logout ---
const logout = () => {
  localStorage.removeItem("user");
  localStorage.removeItem("token");
  router.push("/login");
};
</script>
