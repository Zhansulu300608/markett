<template>
  <div class="min-h-screen bg-gray-50">
    <!-- Logout -->
    <div class="max-w-7xl mx-auto px-4 py-4 flex justify-end">
      <button
        @click="logout"
        class="flex items-center gap-2 text-sm font-medium text-red-500 hover:text-red-600"
      >
        Выход
      </button>
    </div>

    <main class="max-w-7xl mx-auto px-4 flex gap-8 pb-10">
      <!-- Sidebar -->
      <aside class="w-72 bg-white rounded-2xl shadow-sm p-6">
        <!-- Avatar -->
        <div class="w-28 h-28 rounded-full bg-orange-100 flex items-center justify-center text-3xl font-bold text-orange-600 mx-auto mb-4">
          {{ user.name?.charAt(0) || "?" }}
        </div>

        <!-- User info -->
        <div class="text-center mb-4">
          <p class="font-semibold text-lg">{{ user.name }}</p>
          <p class="text-sm text-gray-500">{{ user.email }}</p>
        </div>

        <!-- Role -->
        <span
          class="block text-center px-4 py-1 text-xs font-semibold text-orange-600 bg-orange-100 rounded-full mb-6"
        >
          Покупатель
        </span>

        <!-- Orders count -->
        <div class="rounded-xl border bg-gray-50 px-4 py-3 flex justify-between items-center mb-6">
          <span class="text-xs text-gray-500">Всего заказов</span>
          <span class="text-2xl font-semibold">7</span>
        </div>

        <!-- Menu -->
        <nav class="space-y-2">
          <NuxtLink
            to="/profile"
            class="block px-4 py-3 rounded-xl bg-orange-50 text-orange-600 font-medium"
          >
            Профиль
          </NuxtLink>

          <NuxtLink
            to="/orders"
            class="block px-4 py-3 rounded-xl text-gray-600 hover:bg-gray-100"
          >
            Мои заказы
          </NuxtLink>
        </nav>
      </aside>

      <!-- Content -->
      <section class="flex-1 bg-white rounded-2xl shadow-sm p-8">
        <h1 class="text-3xl font-bold mb-8">Личные данные</h1>

        <form
          class="grid grid-cols-1 md:grid-cols-2 gap-6 max-w-3xl"
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
              class="px-6 py-3 bg-orange-500 text-white rounded-lg hover:bg-orange-600 disabled:opacity-50"
            >
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
})

const form = reactive({
  name: "",
  email: "",
  phone: "",
  address: "",
})

const API_URL = "https://medical-backend-54hp.onrender.com/api/auth"

const logout = () => {
  localStorage.removeItem("token")
  localStorage.removeItem("user")
  router.push("/login")
}

const loadProfile = async () => {
  const token = localStorage.getItem("token")
  if (!token) return router.push("/login")

  try {
    const res = await axios.get(`${API_URL}/me`, {
      headers: { Authorization: `Bearer ${token}` },
    })

    Object.assign(user, res.data)
    Object.assign(form, res.data)
  } catch {
    logout()
  }
}

const saveProfile = async () => {
  const token = localStorage.getItem("token")
  if (!token) return

  saving.value = true

  try {
    await axios.put(`${API_URL}/update`, form, {
      headers: { Authorization: `Bearer ${token}` },
    })
    alert("Данные сохранены")
  } finally {
    saving.value = false
  }
}

onMounted(loadProfile)
</script>
