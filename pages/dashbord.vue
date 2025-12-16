<template>
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
          <p class="text-3xl font-bold">3</p>
        </div>

        <div class="bg-white p-6 rounded shadow">
          <h3 class="text-gray-500">My Posts</h3>
          <p class="text-3xl font-bold">8</p>
        </div>
      </div>

      <!-- PROFILE + ORDERS TABLE -->
      <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
        <!-- PROFILE -->
        <div class="bg-white p-6 rounded shadow">
          <h2 class="text-xl font-bold mb-4">My Profile</h2>
          <p><b>Name:</b> {{ user?.name }}</p>
          <p><b>Email:</b> {{ user?.email }}</p>
          <p><b>Role:</b> {{ user?.role }}</p>
        </div>

        <!-- ORDERS TABLE -->
        <div class="lg:col-span-2 bg-white p-6 rounded shadow">
          <div class="flex justify-between mb-4">
            <h2 class="text-xl font-bold">Мои заказы</h2>
            <button class="bg-blue-600 text-white px-4 py-2 rounded">Add Order</button>
          </div>

          <table class="w-full border">
            <thead class="bg-gray-100">
              <tr>
                <th class="p-2 border">ID</th>
                <th class="p-2 border">Item</th>
                <th class="p-2 border">Status</th>
                <th class="p-2 border">Actions</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="order in orders" :key="order.id">
                <td class="p-2 border">{{ order.id }}</td>
                <td class="p-2 border">{{ order.title }}</td>
                <td class="p-2 border">{{ order.status }}</td>
                <td class="p-2 border space-x-2">
                  <button class="bg-yellow-400 px-3 py-1 rounded">Edit</button>
                  <button @click="deleteOrder(order.id)" class="bg-red-500 text-white px-3 py-1 rounded">Delete</button>
                </td>
              </tr>
              <tr v-if="orders.length === 0">
                <td colspan="4" class="text-center p-4 text-gray-500">No orders yet</td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>

    </main>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from "vue";
import axios from "axios";
import { useRouter } from "vue-router";

const router = useRouter();
const API = "https://YOUR_MOCKAPI/orders"; // Өз API сілтемеңді қой

const user = ref<any>(null);
const orders = ref<any[]>([]);

onMounted(async () => {
  const savedUser = localStorage.getItem("user");
  if (!savedUser) return router.push("/login");

  user.value = JSON.parse(savedUser);

  // Orders тек осы user-ға арналған
  try {
    const res = await axios.get(`${API}?userId=${user.value.id}`);
    orders.value = res.data;
  } catch (err) {
    console.error("Orders fetch error", err);
  }
});

const deleteOrder = async (id: number) => {
  try {
    await axios.delete(`${API}/${id}`);
    orders.value = orders.value.filter((o) => o.id !== id);
  } catch (err) {
    console.error("Delete order error", err);
  }
};

const logout = () => {
  localStorage.removeItem("user");
  localStorage.removeItem("token");
  router.push("/login");
};
</script>
