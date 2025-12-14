<template>
  <div class="min-h-screen bg-gray-50 py-10">
    <div class="max-w-7xl mx-auto px-4 flex gap-8">

      <!-- SIDEBAR -->
      <aside class="w-72 bg-white rounded-2xl shadow-sm p-6">
         <!-- Avatar -->
        <div class="w-28 h-28 rounded-full overflow-hidden bg-gray-200 mb-4 ml-16">
          <!-- Replace with <NuxtImg> or <img> -->
          <div class="w-full h-full bg-gray-300" />
        </div>

        <!-- Name & email -->
        <div class="text-center mb-4">
          <p class="font-semibold text-lg">Жансулу Ахметова</p>
          <p class="text-sm text-gray-500">zhansulu0808@gmail.com</p>
        </div>

        <!-- Role badge -->
        <span
          class="inline-flex items-center ml-16 justify-center px-4 py-1 text-xs font-semibold text-orange-600 bg-orange-100 rounded-full mb-6"
        >
          Покупатель
        </span>

        <!-- Stats -->
        <div
          class="w-full mb-6 rounded-xl border bg-gray-50 px-4 py-3 flex justify-between items-center"
        >
          <div class="text-xs text-gray-500">Всего заказов</div>
          <div class="text-2xl font-semibold">7</div>
        </div>

        <!-- Menu -->
   <nav class="mt-6 space-y-2">
          <NuxtLink
            to="/profile"
            class="flex items-center gap-3 px-4 py-3 rounded-xl text-gray-600 hover:bg-gray-100"
          >
            Профиль
          </NuxtLink>

          <NuxtLink
            to="/order"
            class="flex items-center gap-3 px-4 py-3 rounded-xl bg-orange-50 text-orange-600 font-medium"
          >
            Мои заказы
          </NuxtLink>
        </nav>
      </aside>

      <!-- CONTENT -->
      <main class="flex-1 bg-white rounded-2xl shadow-sm p-8">
        <h1 class="text-2xl font-semibold mb-6">
          История заказов
        </h1>

        <div class="space-y-4">
          <OrderCard
            date="15 января 2024 г."
            sum="12500 ₽"
            status="completed"
          />
          <OrderCard
            date="20 февраля 2024 г."
            sum="8000 ₽"
            status="processing"
          />
          <OrderCard
            date="10 марта 2024 г."
            sum="23000 ₽"
            status="completed"
          />
          <OrderCard
            date="01 апреля 2024 г."
            sum="5000 ₽"
            status="cancelled"
          />
          <OrderCard
            date="05 мая 2024 г."
            sum="15000 ₽"
            status="processing"
          />
        </div>
      </main>

    </div>
  </div>
</template>

<script setup>
// import OrderCard from '@/components/OrderCard.vue'
definePageMeta({ name: "profile-orders" })
</script>
