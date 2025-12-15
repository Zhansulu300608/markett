<template>
  <div class="bg-gray-100 text-gray-900 min-h-screen flex items-center justify-center p-6">
    <div class="w-full max-w-md p-10 rounded-xl shadow-lg bg-white border border-purple-200">

      <!-- Logo -->
      <div class="flex items-center justify-center mb-6">
        <img src="/images/logoo.png" alt="Logo" class="h-10" />
      </div>

      <!-- Title -->
      <h1 class="text-3xl font-bold text-center mb-2">
        Добро пожаловать<br />обратно!
      </h1>
      <p class="text-gray-500 text-center mb-6">
        Войдите в свой аккаунт, чтобы продолжить.
      </p>

      <!-- Error -->
      <div v-if="error" class="mb-4 p-3 rounded-lg bg-red-100 border border-red-300">
        <p class="text-red-600 text-sm text-center font-semibold">{{ error }}</p>
      </div>

      <!-- Success -->
      <div v-if="success" class="mb-4 p-3 rounded-lg bg-green-100 border border-green-300">
        <p class="text-green-600 text-sm text-center font-semibold">{{ success }}</p>
      </div>

      <!-- Form -->
      <form @submit.prevent="handleSubmit" class="flex flex-col gap-3">
        <label class="font-semibold">Электронная почта</label>
        <input
          v-model="form.email"
          type="email"
          required
          placeholder="example@mail.com"
          class="p-3 rounded-lg border border-gray-300 focus:border-[#C1121F] outline-none"
          :disabled="loading"
        />

        <label class="font-semibold">Пароль</label>
        <input
          v-model="form.password"
          type="password"
          required
          placeholder="********"
          class="p-3 rounded-lg border border-gray-300 focus:border-[#C1121F] outline-none"
          :disabled="loading"
        />

        <button
          type="submit"
          :disabled="loading"
          class="mt-4 bg-[#C1121F] hover:bg-[#780000] text-white py-3 rounded-lg font-semibold transition disabled:opacity-50"
        >
          {{ loading ? "Вход..." : "Войти" }}
        </button>
      </form>

      <p class="mt-5 text-center text-sm">
        Еще нет аккаунта?
        <NuxtLink to="/register" class="text-[#C1121F] hover:underline font-semibold">
          Зарегистрироваться
        </NuxtLink>
      </p>
    </div>
  </div>
</template>

<script setup lang="ts">
import { reactive, ref } from "vue";
import axios from "axios";
import { useRouter } from "vue-router";

definePageMeta({ name: "login" });

const router = useRouter();
const loading = ref(false);

const form = reactive({
  email: "",
  password: "",
});

const error = ref("");
const success = ref("");const handleSubmit = async () => {
  error.value = "";
  success.value = "";
  loading.value = true;

  try {
    const { data } = await axios.post(
      "https://medical-backend-54hp.onrender.com/api/auth/login",
      form
    );

    if (!data?.data?.token) {
      throw new Error("Токен не найден");
    }

    // ✅ Сен сұраған формат
    localStorage.setItem("token", data.data.token);
    localStorage.setItem("user", JSON.stringify(data.data.user));

    success.value = "Вход выполнен успешно!";
    form.email = "";
    form.password = "";

    setTimeout(() => {
      router.push("/profile");
    }, 500);
  } catch (err: any) {
    error.value =
      err.response?.data?.message || "Ошибка входа. Проверьте данные.";
  } finally {
    loading.value = false;
  }
};


</script>
