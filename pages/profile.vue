<template>
  <div class="min-h-screen bg-gray-50">
    <!-- Top bar -->
    <div class="max-w-7xl mx-auto px-4 py-4 flex justify-end">
      <button
        @click="logout"
        class="flex items-center gap-2 text-sm font-medium text-red-500 hover:text-red-600"
      >
        <span class="hidden sm:inline">Выход</span>
        <svg
          xmlns="http://www.w3.org/2000/svg"
          class="w-5 h-5"
          fill="none"
          viewBox="0 0 24 24"
          stroke="currentColor"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="1.5"
            d="M15.75 9V5.25A2.25 2.25 0 0013.5 3h-6A2.25 2.25 0 005.25 5.25v13.5A2.25 2.25 0 007.5 21h6a2.25 2.25 0 002.25-2.25V15M9 12h12m0 0l-3 3m3-3l-3-3"
          />
        </svg>
      </button>
    </div>

    <main class="max-w-7xl mx-auto px-4 flex gap-8 py-10">
      <!-- Sidebar -->
      <aside class="w-72 bg-white rounded-2xl shadow-sm p-6">
        <!-- Avatar -->
        <div class="w-28 h-28 rounded-full overflow-hidden bg-gray-200 mb-4 mx-auto">
          <div class="w-full h-full bg-gray-300" />
        </div>

        <!-- Name & email -->
        <div class="text-center mb-4">
          <p class="font-semibold text-lg">{{ user.name }}</p>
          <p class="text-sm text-gray-500">{{ user.email }}</p>
        </div>

        <!-- Role -->
        <span
          class="flex justify-center px-4 py-1 text-xs font-semibold text-orange-600 bg-orange-100 rounded-full mb-6"
        >
          Покупатель
        </span>

        <!-- Stats -->
        <div
          class="w-full mb-6 rounded-xl border bg-gray-50 px-4 py-3 flex justify-between items-center"
        >
          <div class="text-xs text-gray-500">Всего заказов</div>
          <div class="text-2xl font-semibold">{{ ordersCount }}</div>
        </div>

        <!-- Menu -->
        <nav class="space-y-2">
          <NuxtLink
            to="/profile"
            class="flex items-center gap-3 px-4 py-3 rounded-xl bg-gray-100 font-medium"
          >
            Профиль
          </NuxtLink>

          <NuxtLink
            to="/order"
            class="flex items-center gap-3 px-4 py-3 rounded-xl text-gray-600 hover:bg-gray-100"
          >
            Мои заказы
          </NuxtLink>
        </nav>
      </aside>

      <!-- Content -->
      <section class="flex-1">
        <h1 class="text-3xl font-bold mb-8">Личные данные</h1>

        <form
          @submit.prevent="onSubmit"
          class="grid grid-cols-1 md:grid-cols-2 gap-6 max-w-4xl"
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

          <div class="md:col-span-2 mt-4">
            <button
              type="submit"
              class="px-6 py-3 bg-orange-500 text-white rounded-lg hover:bg-orange-600"
            >
              Сохранить изменения
            </button>
          </div>
        </form>
      </section>
    </main>
  </div>
</template>

<script setup lang="ts">
import { reactive, ref, onMounted } from "vue"
import axios from "axios"
import { useRouter } from "vue-router"
const token = localStorage.getItem("token")

const router = useRouter()

const user = reactive({
  name: "",
  email: "",
})

const form = reactive({
  name: "",
  email: "",
  phone: "",
  address: "",
})

const ordersCount = ref(0)

const logout = () => {
  localStorage.removeItem("token")
  router.push("/login")
}

onMounted(async () => {
  const token = localStorage.getItem("token")
  if (!token) return router.push("/login")

  try {
    const res = await axios.get(
      "https://medical-backend-54hp.onrender.com/api/auth/me",
      {
        headers: { Authorization: `Bearer ${token}` },
      }
    )

    user.name = res.data.name
    user.email = res.data.email

    form.name = res.data.name
    form.email = res.data.email
    form.phone = res.data.phone || ""
    form.address = res.data.address || ""

    ordersCount.value = res.data.ordersCount || 0
  } catch (e) {
    localStorage.removeItem("token")
    router.push("/login")
  }
})

const onSubmit = async () => {
  const token = localStorage.getItem("token")

  await axios.put(
    "https://medical-backend-54hp.onrender.com/api/auth/update",
    form,
    {
      headers: { Authorization: `Bearer ${token}` },
    }
  )

  alert("Данные обновлены")
}
</script>
