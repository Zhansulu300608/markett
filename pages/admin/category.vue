<template>
  <div class="flex min-h-screen bg-gray-50 overflow-hidden">
    <div
      v-if="sidebarOpen"
      @click="sidebarOpen = false"
      class="fixed inset-0 bg-black/50 z-40 md:hidden"
    />

    <aside
      class="fixed md:static inset-y-0 left-0 z-50 md:z-auto w-64 bg-gray-100 border-r border-gray-300
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
        <h1 class="font-semibold text-lg">Категории</h1>
      </header>

      <main class="flex-1 p-4 md:p-8 overflow-x-hidden">
        <div class="mb-6 flex flex-col md:flex-row md:items-center md:justify-between gap-4">
          <h1 class="text-2xl md:text-3xl font-bold text-gray-900">
            Управление категориями
          </h1>

          <button
            @click="addCategory"
            class="inline-flex items-center justify-center gap-2 rounded-md bg-[#003049] px-4 py-2 text-sm font-medium text-white hover:bg-white hover:text-[#003049] border transition"
          >
            <span class="text-xl leading-none">+</span>
            <span>Добавить категорию</span>
          </button>
        </div>

        <div class="rounded-xl bg-white p-4 md:p-6 shadow">
          <div class="mb-4">
            <h2 class="text-lg font-semibold text-gray-900">
              Список категорий
            </h2>
            <p class="mt-1 text-sm text-gray-500">
              Создание, редактирование и удаление категорий
            </p>
          </div>

          <div class="mb-4">
            <div class="relative w-full md:w-80">
              <input
                v-model="searchQuery"
                placeholder="Поиск категорий..."
                class="w-full rounded-md border border-gray-200 px-3 py-2 pl-9 text-sm focus:ring-1 focus:ring-emerald-500"
              />
              <svg class="absolute left-3 top-1/2 -translate-y-1/2 h-5 w-5 text-gray-400" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                  d="M21 21l-4.35-4.35m0 0A7.5 7.5 0 1110.5 3a7.5 7.5 0 016.15 13.65z" />
              </svg>
            </div>
          </div>

          <div class="overflow-x-auto border rounded-lg">
            <table class="min-w-[900px] w-full divide-y divide-gray-200 text-sm">
              <thead class="bg-gray-50">
                <tr>
                  <th class="px-4 py-3 text-left text-xs font-semibold text-gray-500">Изображение</th>
                  <th class="px-4 py-3 text-left text-xs font-semibold text-gray-500">ID</th>
                  <th class="px-4 py-3 text-left text-xs font-semibold text-gray-500">Название</th>
                  <th class="px-4 py-3 text-left text-xs font-semibold text-gray-500">Описание</th>
                  <th class="px-4 py-3 text-center text-xs font-semibold text-gray-500">Товары</th>
                  <th class="px-4 py-3 text-center text-xs font-semibold text-gray-500">Активна</th>
                  <th class="px-4 py-3 text-center text-xs font-semibold text-gray-500">Действия</th>
                </tr>
              </thead>

              <tbody class="divide-y">
                <tr v-for="row in filteredRows" :key="row.id" class="hover:bg-gray-50">
                  <td class="px-4 py-3">
                    <img :src="row.image" class="h-10 w-10 object-contain mx-auto" />
                  </td>
                  <td class="px-4 py-3">{{ row.id }}</td>
                  <td class="px-4 py-3 font-medium">{{ row.name }}</td>
                  <td class="px-4 py-3">{{ row.description }}</td>
                  <td class="px-4 py-3 text-center">
                    <span class="px-2 py-0.5 rounded-full bg-emerald-50 text-emerald-700 text-xs">
                      {{ row.products }}
                    </span>
                  </td>
                  <td class="px-4 py-3 text-center">
                    <span :class="row.active ? 'text-emerald-600' : 'text-red-600'">
                      {{ row.active ? '✔' : '✖' }}
                    </span>
                  </td>
                
<td class="px-4 py-3 text-center">
  <div class="flex justify-center gap-2">

    <button @click="openEditModal(row)" class="h-8 w-8 border rounded-md flex items-center justify-center hover:bg-gray-100">
      <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-gray-600" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
        <path stroke-linecap="round" stroke-linejoin="round"
          d="M11 5H6a2 2 0 00-2 2v11a2 2 0 002 2h11a2 2 0 002-2v-5m-10-4l7-7 4 4-7 7H6v-4z" />
      </svg>
    </button>

    
    <button @click="deleteCategory(row.id)" class="h-8 w-8  text-red-500 rounded-md flex items-center justify-center ">
      <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
        <path stroke-linecap="round" stroke-linejoin="round"
          d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5-4h4m-4 0a1 1 0 00-1 1v1h6V4a1 1 0 00-1-1m-4 0h4" />
      </svg>
    </button>
  </div>
</td>

                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </main>
    </div>

    
    <div
      v-if="editModalOpen"
      class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50"
    >
      <div class="bg-white p-6 rounded shadow-lg w-96 max-w-full">
        <h2 class="text-lg font-semibold mb-4">Редактировать категорию</h2>

        <label class="block mb-3">
          Название:
          <input
            v-model="editCategory.name"
            class="w-full border rounded px-3 py-2 mt-1"
            type="text"
          />
        </label>

        <label class="block mb-3">
          Описание:
          <textarea
            v-model="editCategory.description"
            class="w-full border rounded px-3 py-2 mt-1"
            rows="3"
          ></textarea>
        </label>

        <label class="block mb-3 flex items-center gap-2">
          <input type="checkbox" v-model="editCategory.active" />
          Активна
        </label>

        <div class="flex justify-end gap-3 mt-4">
          <button
            @click="closeEditModal"
            class="px-4 py-2 border rounded hover:bg-gray-100"
          >
            Отмена
          </button>
          <button
            @click="saveEdit"
            class="px-4 py-2 bg-blue-600 text-white rounded hover:bg-blue-700"
          >
            Сохранить
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'
import Sidebar from '@/components/Sidebar.vue'

const sidebarOpen = ref(false)

interface Category {
  id: string
  name: string
  description: string
  image: string
  products: number
  active: boolean
}

const categories = [
  { name: "Кондитерские изделия", image: "https://magnum.kz:1337/uploads/konditerskie_izdeliya_590a0ea83b.png" },
  { name: "Товары для дома", image: "https://magnum.kz:1337/uploads/Tovary_dlya_doma_c55163f947.png" },
  { name: "Детские товары", image: "https://magnum.kz:1337/uploads/Detskie_tovary_5ada265732.png" },
  { name: "Бакалея", image: "https://magnum.kz:1337/uploads/bakaleya_3110e86aae.png" },
  { name: "Молочные продукты", image: "https://magnum.kz:1337/uploads/Molochnye_produkty_0fc46bfbd0.png" },
  { name: "Фрукты, овощи", image: "https://magnum.kz:1337/uploads/Frutky_i_ovoshhi_35bebc3cdc.png" },
  { name: "Мясо", image: "https://magnum.kz:1337/uploads/myaso_24f87a2b63.png" },
  { name: "Гастрономия", image: "https://magnum.kz:1337/uploads/Gastronomiya_785ab8a5c8.png" },
  { name: "Безалкогольные напитки", image: "https://magnum.kz:1337/uploads/Bezalkogolnye_napitki_804131c63d.png" },
  { name: "Чай, Кофе, Какао", image: "https://magnum.kz:1337/uploads/Chaj_kofe_kakao_6013945351.png" },
  { name: "Консервы", image: "https://magnum.kz:1337/uploads/konservy_fe0bd212c3.png" },
  { name: "Средства гигиены", image: "https://magnum.kz:1337/uploads/Sredstva_gigieny_75f280ceb3.png" },
  { name: "Бытовая химия", image: "https://magnum.kz:1337/uploads/Bytovaya_himiya_4e6942dd39.png" },
  { name: "Другие товары", image: "https://magnum.kz:1337/uploads/Drugie_tovary_d07556ae4d.png" }
]

const productsByCategory: Record<string, number[]> = {
  'Кондитерские изделия': [306, 307, 416],
  'Товары для дома': [109, 602, 604],
  'Детские товары': [416],
  'Бакалея': [411, 412, 413],
  'Молочные продукты': [204, 503, 400],
  'Фрукты, овощи': [114, 115, 116],
  'Мясо': [112, 119],
  'Гастрономия': [117, 118, 113],
  'Безалкогольные напитки': [300, 600, 601],
  'Чай, Кофе, Какао': [201, 207, 508],
  'Консервы': [304, 305, 209],
  'Бытовая химия': [203, 308, 603],
  'Средства гигиены': [202, 415, 605],
  'Другие товары': [101,102,103]
}

const rows = ref<Category[]>(categories.map((c, index) => ({
  id: `cat-${index+1}`,
  name: c.name,
  description: `Описание категории ${c.name}`,
  image: c.image,
  products: productsByCategory[c.name]?.length || 0,
  active: true
})))

const searchQuery = ref('')
const filteredRows = computed(() => {
  if (!searchQuery.value) return rows.value
  return rows.value.filter(r =>
    r.name.toLowerCase().includes(searchQuery.value.toLowerCase())
  )
})

let counter = rows.value.length + 1

const addCategory = () => {
  const newCategory: Category = {
    id: `cat-${counter}`,
    name: `Новая категория ${counter}`,
    description: 'Описание новой категории',
    image: 'https://via.placeholder.com/50',
    products: 0,
    active: true
  }
  rows.value.push(newCategory)
  counter++
}

const deleteCategory = (id: string) => {
  rows.value = rows.value.filter(r => r.id !== id)
}

const editModalOpen = ref(false)
const editCategory = ref<Category>({
  id: '',
  name: '',
  description: '',
  image: '',
  products: 0,
  active: true
})

const openEditModal = (category: Category) => {
  editCategory.value = { ...category }
  editModalOpen.value = true
}

const closeEditModal = () => {
  editModalOpen.value = false
}

const saveEdit = () => {
  const index = rows.value.findIndex(r => r.id === editCategory.value.id)
  if (index !== -1) {
    rows.value[index] = { ...editCategory.value }
  }
  closeEditModal()
}
</script>
