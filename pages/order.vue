<template>
  <AppHeader />
  <slot />

  <div class="min-h-screen bg-gray-50 py-10">
    <div class="max-w-7xl mx-auto px-4 flex flex-col md:flex-row gap-8">

      <aside class="w-full md:w-72 bg-white rounded-2xl shadow-sm p-6">
        <div
          class="w-28 h-28 rounded-full bg-[#FDF0D5] flex items-center justify-center text-3xl font-bold text-[#003049] mx-auto mb-4"
        >
          {{ user.name?.charAt(0) || "?" }}
        </div>

        <div class="text-center mb-4">
          <p class="font-semibold text-lg">{{ user.name }}</p>
          <p class="text-sm text-gray-500">{{ user.email }}</p>
        </div>

        <span
          class="block text-center px-4 py-1 text-xs font-semibold text-[#003049] bg-[#FDF0D5] rounded-full mb-6"
        >
          {{ user.role === "admin" ? "Админ" : "Покупатель" }}
        </span>

        <div
          class="rounded-xl border bg-gray-50 px-4 py-3 flex justify-between items-center mb-6"
        >
          <span class="text-xs text-gray-500">Всего заказов</span>
          <span class="text-2xl font-semibold">7</span>
        </div>

        <nav class="space-y-2">
          <NuxtLink
            to="/profile"
            class="block px-4 py-3 rounded-xl bg-[#003049] text-[#ffffff] hover:bg-[#FDF0D5] hover:text-[#003049] font-medium"
          >
            Профиль
          </NuxtLink>
          <NuxtLink
            to="/order"
            class="block px-4 py-3 rounded-xl text-[#ffffff] bg-[#003049] hover:bg-[#FDF0D5] hover:text-[#003049]"
          >
            Мои заказы
          </NuxtLink>

          <NuxtLink
            v-if="user.role === 'admin'"
            to="/admin"
            class="block px-4 py-3 rounded-xl text-red-600 hover:bg-gray-100 font-semibold"
          >
            Админ панель
          </NuxtLink>
        </nav>
      </aside>

      <main class="flex-1 bg-white rounded-2xl shadow-sm p-6 md:p-8">
        <div class="max-w-6xl mx-auto">
          <h1 class="text-2xl font-bold mb-6">Мои заказы</h1>

          <div v-if="orders.length === 0" class="text-gray-500">
            Заказов пока нет
          </div>

          <div v-else>
            <div
              class="mb-6 flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4"
            >
              <p class="text-lg font-semibold">
                Общая сумма:
                <span class="text-[#003049]">{{ totalPrice }} тг</span>
              </p>

              <button
                @click="sendToWhatsApp"
                class="bg-green-500 hover:bg-green-600 text-white px-6 py-3 rounded-xl font-semibold transition"
              >
                Оформить заказ
              </button>
            </div>

            <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-4 gap-6">
              <div
                v-for="item in orders"
                :key="item.id"
                class="border rounded-xl p-4 relative"
              >
                <button
                  @click="removeOrder(item.id)"
                  class="absolute top-2 right-2 text-gray-400 hover:text-red-600"
                >
                  ✕
                </button>

                <img
                  :src="item.image || '/images/no-image.png'"
                  alt="product"
                  class="h-32 w-full object-contain mb-3 bg-gray-100 rounded"
                />

                <p class="text-sm font-semibold line-clamp-2">
                  {{ item.name || "Без названия" }}
                </p>

                <p class="font-bold mt-2">{{ item.final_price ?? 0 }} тг</p>
              </div>
            </div>
          </div>
        </div>
      </main>
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
            <NuxtLink to="/dashbord" class="hover:text-[#003049]">Дашборд</NuxtLink>
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

        <h3 class="text-white font-semibold mb-4">Мы в социальных сетях</h3>

        <div class="flex flex-wrap gap-3 mb-8">
          <a href="https://twitter.com" target="_blank" rel="noopener" class="w-6 h-6">
            <img src="/images/icons8-phone-50.png" alt="Twitter" class="w-full h-full object-contain" />
          </a>
          <a href="https://twitter.com" target="_blank" rel="noopener" class="w-6 h-6">
            <img src="/images/icons8-whatsapp-logo-50.png" alt="Twitter" class="w-full h-full object-contain" />
          </a>
          <a href="https://twitter.com" target="_blank" rel="noopener" class="w-6 h-6">
            <img src="/images/icons8-email-50.png" alt="Twitter" class="w-full h-full object-contain" />
          </a>
          <a href="https://twitter.com" target="_blank" rel="noopener" class="w-6 h-6">
            <img src="/images/icons8-tiktok-50.png" alt="Twitter" class="w-full h-full object-contain" />
          </a>
          <a href="https://twitter.com" target="_blank" rel="noopener" class="w-6 h-6">
            <img src="/images/icons8-instagram-logo-50.png" alt="Twitter" class="w-full h-full object-contain" />
          </a>

          <a href="https://instagram.com" target="_blank" rel="noopener" class="w-6 h-6"></a>
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
import { reactive, ref, onMounted, computed } from "vue"
import { useRouter } from "vue-router"
import axios from "axios"

definePageMeta({ name: "profile-orders" })

const router = useRouter()
const saving = ref(false)
const user = reactive({
  name: "",
  email: "",
  role: "user",
})

const form = reactive({
  name: "",
  email: "",
  phone: "",
  address: "",
})

const API_URL = "https://medical-backend-54hp.onrender.com/api/auth"

const loadProfile = async () => {
  const token = localStorage.getItem("token")
  if (!token) return router.push("/profile")

  try {
    const res = await axios.get(`${API_URL}/me`, {
      headers: { Authorization: `Bearer ${token}` },
    })

    const profile = res.data.data || res.data.data?.user

    Object.assign(user, profile)
    Object.assign(form, profile)

    localStorage.setItem("user", JSON.stringify(profile))
  } catch (err) {
    console.error(err)
    logout()
  }
}

const sendToWhatsApp = () => {
  if (!orders.value.length) return

  let message = " Мой заказ:%0A%0A"

  orders.value.forEach((item, index) => {
    message += `${index + 1}. ${item.name || "Без названия"} — ${item.final_price ?? 0} тг%0A`
  })

  message += `%0A Итого: ${totalPrice.value} тг`

  const phone = "77766248255"
  const url = `https://wa.me/${phone}?text=${message}`

  window.open(url, "_blank")
}

const saveProfile = async () => {
  const token = localStorage.getItem("token")
  if (!token) return

  saving.value = true
  try {
    const { data } = await axios.put(`${API_URL}/update`, form, {
      headers: { Authorization: `Bearer ${token}` },
    })

    Object.assign(user, data)
    localStorage.setItem("user", JSON.stringify(data))

    alert("Данные сохранены")
  } catch (err) {
    console.error(err)
    alert("Ошибка при сохранении")
  } finally {
    saving.value = false
  }
}

onMounted(loadProfile)

const logout = () => {
  localStorage.removeItem("token")
  localStorage.removeItem("user")
  router.push("/login")
}

const orders = ref<any[]>([])

onMounted(() => {
  const saved = localStorage.getItem("orders")
  orders.value = saved ? JSON.parse(saved) : []
})

const removeOrder = (id: string | number) => {
  orders.value = orders.value.filter((item) => item.id !== id)
  localStorage.setItem("orders", JSON.stringify(orders.value))
}

const totalPrice = computed(() => {
  return orders.value.reduce((sum, item) => sum + Number(item.final_price || 0), 0)
})
</script>
