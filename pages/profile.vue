<template>
  <div class="min-h-screen bg-gray-100 flex items-center justify-center p-4">
    <div class="w-full max-w-md bg-white rounded-xl shadow-lg p-6">

      <!-- Header -->
      <div class="flex justify-between items-center mb-6">
        <h2 class="text-lg font-semibold text-gray-800">
          Профиль покупателя
        </h2>

        <button
          @click="logout"
          class="text-sm font-medium text-red-500 hover:text-red-600"
        >
          Выход
        </button>
      </div>

      <!-- Profile data -->
      <div v-if="loaded" class="space-y-4">
        <div>
          <p class="text-sm text-gray-500">Имя</p>
          <p class="font-medium text-gray-800">{{ user.name }}</p>
        </div>

        <div>
          <p class="text-sm text-gray-500">Email</p>
          <p class="font-medium text-gray-800">{{ user.email }}</p>
        </div>

        <div>
          <p class="text-sm text-gray-500">Телефон</p>
          <p class="font-medium text-gray-800">
            {{ user.phone || "—" }}
          </p>
        </div>
      </div>

      <!-- Loading -->
      <div v-else class="text-center text-gray-500 py-10">
        Загрузка профиля...
      </div>

    </div>
  </div>
</template>

<script setup>
import { reactive, ref, onMounted } from "vue"
import { useRouter } from "vue-router"
import axios from "axios"

const router = useRouter()

/* ===============================
   USER STATE
================================ */
const user = reactive({
  name: "",
  email: "",
  phone: "",
})

const loaded = ref(false)

/* ===============================
   LOAD PROFILE (ME API)
================================ */
const loadProfile = async () => {
  const token = localStorage.getItem("token")

  if (!token) {
    router.push("/profile")
    return
  }

  try {
    const response = await axios.get(
      "https://medical-backend-54hp.onrender.com/api/auth/me",
      {
        headers: {
          Authorization: `Bearer ${token}`,
        },
      }
    )

    Object.assign(user, response.data)
    loaded.value = true

  } catch (error) {
    console.error("Profile error:", error)
    logout()
  }
}

/* ===============================
   LOGOUT
================================ */
const logout = () => {
  localStorage.removeItem("token")
  router.push("/profile")
}

/* ===============================
   ON MOUNT
================================ */
onMounted(() => {
  loadProfile()
})
</script>
