<template>
  <div class="flex min-h-screen bg-gray-50">

    
    <aside
      class="fixed inset-y-0 left-0 z-50 w-64 bg-gray-100 border-r border-gray-300
        transform transition-transform duration-300
        md:static md:translate-x-0"
      :class="sidebarOpen ? 'translate-x-0' : '-translate-x-full'"
    >
      <Sidebar />

      <button
        @click="sidebarOpen = false"
        class="absolute top-4 right-4 md:hidden text-gray-600"
      >
        ✕
      </button>
    </aside>

   
    <div
      v-if="sidebarOpen"
      @click="sidebarOpen = false"
      class="fixed inset-0 bg-black/30 z-40 md:hidden"
    ></div>

  
    <main class="flex-1 p-4 md:p-6 overflow-auto">

     
      <div class="flex items-center gap-3 mb-6">
        <button
          @click="sidebarOpen = true"
          class="md:hidden p-2 rounded hover:bg-gray-200"
        >
          <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
            <path stroke-linecap="round" stroke-linejoin="round" d="M4 6h16M4 12h16M4 18h16" />
          </svg>
        </button>

        <h1 class="text-2xl font-semibold">
          Управление пользователями
        </h1>
      </div>

    
      <div class="bg-white rounded-xl shadow overflow-auto">
        <table class="min-w-full table-auto">
          <thead class="bg-gray-50 text-gray-600 text-sm">
            <tr>
              <th class="p-3 text-left">Аватар</th>
              <th class="p-3 text-left">Имя</th>
              <th class="p-3 text-left">Email</th>
              <th class="p-3 text-left">Роль</th>
              <th class="p-3 text-left">Дата</th>
              <th class="p-3 text-left">Действия</th>
            </tr>
          </thead>

          <tbody>
            <tr
              v-for="user in users"
              :key="user.id"
              class="border-t hover:bg-gray-50"
            >
              <td class="p-3">
                <img
                  :src="user.avatar || defaultAvatar"
                  class="w-10 h-10 rounded-full object-cover"
                />
              </td>

              <td class="p-3 font-medium">
                {{ user.name || 'Без имени' }}
              </td>

              <td class="p-3 text-gray-600">
                {{ user.email }}
              </td>

              <td class="p-3">
                <span class="px-2 py-1 bg-gray-100 rounded text-sm">
                  {{ user.role }}
                </span>
              </td>

              <td class="p-3 text-gray-600">
                {{ formatDate(user.created_at) }}
              </td>

              <td class="p-3 flex gap-2">
            
                <button class="p-2 border rounded hover:bg-gray-100">
                  <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-gray-600" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                    <path stroke-linecap="round" stroke-linejoin="round"
                      d="M11 5H6a2 2 0 00-2 2v11a2 2 0 002 2h11a2 2 0 002-2v-5m-10-4l7-7 4 4-7 7H6v-4z" />
                  </svg>
                </button>

                
                <button class="p-2 bg-red-600 text-white rounded hover:bg-red-700">
                  <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                    <path stroke-linecap="round" stroke-linejoin="round"
                      d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5-4h4" />
                  </svg>
                </button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>

    </main>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import Sidebar from '@/components/Sidebar.vue'

const sidebarOpen = ref(false)
const users = ref([])

const defaultAvatar = 'https://i.pravatar.cc/100'
const API_URL = 'https://medical-backend-54hp.onrender.com/api/auth'

const fetchUsers = async () => {
  try {
    const res = await fetch(API_URL)
    const result = await res.json()

    if (result.success && result.data) {
      users.value = [result.data]
    }
  } catch (err) {
    console.error('API error:', err)
  }
}

const formatDate = (date) => {
  if (!date) return '-'
  return new Date(date).toLocaleDateString()
}

onMounted(fetchUsers)
</script>
