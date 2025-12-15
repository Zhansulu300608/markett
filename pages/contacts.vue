<template>
     <header class="w-full bg-[url('/images/newYear.3940986.png')] bg-contain bg-no-repeat
  bg-center h-40 text-white shadow-md relative overflow-hidden">
    <div class="max-w-7xl mx-auto px-4 py-4 flex items-center justify-between">
      <!-- Left Section -->
      <div class="flex items-center space-x-6">
        <!-- Burger Menu -->
        <!-- <button class="text-2xl">
          <span class="material-icons">menu</span>
        </button> -->

        <!-- Logo -->
        <div class="flex items-center space-x-2 text-[#003049]">
          <img src="../public/images/logo.png" alt="Logo" class="h-10" />
         
        </div>
      </div>

      <!-- Search Bar -->
      <div class="flex-1 mx-10 mt-5">
        <div class="flex rounded-full overflow-hidden shadow-sm">
          <input
            type="text"
            placeholder="Поиск"
            class="flex-1 px-4 py-2 text-gray-700 focus:outline-none"
          />
          <button class="px-4 text-white bg-[#003049] border-l">по скидкам</button>
        </div>
      </div>

      <!-- Location / Lang -->
      <div class="flex items-center space-x-4">
        <button class="bg-white text-gray-800 px-4 py-2 rounded-full flex items-center space-x-2 shadow">
        <NuxtLink to="/login" class="login-btn text-[#003049]">Login</NuxtLink>
        </button>
        <button class="bg-white text-gray-800 px-4 py-2 rounded-full flex items-center space-x-2 shadow">
          <span>RU</span>
        </button>
      </div>
    </div>

    <!-- Bottom Nav -->
    <nav class=" text-gray-800 py-4 mt-5 shadow-md">
      <div class="max-w-7xl mx-auto px-4 flex space-x-8 text-sm font-medium">
           <NuxtLink :to="{ name: 'glav' }" class="hover:text-[#C1121F] text-[#003049]">
         Главная
        </NuxtLink>
          <NuxtLink :to="{ name: 'glav' }" class="hover:text-[#C1121F] text-[#003049]">
         Каталог
        </NuxtLink>
        <a href="#" class="hover:text-[#C1121F] text-[#003049]">Доставка</a>
        <a href="#" class="hover:text-[#C1121F] text-[#003049]">Профиль</a>
         <a href="#" class="hover:text-[#C1121F] text-[#003049]">О компании</a>
        <NuxtLink :to="{ name: 'contact' }" class="hover:text-[#C1121F] text-[#003049]">
         Контакты
        </NuxtLink>
      </div>
    </nav>
  </header>
  <div class="min-h-screen bg-white">
    <div class="max-w-7xl mx-auto px-4 py-8">
     <h1 class="text-4xl md:text-4xl  font-bold text-[#003049] text-start">
    Контакты
    </h1>


      <div class="grid grid-cols-1 lg:grid-cols-2 gap-6 mt-4">
        <!-- Left column -->
        <div class="space-y-6">
          <!-- Contact info -->
          <div class="border rounded-lg p-6 bg-[#FDF0D5]">
            <h2 class="text-lg font-semibold mb-4 text-[#003049]">Связаться с нами</h2>
            <ul class="space-y-2 text-sm">
              <li class="flex items-center space-x-2">
                <span class="inline-flex h-8 w-8 items-center justify-center rounded-full bg-green-500 text-white text-xs">
                  ☎
                </span>
                <span class="text-[#003049]">+1 (234) 567-890</span>
              </li>
              <li class="flex items-center space-x-2">
                <span class="inline-flex h-8 w-8 items-center justify-center rounded-full bg-gray-200 text-xs">
                  ✉
                </span>
                <span class="text-[#003049]">support@foodmarket.com</span>
              </li>
              <li class="flex items-center space-x-2">
                <span class="inline-flex h-8 w-8 items-center justify-center rounded-full bg-gray-200 text-xs">
                  📍
                </span>
                <span class="text-[#003049]">г. фуд-Сити, ул. Рыночная, 123</span>
              </li>
            </ul>
          </div>

          <!-- Working hours -->
          <div class="border rounded-lg p-6 bg-[#FDF0D5]">
            <h2 class="text-lg font-semibold mb-4">Часы работы</h2>
            <div class="space-y-2 text-sm">
              <div class="flex justify-between">
                <span>Понедельник - Пятница:</span>
                <span>9:00 - 18:00</span>
              </div>
              <div class="flex justify-between">
                <span>Суббота:</span>
                <span>10:00 - 17:00</span>
              </div>
              <div class="flex justify-between">
                <span>Воскресенье:</span>
                <span class="text-red-500">Выходной</span>
              </div>
            </div>
          </div>

          <!-- Contact form -->
          <div class="border rounded-lg p-6 bg-[#FDF0D5]">
  <h2 class="text-lg font-semibold mb-4">Форма обратной связи</h2>
  <form id="form" class="space-y-4">
    <div>
      <label class="block text-sm mb-1">Ваше имя</label>
      <input
        type="text"
        name="name"
        id="name"
        placeholder="Имя Фамилия"
        class="w-full rounded-md border px-3 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-green-500"
        required
      />
    </div>
    <div>
      <label class="block text-sm mb-1">Ваш Email</label>
      <input
        type="email"
        name="email"
        id="email"
        placeholder="email@example.com"
        class="w-full rounded-md border px-3 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-green-500"
        required
      />
    </div>
    <div>
      <label class="block text-sm mb-1">Телефон</label>
      <input
        type="text"
        name="phone"
        id="phone"
        placeholder="Введите номер телефона"
        class="w-full rounded-md border px-3 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-green-500"
        required
      />
    </div>
    <div>
      <label class="block text-sm mb-1">Сообщение</label>
      <textarea
        name="message"
        id="message"
        rows="4"
        placeholder="Введите ваше сообщение здесь..."
        class="w-full rounded-md border px-3 py-2 text-sm resize-none focus:outline-none focus:ring-2 focus:ring-green-500"
        required
      ></textarea>
    </div>
    <button
      type="submit"
      class="w-full bg-[#003049] hover:bg-green-600 text-white text-sm font-medium py-2.5 rounded-md"
    >
      Отправить сообщение
    </button>
    <p id="success-message" class="text-green-600 mt-2 hidden">Ваше сообщение отправлено!</p>
  </form>
</div>
        </div>

        <!-- Right column: map -->
        <div class="border rounded-lg p-6">
          <h2 class="text-lg font-semibold mb-4">Наши магазины на карте</h2>
          <div class="h-[360px] w-full overflow-hidden rounded-md bg-gray-100">
            <!-- Replace this with your real map component or iframe -->
            <iframe
              class="w-full h-full border-0"
              src="https://www.openstreetmap.org/export/embed.html"
              loading="lazy"
            ></iframe>
          </div>
        </div>
      </div>
    </div>
  </div>

  <footer class="relative bg-[#C1121F] ">

    <div class="absolute inset-0 pointer-events-none opacity-40  [background-size:20px_20px]"></div>

    <div class="relative max-w-7xl mx-auto px-6 py-16">
    
      <div class="mb-12">
       <div class="flex items-center space-x-2 text-[#003049]">
          <img src="../public/images/logo.png" alt="Logo" class="h-10" />
         
        </div>
        <div class="text-white font-bold tracking-wide mt-2">
          НАПОЛНЯЕМ ЖИЗНЬ
        </div>
      </div>

      <!-- Footer grid -->
      <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-12 text-gray-800">
        <!-- Покупателям -->
        <div>
          <h3 class="text-white font-bold mb-4 fs-4">Быстрые ссылки</h3>
          <ul class="space-y-3 text-[#003049]">
            <li class="hover:text-[#003049] text-white">Каталог скидок</li>
            <li class="hover:text-[#003049] text-white">Foodi Chef</li>
            <li class="hover:text-[#003049] text-white">Доставка</li>
            <li class="hover:text-[#003049] text-white">Контакты</li>
          </ul>
        </div>

        <!-- О компании -->
        <div>
          <h3 class="text-white font-semibold mb-4">Быстрые ссылки</h3>
          <ul class="space-y-3 text-[#003049]">
            <li class="hover:text-[#003049] text-white">Магазин Foodi</li>
             <li class="hover:text-[#003049] text-white">Корзина</li>
            <li class="hover:text-[#003049] text-white">Профиль</li>
          </ul>
        </div>

        <!-- Сотрудничество -->
        <div>
          <h3 class="text-white font-semibold mb-4">Свяжитесь с нами</h3>
          <ul class="space-y-3">
            <li class="hover:text-[#003049] text-white"><a href="https://twitter.com" target="_blank" rel="noopener" class="w-6 h-6">+7 7766248255</a></li>
            <li class="hover:text-[#003049] text-white">Рекламодателям</li>
            <li class="hover:text-[#003049] text-white">Арендаторам</li>
          </ul>
        </div>

        <!-- Support & social -->
        <div>
          <h3 class="text-white font-semibold mb-4">Служба поддержки</h3>
          <p class="text-lg mb-6 hover:text-[#003049] text-white">
            <span class="font-semibold hover:text-[#003049] text-white">7766</span> | звонок бесплатный
          </p>

          <h3 class="text-white font-semibold mb-4">
            Мы в социальных сетях
          </h3>

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

  <a href="https://instagram.com" target="_blank" rel="noopener" class="w-6 h-6">
   
  </a>
</div>


          <h3 class="text-white font-semibold mb-2 hover:text-[#003049] text-white">
            Служба доставки 
          </h3>
          <p class="text-lg hover:text-[#003049] text-white">
            <span class="font-semibold">7772</span> | звонок бесплатный
          </p>
        </div>
      </div>
    </div>
  </footer>
  
</template>

<script setup>
import { onMounted } from 'vue'

definePageMeta({ name: "contact" })

const API_URL = "https://script.google.com/macros/s/AKfycbzadttG7NGKw7F-UTrBFxrIQ8pucexJQpjN-g3P1oEmsF3p4PC3NLAvuxla1ZLr6q2e/exec";

onMounted(() => {
  const contactForm = document.getElementById("form");
  const successMessage = document.getElementById("success-message");
  const phoneInput = document.getElementById("phone");
  const submitButton = contactForm.querySelector("button");

  phoneInput.addEventListener("input", function () {
    if (!/^\d*$/.test(phoneInput.value)) {
      phoneInput.setCustomValidity("Phone number should contain only numbers.");
    } else {
      phoneInput.setCustomValidity("");
    }
  });

  contactForm.addEventListener("submit", function (event) {
    event.preventDefault();

    if (!/^\d{10,15}$/.test(phoneInput.value)) {
      alert("Phone number should contain only numbers (10-15 digits).");
      return;
    }

    submitButton.disabled = true;
    const formData = new FormData(contactForm);
    const params = new URLSearchParams();

    for (const pair of formData.entries()) {
      params.append(pair[0], pair[1]);
    }

    fetch(API_URL, {
      method: "POST",
      headers: { "Content-Type": "application/x-www-form-urlencoded" },
      body: params.toString(),
    })
      .then(response => response.json())
      .then(data => {
        if (data.result === "success") {
          successMessage.classList.remove("hidden");
          setTimeout(() => {
            successMessage.classList.add("hidden");
            contactForm.reset();
            submitButton.disabled = false;
          }, 3000);
        } else {
          alert("Error: " + data.error);
          submitButton.disabled = false;
        }
      })
      .catch(error => {
        console.error("Error:", error);
        alert("Something went wrong. Please try again.");
        submitButton.disabled = false;
      });
  });
});
</script>


<style scoped>

</style>