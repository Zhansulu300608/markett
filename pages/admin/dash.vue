
<template>
  <div class="flex h-screen bg-gray-50">
    
    <aside class="w-64 bg-white border-r border-gray-200 flex flex-col">
      <div class="p-6 flex items-center space-x-3">
        <div class="w-8 h-8 bg-blue-600 rounded-lg flex items-center justify-center">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-white" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M20 7l-8-4-8 4m16 6l-8 4m8-4v10l-8 4m0-10L4 13m8-4v10l8 4" />
          </svg>
        </div>
        <span class="font-bold text-lg text-gray-800">Admin</span>
      </div>

      <nav class="mt-6 flex-1 px-4">
        <ul class="space-y-2">
          <li>
            <NuxtLink
              to="/admin/dashboard"
              class="flex items-center px-4 py-3 text-gray-700 hover:bg-blue-50 hover:text-blue-700 rounded-lg transition-colors"
            >
              <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-3" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 7v10a2 2 0 002 2h14a2 2 0 002-2V9a2 2 0 00-2-2h-6l-2-2H5a2 2 0 00-2 2z" />
              </svg>
              Dashboard
            </NuxtLink>
          </li>
          <li>
            <NuxtLink
              to="/admin/products"
              class="flex items-center px-4 py-3 text-gray-700 hover:bg-blue-50 hover:text-blue-700 rounded-lg transition-colors"
              :class="{ 'bg-blue-600 text-white': $route.path === '/products' }"
            >
              <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-3" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M20 7l-8-4-8 4m16 6l-8 4m8-4v10l-8 4m0-10L4 13m8-4v10l8 4" />
              </svg>
        Управление продуктами
            </NuxtLink>
          </li>
          <li>
            <NuxtLink
              to="/admin/category"
              class="flex items-center px-4 py-3 text-gray-700 hover:bg-blue-50 hover:text-blue-700 rounded-lg transition-colors"
            >
              <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-3" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10.325 4.317c.426-.426 1.038-.426 1.464 0l1.515 1.515m-7.07 7.07a7.071 7.071 0 0114.142 0 7.071 7.071 0 01-14.142 0z" />
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10.325 4.317c.426-.426 1.038-.426 1.464 0l1.515 1.515m-7.07 7.07a7.071 7.071 0 0114.142 0 7.071 7.071 0 01-14.142 0z" />
              </svg>
        Управление категорий
            </NuxtLink>
          </li>
        </ul>
      </nav>

      <div class="p-6 border-t border-gray-200">
        <NuxtLink
          to="/"
          class="flex items-center text-gray-500 hover:text-gray-700 transition-colors"
        >
          <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-2" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 19l-7-7m0 0l7-7m-7 7h18" />
          </svg>
          Назад на сайт
        </NuxtLink>
      </div>
    </aside>

  
  </div>
</template>