<template>
  <div class="min-h-screen flex items-center justify-center bg-gray-100 px-4 py-10">
    <div
      class="grid md:grid-cols-2 bg-white shadow-xl rounded-2xl overflow-hidden max-w-5xl w-full"
    >
    
      <div class="p-10">
        <h1 class="text-3xl font-bold text-center mb-6">Зарегистрировать аккаунт</h1>

        
        <p
          v-if="success"
          class="bg-green-100 text-green-700 text-center py-3 rounded-lg mb-4"
        >
          {{ success }}
        </p>

 
        <p
          v-if="error"
          class="bg-red-100 text-red-700 text-center py-3 rounded-lg mb-4"
        >
          {{ error }}
        </p>

        <form @submit.prevent="handleSubmit" class="space-y-5">
          
          <div>
            <label class="block text-sm font-semibold mb-1">Имя</label>
            <input
              v-model="form.name"
              type="text"
              placeholder="Введите ваше имя"
              class="w-full border rounded-lg px-3 py-3 outline-none focus:ring-2 focus:ring-blue-400"
            />
          </div>

     
          <div>
            <label class="block text-sm font-semibold mb-1">Email</label>
            <input
              v-model="form.email"
              type="email"
              placeholder="example@email.com"
              class="w-full border rounded-lg px-3 py-3 outline-none focus:ring-2 focus:ring-blue-400"
            />
          </div>

        
          <div>
            <label class="block text-sm font-semibold mb-1">Пароль</label>
            <input
              v-model="form.password"
              type="password"
              placeholder="Создайте пароль"
              class="w-full border rounded-lg px-3 py-3 outline-none focus:ring-2 focus:ring-blue-400"
            />
          </div>

        
          <div>
            <label class="block text-sm font-semibold mb-1">Подтвердите пароль</label>
            <input
              v-model="form.confirmPassword"
              type="password"
              placeholder="Повторите пароль"
              class="w-full border rounded-lg px-3 py-3 outline-none focus:ring-2 focus:ring-blue-400"
            />
          </div>

      
          <button
            type="submit"
            class="w-full bg-[#C1121F] hover:bg-[#780000] text-white py-3 rounded-lg text-lg transition"
          >
            Зарегистрироваться
          </button>

        
       
        </form>

        <p class="text-center text-sm text-gray-600 mt-6">
          Уже есть аккаунт?
          <a href="/login" class="text-[#C1121F] hover:underline">Войти</a>
        </p>
      </div>

      
      <div class="hidden md:flex bg-gray-200 items-center justify-center p-10 text-center">
        <p class="text-gray-700 text-lg leading-relaxed">
          Ваш личный помощник в мире финансов.<br />
          Зарегистрируйтесь и управляйте своими<br />
          средствами легко и эффективно.
        </p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { reactive, ref } from "vue";
import axios from "axios";
import { useRouter } from "vue-router";

const router = useRouter();

const form = reactive({
  name: "",
  email: "",
  password: "",
  confirmPassword: "",
});

const error = ref("");
const success = ref("");

const handleSubmit = async () => {
  error.value = "";
  success.value = "";

  if (form.password !== form.confirmPassword) {
    error.value = "Пароли не совпадают";
    return;
  }

  try {
    const response = await axios.post(
      "https://medical-backend-54hp.onrender.com/api/auth/register",
      {
        name: form.name,
        email: form.email,
        password: form.password,
      }
    );

    success.value = "Регистрация успешна!";

    if (response.data?.token) {
      localStorage.setItem("token", response.data.token);
    }

    form.name = "";
    form.email = "";
    form.password = "";
    form.confirmPassword = "";

    setTimeout(() => router.push("/login"), 1200);
  } catch (err) {
    error.value = err.response?.data?.message || "Ошибка регистрации";
  }
};
</script>
