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
        class="absolute top-4 right-4 md:hidden text-gray-600 hover:text-gray-900"
        aria-label="Close sidebar"
      >
        ✕
      </button>
    </aside>

   
    <div
      v-if="sidebarOpen"
      @click="sidebarOpen = false"
      class="fixed inset-0 bg-black bg-opacity-30 z-40 md:hidden"
    ></div>

   
    <main class="flex-1 p-4 md:p-6 overflow-auto">
      <div class="flex items-center gap-3 mb-4 md:mb-6">
        <button
          @click="sidebarOpen = true"
          aria-label="Open sidebar"
          class="md:hidden bg-transparent hover:bg-gray-200 p-2 rounded"
        >
          <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-gray-900" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
            <path stroke-linecap="round" stroke-linejoin="round" d="M4 6h16M4 12h16M4 18h16" />
          </svg>
        </button>
        <h1 class="text-2xl font-semibold text-gray-900">
          Управление Пользователями
        </h1>
      </div>

      
      <div class="flex flex-wrap items-center gap-2 mb-4 md:mb-6">
        <input
          v-model="search"
          type="text"
          placeholder="Поиск пользователей..."
          class="w-full md:w-64 px-3 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-red-500"
        />

    <select v-model="filterRole">
  <option value="">Все</option>
  <option value="admin">Admin</option>
  <option value="user">User</option>
</select>


        <select v-model="filterStatus" class="px-3 py-2 border rounded-lg bg-white w-full md:w-auto">
          <option value="">Все статусы</option>
          <option value="Активный">Активный</option>
          <option value="В ожидании">В ожидании</option>
          <option value="Заблокирован">Заблокирован</option>
        </select>

        <button
          class="ml-auto flex items-center gap-2 bg-red-600 text-white px-4 py-2 rounded-lg hover:bg-red-700 transition w-full md:w-auto justify-center"
        >
          <span class="text-lg">+</span>
          Создать Пользователя
        </button>
      </div>

    
      <div class="bg-white rounded-xl shadow overflow-auto">
        <table class="min-w-full border-collapse table-auto">
          <thead class="bg-gray-50 text-gray-600 text-sm">
            <tr>
              <th class="p-2 text-left"><input type="checkbox" /></th>
              <th class="p-2 text-left">Аватар</th>
              <th class="p-2 text-left">Имя</th>
              <th class="hidden sm:table-cell p-2 text-left">Email</th>
              <th class="hidden md:table-cell p-2 text-left">Роль</th>
              <th class="hidden md:table-cell p-2 text-left">Статус</th>
              <th class="hidden lg:table-cell p-2 text-left">Дата Регистрации</th>
              <th class="p-2 text-left">Действия</th>
            </tr>
          </thead>

          <tbody>
            <tr
              v-for="user in filteredUsers"
              :key="user.email"
              class="border-t hover:bg-gray-50"
            >
              <td class="p-2"><input type="checkbox" /></td>
              <td class="p-2">
                <img :src="user.avatar || `https://i.pravatar.cc/100?u=${user.email}`" class="w-10 h-10 rounded-full object-cover" />
              </td>
              <td class="p-2 font-medium text-gray-900">{{ user.name }}</td>
              <td class="hidden sm:table-cell p-2 text-gray-600">{{ user.email }}</td>
              <td class="hidden md:table-cell p-2">
                <span class="px-2 py-1 rounded-full text-sm bg-gray-100">{{ user.role }}</span>
              </td>
              <td class="hidden md:table-cell p-2">
                <span
                  :class="[
                    'px-2 py-1 rounded-full text-sm',
                    user.status === 'Активный' && 'bg-green-100 text-green-700',
                    user.status === 'В ожидании' && 'bg-yellow-100 text-yellow-700',
                    user.status === 'Заблокирован' && 'bg-red-100 text-red-700',
                  ]"
                >
                  {{ user.status || 'Активный' }}
                </span>
              </td>
              <td class="hidden lg:table-cell p-2 text-gray-600">{{ formatDate(user.created_at) }}</td>
              <td class="p-2 flex items-center gap-1 text-sm">
                <button class="flex items-center gap-1 px-2 py-1 border rounded-lg hover:bg-gray-100" title="Редактировать">✎</button>
                <button class="text-red-500 hover:text-red-700" title="Удалить">🗑</button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </main>
  </div>
</template><script setup>
import { ref, onMounted, computed } from 'vue'
import Sidebar from '@/components/Sidebar.vue'

const API_URL = 'https://medical-backend-54hp.onrender.com/api/auth'

const sidebarOpen = ref(false)
const users = ref([])

const search = ref('')
const filterRole = ref('')
const filterStatus = ref('')

const fetchUsers = async () => {
  try {
    const token = localStorage.getItem('token')

    if (!token) {
      console.error('Token табылмады')
      return
    }

    const res = await fetch(`${API_URL}/user`, {
      method: 'GET',
      headers: {
        'Content-Type': 'application/json',
        Authorization: `Bearer ${token}`,
      },
    })

    const json = await res.json()

    if (json.success && json.data) {
      users.value = [
        {
          id: json.data.id,
          name: json.data.name,
          email: json.data.email,
          role: json.data.role,
          avatar: json.data.avatar,
          created_at: json.data.created_at,
          status: 'Активный',
        },
      ]
    } else {
      console.error('API error:', json)
    }
  } catch (err) {
    console.error('Ошибка при получении пользователя:', err)
  }
}

onMounted(fetchUsers)

const filteredUsers = computed(() => {
  return users.value.filter(u => {
    return (
      (!search.value ||
        u.name?.toLowerCase().includes(search.value.toLowerCase()) ||
        u.email?.toLowerCase().includes(search.value.toLowerCase())) &&
      (!filterRole.value || u.role === filterRole.value) &&
      (!filterStatus.value || u.status === filterStatus.value)
    )
  })
})

const formatDate = (dateStr) => {
  if (!dateStr) return '-'
  return new Date(dateStr).toLocaleDateString('ru-RU')
}
</script>
