<template>
  <AppHeader />
  <SnowEffect />

  
  <Transition name="page">
    <div class="min-h-screen bg-white">
      <div class="max-w-7xl mx-auto px-4 py-8">
        <h1
          class="text-4xl font-bold text-[#003049] text-start animate-on-scroll"
        >
          Контакты
        </h1>

        <div class="grid grid-cols-1 lg:grid-cols-2 gap-6 mt-6">
      
          <div class="space-y-6">
      
            <div class="border rounded-lg p-6 bg-[#FDF0D5] animate-on-scroll">
              <h2 class="text-lg font-semibold mb-4 text-[#003049]">
                Доставка
              </h2>
              <ul class="space-y-2 text-sm text-[#003049]">
                <li>
                  Мы стремимся обеспечить быструю и надежную доставку всех
                  заказов. Обработка с 9:00 до 20:00.
                </li>
                <li>
                  Время доставки зависит от загруженности дорог и объема заказа.
                </li>
                <li>Доставка через Яндекс Еда</li>
              </ul>
            </div>

            
            <div class="border rounded-lg p-6 bg-[#FDF0D5] animate-on-scroll">
              <h2 class="text-lg font-semibold mb-4">Часы работы</h2>
              <div class="space-y-2 text-sm">
                <div class="flex justify-between">
                  <span>Понедельник – Пятница:</span>
                  <span>9:00 – 18:00</span>
                </div>
                <div class="flex justify-between">
                  <span>Суббота:</span>
                  <span>10:00 – 17:00</span>
                </div>
                <div class="flex justify-between">
                  <span>Воскресенье:</span>
                  <span class="text-red-500">Выходной</span>
                </div>
              </div>
            </div>

         
            <div class="border rounded-lg p-6 bg-[#FDF0D5] animate-on-scroll">
              <h2 class="text-lg font-semibold mb-4">
                Форма обратной связи
              </h2>

              <form id="form" class="space-y-4">
                <input
                  name="name"
                  placeholder="Имя Фамилия"
                  class="input"
                  required
                />
                <input
                  type="email"
                  name="email"
                  placeholder="email@example.com"
                  class="input"
                  required
                />
                <input
                  id="phone"
                  name="phone"
                  placeholder="Телефон"
                  class="input"
                  required
                />
                <textarea
                  name="message"
                  rows="4"
                  placeholder="Сообщение..."
                  class="input resize-none"
                  required
                ></textarea>

                <button
                  type="submit"
                  class="w-full bg-[#003049] text-white py-2.5 rounded-md
                         transition-all duration-300
                         hover:bg-green-600 hover:scale-[1.02]
                         active:scale-95"
                >
                  Отправить сообщение
                </button>

                <p
                  id="success-message"
                  class="text-green-600 text-sm hidden"
                >
                  Ваше сообщение отправлено!
                </p>
              </form>
            </div>
          </div>

        
          <div class="border rounded-lg p-6 animate-on-scroll">
            <h2 class="text-lg font-semibold mb-4">
              Наши магазины на карте
            </h2>
            <div class="h-[360px] rounded-md overflow-hidden">
              <iframe
                class="w-full h-full border-0"
                src="https://www.openstreetmap.org/export/embed.html"
                loading="lazy"
              ></iframe>
            </div>
          </div>
        </div>
      </div>

      
      <footer class="bg-[#C1121F] py-16 mt-16">
        <div
          class="max-w-7xl mx-auto px-6 text-white
                 grid grid-cols-1 md:grid-cols-4 gap-12 animate-on-scroll"
        >
          <div>
            <img src="/images/logo.png" class="h-10 mb-4" />
            <p class="font-bold">НАПОЛНЯЕМ ЖИЗНЬ</p>
          </div>

          <div>
            <h3 class="font-bold mb-4">Быстрые ссылки</h3>
            <ul class="space-y-2">
              <NuxtLink to="/">Главная</NuxtLink>
              <NuxtLink to="/catalog">Каталог</NuxtLink>
              <NuxtLink to="/dashbord">Дашборд</NuxtLink>
              <NuxtLink to="/contacts">Контакты</NuxtLink>
            </ul>
          </div>

          <div>
            <h3 class="font-bold mb-4">Контакты</h3>
            <p>+7 776 624 82 55</p>
            <p>+7 776 777 77 09</p>
          </div>

          <div>
            <h3 class="font-bold mb-4">Служба поддержки</h3>
            <p class="text-lg">
              <span class="font-semibold">7766</span> | бесплатно
            </p>
          </div>
        </div>
      </footer>
    </div>
  </Transition>
</template>

<script setup>
import { onMounted } from "vue"
import SnowEffect from "~/components/SnowEffect.vue"

definePageMeta({ name: "contact" })

const API_URL =
  "https://script.google.com/macros/s/AKfycbzadttG7NGKw7F-UTrBFxrIQ8pucexJQpjN-g3P1oEmsF3p4PC3NLAvuxla1ZLr6q2e/exec"

onMounted(() => {
 
  const items = document.querySelectorAll(".animate-on-scroll")
  const observer = new IntersectionObserver(
    entries => {
      entries.forEach(entry => {
        if (entry.isIntersecting) {
          entry.target.classList.add("show")
        }
      })
    },
    { threshold: 0.2 }
  )
  items.forEach(el => observer.observe(el))

  
  const form = document.getElementById("form")
  const phone = document.getElementById("phone")
  const success = document.getElementById("success-message")

  form.addEventListener("submit", e => {
    e.preventDefault()

    if (!/^\d{10,15}$/.test(phone.value)) {
      alert("Введите номер (10–15 цифр)")
      return
    }

    const data = new URLSearchParams(new FormData(form))

    fetch(API_URL, {
      method: "POST",
      headers: { "Content-Type": "application/x-www-form-urlencoded" },
      body: data.toString(),
    })
      .then(r => r.json())
      .then(res => {
        if (res.result === "success") {
          success.classList.remove("hidden")
          form.reset()
          setTimeout(() => success.classList.add("hidden"), 3000)
        }
      })
  })
})
</script>

<style scoped>

.page-enter-active {
  transition: opacity 0.8s ease, transform 0.8s ease;
}
.page-enter-from {
  opacity: 0;
  transform: translateY(20px);
}


.animate-on-scroll {
  opacity: 0;
  transform: translateY(30px);
  transition: all 0.7s ease;
}
.animate-on-scroll.show {
  opacity: 1;
  transform: translateY(0);
}


.input {
  width: 100%;
  border: 1px solid #ddd;
  border-radius: 6px;
  padding: 8px 12px;
  font-size: 14px;
}
.input:focus {
  outline: none;
  border-color: #003049;
}
</style>
