<template>
  <div class="bg-gray-100 text-gray-900 min-h-screen flex items-center justify-center p-6">

    <div class="w-full max-w-md p-10 rounded-xl shadow-lg relative bg-white border border-purple-200">

      <!-- Logo -->
      <div class="flex items-center justify-center gap-3 mb-6">
      <img src="../public/images/logoo.png" alt="Logo" class="h-10" />
      </div>

      <!-- Title -->
      <h1 class="text-3xl font-bold leading-tight mb-2 text-center">
        Добро пожаловать<br />обратно!
      </h1>

      <p class="text-gray-500 text-center mb-6">
        Войдите в свой аккаунт, чтобы продолжить.
      </p>

      <!-- Errors -->
      <p
        v-if="error"
        class="text-red-500 text-center mb-2 font-semibold"
      >{{ error }}</p>

      <p
        v-if="success"
        class="text-green-500 text-center mb-2 font-semibold"
      >{{ success }}</p>

      <!-- Form -->
      <form @submit.prevent="handleSubmit" class="flex flex-col gap-3">
        <label class="font-semibold">Электронная почта</label>
        <input
          type="email"
          v-model="form.email"
          placeholder="ваша@почта.рф"
          class="p-3 rounded-lg border border-gray-300 bg-transparent focus:border-[#C1121F] outline-none"
        />

        <label class="font-semibold">Пароль</label>
        <input
          type="password"
          v-model="form.password"
          placeholder="********"
          class="p-3 rounded-lg border border-gray-300 bg-transparent focus:border-[#C1121F] outline-none"
        />

        <button
          type="submit"
          class="mt-3 bg-[#C1121F] hover:bg-[#780000] text-white py-3 rounded-lg font-semibold transition"
        >
          Войти
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

<script setup>
import { reactive, ref } from "vue";
import axios from "axios";
import { useRouter } from "vue-router";
definePageMeta({ name: "login" })
const router = useRouter();

const form = reactive({
  email: "",
  password: "",
});

const error = ref("");
const success = ref("");

// LOGIN
const handleSubmit = async () => {
  error.value = "";
  success.value = "";

  try {
    const res = await axios.post(
      "https://medical-backend-54hp.onrender.com/api/auth/login",
      form
    );

    success.value = "Got it! You are logged in.";

    if (res.data.token) {
      localStorage.setItem("token", res.data.token);
    }

    form.email = "";
    form.password = "";

    setTimeout(() => {
      router.push('/profile')
    }, 1500);

  } catch (e) {
    error.value = e?.response?.data?.message || "Login failed";
  }
};
</script>
