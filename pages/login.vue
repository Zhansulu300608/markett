<template>
  <div class="min-h-screen flex items-center justify-center bg-gray-100 px-4 py-10">
    <div class="w-full max-w-md bg-white p-10 rounded-2xl shadow-lg">
      <h1 class="text-3xl font-bold text-center mb-6">Войти</h1>

      <p v-if="error" class="bg-red-100 text-red-700 text-center py-2 rounded mb-4">
        {{ error }}
      </p>

      <form @submit.prevent="handleLogin" class="space-y-4">
        <div>
          <label class="block text-sm font-semibold mb-1">Email</label>
          <input
            v-model="form.email"
            type="email"
            placeholder="example@email.com"
            class="w-full border rounded-lg px-3 py-2 focus:outline-none focus:ring-2 focus:ring-blue-400"
          />
        </div>

        <div>
          <label class="block text-sm font-semibold mb-1">Пароль</label>
          <input
            v-model="form.password"
            type="password"
            placeholder="Пароль"
            class="w-full border rounded-lg px-3 py-2 focus:outline-none focus:ring-2 focus:ring-blue-400"
          />
        </div>

        <button
          type="submit"
          class="w-full bg-[#C1121F] hover:bg-[#780000] text-white py-2 rounded-lg text-lg transition"
        >
          Войти
        </button>
      </form>

      <p class="text-center text-sm text-gray-600 mt-6">
        Нет аккаунта?
        <NuxtLink to="/register" class="text-[#C1121F] hover:underline">Регистрация</NuxtLink>
      </p>
    </div>
  </div>
</template>

<script setup lang="ts">
import { reactive, ref } from "vue"
import axios from "axios"
import { useRouter } from "vue-router"

const router = useRouter()
const form = reactive({ email: "", password: "" })
const error = ref("")

const handleLogin = async () => {
  error.value = ""
  try {
    const res = await axios.post(
      "https://medical-backend-54hp.onrender.com/api/auth/login",
      form
    )

    if (res.data?.token) {
      localStorage.setItem("token", res.data.token)
      router.push("/profile")
    }
  } catch (err: any) {
    error.value = err.response?.data?.message || "Ошибка входа"
  }
}
</script>
