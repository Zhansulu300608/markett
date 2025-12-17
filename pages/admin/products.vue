<template>
  <NuxtLayout>
    <div class="flex min-h-screen bg-gray-50 overflow-hidden">
      <div
        v-if="sidebarOpen"
        @click="sidebarOpen = false"
        class="fixed inset-0 bg-black/50 z-40 md:hidden"
      />

      <aside
        class="fixed md:static inset-y-0 left-0 z-50 md:z-auto w-64 bg-white shadow-lg
        transform transition-transform duration-300 md:translate-x-0"
        :class="sidebarOpen ? 'translate-x-0' : '-translate-x-full'"
      >
        <Sidebar />
      </aside>

      <div class="flex-1 flex flex-col w-full">
        <header class="md:hidden flex items-center gap-3 px-4 py-3 bg-white shadow">
          <button @click="sidebarOpen = true">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                d="M4 6h16M4 12h16M4 18h16" />
            </svg>
          </button>
          <h1 class="font-semibold text-lg">Продукты</h1>
        </header>

        <main class="flex-1 p-4 md:p-6 overflow-x-hidden">
          <div class="mb-6">
            <h2 class="text-2xl md:text-3xl font-bold text-gray-800 mb-2">
              Продукты
            </h2>
            <p class="text-gray-600 text-sm">
              Управление каталогом продуктов ({{ filteredProducts.length }} товаров)
            </p>
          </div>

          <div class="bg-white p-4 md:p-5 rounded-xl shadow-md mb-6">
            <div class="flex flex-col md:flex-row gap-4">
              <input
                v-model="searchTerm"
                placeholder="Поиск продуктов..."
                class="flex-1 px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500"
              />

              <select
                v-model="selectedCategory"
                class="px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500"
              >
                <option value="">Все категории</option>
                <option v-for="cat in categories" :key="cat" :value="cat">
                  {{ cat }}
                </option>
              </select>
            </div>
          </div>

          <div class="overflow-x-auto bg-white rounded-xl shadow-md">
            <table class="min-w-[900px] w-full divide-y divide-gray-200">
              <thead class="bg-gray-100">
                <tr class="text-xs text-gray-500 uppercase">
                  <th class="px-6 py-3">Изображение</th>
                  <th class="px-6 py-3">Название</th>
                  <th class="px-6 py-3">Цена</th>
                  <th class="px-6 py-3">Наличие</th>
                  <th class="px-6 py-3">Категория</th>
                  <th class="px-6 py-3">Действия</th>
                </tr>
              </thead>

              <tbody class="divide-y">
                <tr
                  v-for="product in filteredProducts"
                  :key="product.id"
                  class="hover:bg-gray-50"
                >
                  <td class="px-6 py-4">
                    <img :src="product.image" class="h-12 w-12 rounded object-cover border" />
                  </td>

                  <td class="px-6 py-4">
                    <input
                      v-if="editId === product.id"
                      v-model="product.name"
                      class="px-2 py-1 border rounded w-full"
                    />
                    <span v-else>{{ product.name }}</span>
                  </td>

                  <td class="px-6 py-4">
                    <input
                      v-if="editId === product.id"
                      v-model.number="product.price"
                      type="number"
                      class="px-2 py-1 border rounded w-full"
                    />
                    <span v-else>₸{{ product.price.toLocaleString() }}</span>
                  </td>

                  <td class="px-6 py-4">
                    <select
                      v-if="editId === product.id"
                      v-model="product.stock"
                      class="px-2 py-1 border rounded w-full"
                    >
                      <option value="Есть">Есть</option>
                      <option value="Нет">Нет</option>
                    </select>
                    <span v-else :class="product.stock === 'Нет' ? 'text-red-500' : ''">
                      {{ product.stock }}
                    </span>
                  </td>

                  <td class="px-6 py-4">
                    <select
                      v-if="editId === product.id"
                      v-model="product.category"
                      class="px-2 py-1 border rounded w-full"
                    >
                      <option v-for="cat in categories" :key="cat" :value="cat">
                        {{ cat }}
                      </option>
                    </select>
                    <span v-else>{{ product.category }}</span>
                  </td>

                  <td class="px-6 py-4 flex gap-2">
                    <button
                      v-if="editId !== product.id"
                      @click="startEdit(product.id)"
                      class="text-blue-600 hover:underline"
                    >
                      Редактировать
                    </button>
                    <button
                      v-else
                      @click="saveEdit(product.id)"
                      class="text-green-600 hover:underline"
                    >
                      Сохранить
                    </button>
                    <button
                      v-if="editId === product.id"
                      @click="cancelEdit"
                      class="text-red-600 hover:underline"
                    >
                      Отмена
                    </button>
                  </td>
                </tr>

                <tr v-if="filteredProducts.length === 0">
                  <td colspan="6" class="text-center py-6 text-gray-400">
                    Продукты не найдены
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </main>
      </div>
    </div>
  </NuxtLayout>
</template>


<script setup>
import { ref, computed, onMounted } from 'vue'
import Sidebar from '~/components/Sidebar.vue'
const sidebarOpen = ref(false)

const products = ref([])
const searchTerm = ref('')
const selectedCategory = ref('')
const editId = ref(null) 

const categories = [
  "Кондитерские изделия",
  "Товары для дома",
  "Детские товары",
  "Бакалея",
  "Молочные продукты",
  "Фрукты, овощи",
  "Мясо",
  "Гастрономия",
  "Безалкогольные напитки",
  "Чай, Кофе, Какао",
  "Консервы",
  "Средства гигиены",
  "Бытовая химия",
  "Другие товары"
]

const categoryMap = {
  'Фрукты, овощи': [29, 30, 31, 32].slice(0, 4),
  'Мясо': [27, 28].slice(0, 2),
  'Гастрономия': [34, 58, 59, 81, 92].slice(0, 4),
  'Безалкогольные напитки': [36, 37, 38, 39, 41, 65, 72, 79].slice(0, 4),
  'Кондитерские изделия': [46, 47, 48, 49, 50, 62, 69, 70, 73, 76, 80, 82, 84, 85, 86, 87, 89, 90, 97, 98, 99].slice(0, 5),
  'Бакалея': [45, 61, 64, 67, 91, 94, 95].slice(0, 4),
  'Молочные продукты': [33, 35, 40, 57, 60, 68, 71, 74].slice(0, 4),
  'Собственное производство': [44],
  'Чай, Кофе, Какао': [51, 52, 63, 66].slice(0, 4),
  'Консервы': [64, 77, 92].slice(0, 3),
  'Замороженные продукты': [58],
  'Бытовая химия': [42, 43, 83, 88, 96].slice(0, 4),
  'Средства гигиены': [54, 55, 56, 75, 78, 93, 100].slice(0, 4),
  'Товары для дома': Array.from({ length: 17 }, (_, i) => i + 1).slice(0, 5),
  'Детские товары': Array.from({ length: 9 }, (_, i) => i + 18).slice(0, 4),
}

async function fetchProducts() {
  try {
    const res = await fetch('https://67cbeea23395520e6af6ab52.mockapi.io/categorysale')
    const data = await res.json()
    
   products.value = data.map(item => {
  let category = 'Другие товары'

  for (const [cat, ids] of Object.entries(categoryMap)) {
    if (ids.includes(Number(item.id))) {
      category = cat
      break
    }
  }

  return {
    id: item.id,
    name: item.name,
    price: Number(item.final_price),
    stock: Math.random() > 0.3 ? 'Есть' : 'Нет', 
    category,
    image: item.image || 'https://via.placeholder.com/40',
  }
})
    } catch (error) {
        console.error('Ошибка при загрузке продуктов:', error)
    }
    }

onMounted(fetchProducts)

const filteredProducts = computed(() => {
  return products.value.filter(product => {
    const matchesSearch = product.name.toLowerCase().includes(searchTerm.value.toLowerCase())
    const matchesCategory = selectedCategory.value === '' || product.category === selectedCategory.value
    return matchesSearch && matchesCategory
  })
})


function startEdit(id) {
  editId.value = id
}

function cancelEdit() {
  editId.value = null
}


async function saveEdit(id) {
  const product = products.value.find(p => p.id === id)
  if (!product) return

  try {
    const res = await fetch(`https://67cbeea23395520e6af6ab52.mockapi.io/categorysale/${id}`, {
      method: 'PUT',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({
        name: product.name,
        final_price: product.price,
      })
    })

    if (!res.ok) throw new Error('Не удалось сохранить изменения')

    const updated = await res.json()
    product.name = updated.name
    product.price = Number(updated.final_price)

    editId.value = null
    alert('Изменения успешно сохранены!')

  } catch (error) {
    console.error(error)
    alert('Ошибка при сохранении изменений')
  }
}
</script>
