<template>
  <div class="flex flex-row min-h-screen">
   
    <div
      v-if="sidebarOpen"
      @click="sidebarOpen = false"
      class="fixed inset-0 bg-black bg-opacity-50 z-40 md:hidden"
    />

  
    <aside class="fixed md:static z-50 md:z-auto inset-y-0 left-0 w-48 md:w-64 bg-gray-100 border-r border-gray-300
  transform transition-transform duration-300
  md:translate-x-0"
  :class="sidebarOpen ? 'translate-x-0' : '-translate-x-full md:translate-x-0'">

      <Sidebar />
    </aside>

   
   <main class="flex-1 p-4 md:p-6 flex flex-col space-y-6 ">
    
      <div class="flex items-center gap-3 mb-4">
      
        <button
          @click="sidebarOpen = true"
          class="md:hidden p-2 rounded-lg border hover:bg-gray-100"
          aria-label="Открыть меню"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            class="h-6 w-6"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M4 6h16M4 12h16M4 18h16"
            />
          </svg>
        </button>

        <div>
          <h2 class="text-2xl md:text-3xl font-bold text-gray-800">
            Панель управления
          </h2>
          <p class="text-gray-600 text-sm md:text-base">
            Добро пожаловать! Здесь представлен обзор вашего бизнеса.
          </p>
        </div>
      </div>

   
      <div
        class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4 md:gap-6"
      >
        <div
          class="bg-white p-4 md:p-5 rounded-lg shadow flex items-center justify-between"
        >
          <div>
            <p class="text-xs md:text-sm text-gray-500">Всего продуктов</p>
            <p class="text-xl md:text-2xl font-bold text-gray-800">
              {{ recentProducts.length }}
            </p>
          </div>
          <div
            class="w-10 h-10 md:w-12 md:h-12 bg-blue-50 rounded-lg flex items-center justify-center"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              class="h-5 w-5 md:h-6 md:w-6 text-blue-600"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M20 7l-8-4-8 4m16 6l-8 4m8-4v10l-8 4m0-10L4 13m8-4v10l8 4"
              />
            </svg>
          </div>
        </div>

        <div
          class="bg-white p-4 md:p-5 rounded-lg shadow flex items-center justify-between"
        >
          <div>
            <p class="text-xs md:text-sm text-gray-500">В наличии</p>
            <p class="text-xl md:text-2xl font-bold text-gray-800">
              {{ inStockCount }}
            </p>
          </div>
          <div
            class="w-10 h-10 md:w-12 md:h-12 bg-green-50 rounded-lg flex items-center justify-center"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              class="h-5 w-5 md:h-6 md:w-6 text-green-600"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M3 3h2l.4 2M7 13h10v8a2 2 0 002 2H5a2 2 0 002-2v-8zm5-1a2 2 0 012 2v6a2 2 0 01-2 2H9a2 2 0 01-2-2v-6a2 2 0 012-2h6z"
              />
            </svg>
          </div>
        </div>

        <div
          class="bg-white p-4 md:p-5 rounded-lg shadow flex items-center justify-between"
        >
          <div>
            <p class="text-xs md:text-sm text-gray-500">Общая стоимость склада</p>
            <p class="text-xl md:text-2xl font-bold text-gray-800">
              ₸{{ totalInventoryValue.toLocaleString() }}
            </p>
          </div>
          <div
            class="w-10 h-10 md:w-12 md:h-12 bg-purple-50 rounded-lg flex items-center justify-center"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              class="h-5 w-5 md:h-6 md:w-6 text-purple-600"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M13 7h8v6h-8V7z"
              />
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M13 13h8v6h-8v-6z"
              />
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M3 13h8v6H3v-6z"
              />
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M3 7h8v6H3V7z"
              />
            </svg>
          </div>
        </div>

        <div
          class="bg-white p-4 md:p-5 rounded-lg shadow flex items-center justify-between"
        >
          <div>
            <p class="text-xs md:text-sm text-gray-500">Категории</p>
            <p class="text-xl md:text-2xl font-bold text-gray-800">13</p>
          </div>
          <div
            class="w-10 h-10 md:w-12 md:h-12 bg-orange-50 rounded-lg flex items-center justify-center"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              class="h-5 w-5 md:h-6 md:w-6 text-orange-600"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M12 4.354a4 4 0 110 5.292M15 21H3v-1a6 6 0 0112 0v1zm0 0h6v-1a6 6 0 00-9-5.196A6 6 0 0012 9a6 6 0 006 6v1z"
              />
            </svg>
          </div>
        </div>
      </div>

      <div class="grid grid-cols-1 lg:grid-cols-3 gap-4 md:gap-6">
        
        <section
          class="lg:col-span-2 bg-white p-4 md:p-6 rounded-lg shadow overflow-auto"
        >
          <div
            class="flex justify-between items-center mb-4 flex-wrap gap-2"
          >
            <h3 class="font-bold text-gray-800 text-lg md:text-xl">
              Недавние продукты
            </h3>
            <NuxtLink
              to="/admin/products"
              class="text-sm text-blue-600 hover:text-blue-800 font-medium flex items-center"
            >
              Смотреть все
              <svg
                xmlns="http://www.w3.org/2000/svg"
                class="h-4 w-4 ml-1"
                fill="none"
                viewBox="0 0 24 24"
                stroke="currentColor"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="2"
                  d="M9 5l7 7-7 7"
                />
              </svg>
            </NuxtLink>
          </div>

          <div class="space-y-3">
            <div
              v-for="product in recentProducts"
              :key="product.id"
              class="flex items-center py-2 border-b border-gray-100 last:border-b-0"
            >
              <img
                :src="product.image"
                :alt="product.name"
                class="h-8 w-8 md:h-10 md:w-10 object-cover rounded mr-3"
              />
              <div class="flex-1 min-w-0">
                <div
                  class="text-sm md:text-base font-medium text-gray-800 truncate"
                  :title="product.name"
                >
                  {{ product.name }}
                </div>
                <div class="text-xs md:text-sm text-gray-500">
                  ₸{{ product.final_price.toLocaleString() }}
                </div>
              </div>
              <span
                :class="[
                  'px-2 py-0.5 text-xs md:text-sm font-medium rounded-full whitespace-nowrap',
                  product.inStock
                    ? 'bg-green-100 text-green-800'
                    : 'bg-red-100 text-red-800',
                ]"
              >
                {{ product.inStock ? 'В наличии' : 'Нет' }}
              </span>
            </div>
          </div>
        </section>

       
        <section
          class="bg-white p-4 md:p-6 rounded-lg shadow flex flex-col justify-between"
        >
          <h3 class="font-bold text-gray-800 mb-4 text-lg md:text-xl">
            Быстрые действия
          </h3>

          <div class="space-y-3">
            <button
              @click="addProduct"
              class="flex items-center px-4 py-3 bg-blue-600 text-white font-medium rounded-lg hover:bg-blue-700 transition-colors w-full"
            >
              <svg
                xmlns="http://www.w3.org/2000/svg"
                class="h-5 w-5 mr-2"
                fill="none"
                viewBox="0 0 24 24"
                stroke="currentColor"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="2"
                  d="M12 6v6m0 0v6m0-6h6m-6 0H6"
                />
              </svg>
              Создать новый продукт
            </button>

            <NuxtLink
              to="/admin/products"
              class="flex items-center px-4 py-3 border border-gray-300 text-gray-700 font-medium rounded-lg hover:bg-gray-50 transition-colors w-full justify-center"
            >
              <svg
                xmlns="http://www.w3.org/2000/svg"
                class="h-5 w-5 mr-2"
                fill="none"
                viewBox="0 0 24 24"
                stroke="currentColor"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="2"
                  d="M20 7l-8-4-8 4m16 6l-8 4m8-4v10l-8 4m0-10L4 13m8-4v10l8 4"
                />
              </svg>
              Смотреть все продукты
            </NuxtLink>
          </div>

          <div class="mt-6 p-4 bg-blue-50 rounded-lg text-sm text-blue-800 flex items-start">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              class="h-5 w-5 text-blue-600 mr-2 mt-0.5 flex-shrink-0"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M9.663 17h4.674M12 3v1m6.364 1.664-.707.707M21 12h-1M4 12H3m3.343-5.657l-.707-.707M21 12a9 9 0 11-18 0 9 9 0 0118 0z"
              />
            </svg>
            <p>
              <strong>Совет:</strong> Обновляйте ваш склад, чтобы отслеживать
              наличие товаров и избегать их отсутствия.
            </p>
          </div>
        </section>
      </div>
    </main>


    <div
      v-if="showModal"
      class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50 p-4"
    >
      <div class="bg-white p-6 rounded w-full max-w-md max-h-[90vh] overflow-auto">
        <h2 class="text-xl font-bold mb-4">Добавить продукт</h2>

        <input
          v-model="productForm.name"
          placeholder="Название"
          class="w-full p-2 border rounded mb-2"
        />
        <input
          v-model.number="productForm.start_price"
          type="number"
          placeholder="Начальная цена"
          class="w-full p-2 border rounded mb-2"
        />
        <input
          v-model.number="productForm.final_price"
          type="number"
          placeholder="Конечная цена"
          class="w-full p-2 border rounded mb-2"
        />
        <input
          v-model="productForm.image"
          placeholder="URL изображения"
          class="w-full p-2 border rounded mb-2"
        />
        <input
          v-model="productForm.action_start"
          placeholder="Дата начала акции"
          class="w-full p-2 border rounded mb-2"
          type="date"
        />
        <input
          v-model="productForm.action_end"
          placeholder="Дата окончания акции"
          class="w-full p-2 border rounded mb-4"
          type="date"
        />

        <div class="flex justify-end gap-2 flex-wrap">
          <button
            @click="showModal = false"
            class="border px-4 py-2 rounded w-full sm:w-auto"
          >
            Отмена
          </button>
          <button
            @click="saveProduct"
            class="bg-blue-600 text-white px-4 py-2 rounded w-full sm:w-auto"
          >
            Создать
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import Sidebar from '@/components/Sidebar.vue'
import { ref, computed, onMounted } from "vue"
import { useRouter } from "#imports"
import axios from "axios"

const sidebarOpen = ref(false)

// Типы
interface Product {
  id?: number
  name: string
  start_price: number
  final_price: number
  image: string
  action_start: string
  action_end: string
  inStock?: boolean
}

const router = useRouter()
const API = "https://6940519c993d68afba6bb782.mockapi.io/market"

const showModal = ref(false)
const productForm = ref<Product>({
  name: "",
  start_price: 0,
  final_price: 0,
  image: "",
  action_start: "",
  action_end: ""
})

const recentProducts = ref<Product[]>([])

const inStockCount = computed(() => recentProducts.value.filter(p => p.inStock).length)
const totalInventoryValue = computed(() => recentProducts.value.reduce((sum, p) => sum + (p.final_price || 0), 0))


const addProduct = () => {
  productForm.value = { name: "", start_price: 0, final_price: 0, image: "", action_start: "", action_end: "" }
  showModal.value = true
}

const saveProduct = async () => {
  try {
    const { data } = await axios.post(API, { ...productForm.value, type: 'product', inStock: true })
    recentProducts.value.unshift({ ...data })
    showModal.value = false
    router.push("/catalog")
  } catch (err) {
    console.error("Ошибка при сохранении продукта", err)
  }
}


const fetchRecentProducts = async () => {
  try {
    const { data } = await axios.get<Product[]>(API)
  
    recentProducts.value = data.slice(-4).reverse()
  } catch (err) {
    console.error("Ошибка при получении продуктов", err)
  }
}


onMounted(() => {
  fetchRecentProducts()
})
</script>
