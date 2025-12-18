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
        
        <svg xmlns="http://www.w3.org/2000/svg" class="w-6 h-6" fill="none"
          viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
          <path stroke-linecap="round" stroke-linejoin="round"
            d="M6 18L18 6M6 6l12 12" />
        </svg>
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
         
          <svg xmlns="http://www.w3.org/2000/svg" class="w-6 h-6" fill="none"
            viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
            <path stroke-linecap="round" stroke-linejoin="round"
              d="M4 6h16M4 12h16M4 18h16" />
          </svg>
        </button>

        <h1 class="text-2xl font-semibold">Управление пользователями</h1>
      </div>

   
      <div class="flex flex-wrap gap-2 mb-6">
        <input
          v-model="search"
          placeholder="Поиск..."
          class="px-3 py-2 border rounded-lg w-full md:w-64"
        />

        <select v-model="filterRole" class="px-3 py-2 border rounded-lg">
          <option value="">Все роли</option>
          <option value="admin">Admin</option>
          <option value="user">User</option>
        </select>

        <select v-model="filterStatus" class="px-3 py-2 border rounded-lg">
          <option value="">Все статусы</option>
          <option value="Активный">Активный</option>
          <option value="В ожидании">В ожидании</option>
          <option value="Заблокирован">Заблокирован</option>
        </select>

       
        <button
          @click="openCreate"
          class="ml-auto flex items-center gap-2 bg-red-600 text-white px-4 py-2 rounded-lg hover:bg-red-700"
        >
        
          <svg xmlns="http://www.w3.org/2000/svg" class="w-5 h-5" fill="none"
            viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
            <path stroke-linecap="round" stroke-linejoin="round"
              d="M12 4v16m8-8H4" />
          </svg>
          Создать
        </button>
      </div>

    
      <div class="bg-white rounded-xl shadow overflow-auto">
        <table class="min-w-full">
          <thead class="bg-gray-50 text-sm text-gray-600">
            <tr>
       
              <th class="p-2 text-left">Имя</th>
              <th class="hidden md:table-cell p-2">Email</th>
              <th class="p-2">Роль</th>
              <th class="p-2">Статус</th>
              <th class="hidden lg:table-cell p-2">Дата</th>
              <th class="p-2">Действия</th>
            </tr>
          </thead>

          <tbody>
            <tr
              v-for="user in filteredUsers"
              :key="user.id"
              class="border-t hover:bg-gray-50"
            >
           

              <td class="p-2 font-medium">{{ user.name }}</td>
              <td class="hidden md:table-cell p-2 text-gray-600">{{ user.email }}</td>
              <td class="p-2">{{ user.role }}</td>

              <td class="p-2">
                <span
                  class="px-2 py-1 rounded-full text-sm"
                  :class="statusClass(user.status)"
                >
                  {{ user.status }}
                </span>
              </td>

              <td class="hidden lg:table-cell p-2 text-gray-500">
                {{ formatDate(user.created_at) }}
              </td>

              <td class="p-2 flex gap-3">
              
                <button @click="openEdit(user)">
                  <svg xmlns="http://www.w3.org/2000/svg" class="w-5 h-5 text-gray-600 hover:text-black"
                    fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                    <path stroke-linecap="round" stroke-linejoin="round"
                      d="M11 4h2m2 2l3 3M5 19l4-1 9-9-3-3-9 9-1 4z" />
                  </svg>
                </button>

             
                <button @click="deleteUser(user.id)">
                  <svg xmlns="http://www.w3.org/2000/svg" class="w-5 h-5 text-red-500 hover:text-red-700"
                    fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                    <path stroke-linecap="round" stroke-linejoin="round"
                      d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862
                      a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6M9 7h6m2 0H7m3-3h4" />
                  </svg>
                </button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </main>

   
    <div
      v-if="showModal"
      class="fixed inset-0 z-50 flex items-center justify-center bg-black/40"
    >
      <div class="bg-white rounded-xl p-6 w-full max-w-md">
        <h2 class="text-xl font-semibold mb-4">
          {{ isEdit ? 'Редактировать пользователя' : 'Создать пользователя' }}
        </h2>

        <div class="space-y-3">
          <input v-model="form.name" placeholder="Имя" class="input" />
          <input v-model="form.email" placeholder="Email" class="input" />
          <select v-model="form.role" class="input">
            <option value="admin">Admin</option>
            <option value="user">User</option>
          </select>
          <select v-model="form.status" class="input">
            <option>Активный</option>
            <option>В ожидании</option>
            <option>Заблокирован</option>
          </select>
        </div>

        <div class="flex justify-end gap-2 mt-6">
          <button @click="showModal=false" class="px-4 py-2 border rounded-lg">
            Отмена
          </button>
          <button
            @click="isEdit ? updateUser() : createUser()"
            class="px-4 py-2 bg-red-600 text-white rounded-lg"
          >
            Сохранить
          </button>
        </div>
      </div>
    </div>

  </div>
</template>
<script setup>
import { ref, computed, onMounted } from 'vue'
import Sidebar from '@/components/Sidebar.vue'


const API_URL = 'https://medical-backend-54hp.onrender.com/api/auth'


const sidebarOpen = ref(false)
const users = ref([])

const search = ref('')
const filterRole = ref('')
const filterStatus = ref('')

const showModal = ref(false)
const isEdit = ref(false)

const form = ref({
  id: null,
  name: '',
  email: '',
  role: 'user',
  status: 'Активный',
})


const fetchUsers = async () => {
  try {
    const token = localStorage.getItem('token')
    if (!token) return

    const res = await fetch(`${API_URL}/users`, {
      headers: {
        Authorization: `Bearer ${token}`,
      },
    })

    const json = await res.json()

    if (json.success) {
      users.value = [
        {
          ...json.data,
          status: 'Активный',
          created_at: json.data.created_at || new Date(),
        },
      ]
    }
  } catch (e) {
    console.error('Fetch users error:', e)
  }
}

onMounted(fetchUsers)


const openCreate = () => {
  isEdit.value = false
  form.value = {
    id: null,
    name: '',
    email: '',
    role: 'user',
    status: 'Активный',
  }
  showModal.value = true
}

const createUser = () => {
  users.value.push({
    ...form.value,
    id: Date.now(),
    created_at: new Date(),
  })
  showModal.value = false
}

const openEdit = (user) => {
  isEdit.value = true
  form.value = { ...user }
  showModal.value = true
}

const updateUser = () => {
  const index = users.value.findIndex(u => u.id === form.value.id)
  if (index !== -1) {
    users.value[index] = { ...form.value }
  }
  showModal.value = false
}

const deleteUser = (id) => {
  if (confirm('Удалить пользователя?')) {
    users.value = users.value.filter(u => u.id !== id)
  }
}


const filteredUsers = computed(() =>
  users.value.filter(u =>
    (!search.value || u.name?.toLowerCase().includes(search.value.toLowerCase())) &&
    (!filterRole.value || u.role === filterRole.value) &&
    (!filterStatus.value || u.status === filterStatus.value)
  )
)

const statusClass = (s) => ({
  'bg-green-100 text-green-700': s === 'Активный',
  'bg-yellow-100 text-yellow-700': s === 'В ожидании',
  'bg-red-100 text-red-700': s === 'Заблокирован',
})

const formatDate = d =>
  d ? new Date(d).toLocaleDateString('ru-RU') : '-'
</script>


<style scoped>
.input {
  @apply w-full px-3 py-2 border rounded-lg;
}
</style>
