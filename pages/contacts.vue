<template>
     <AppHeader />
  <slot />
 <SnowEffect />

  <slot />
  <div class="min-h-screen bg-white">
    <div class="max-w-7xl mx-auto px-4 py-8">
     <h1 class="text-4xl md:text-4xl  font-bold text-[#003049] text-start">
    Контакты
    </h1>


      <div class="grid grid-cols-1 lg:grid-cols-2 gap-6 mt-4">
       
        <div class="space-y-6">
        
          <div class="border rounded-lg p-6 bg-[#FDF0D5]">
            <h2 class="text-lg font-semibold mb-4 text-[#003049]">Доставка</h2>
            <ul class="space-y-2 text-sm">
              <li class="flex items-center space-x-2">
                <span class="inline-flex h-8 w-8 items-center justify-center rounded-full bg-[#003049]  text-white text-xs">
                 
                </span>
                <span class="text-[#003049]">Мы стремимся обеспечить быструю и надежную доставку всех ваших заказов. Обработка заказов производится ежедневно с 9:00 до 20:00.</span>
              </li>
              <li class="flex items-center space-x-2">
                
                <span class="text-[#003049]">Время доставки может варьироваться в зависимости от загруженности дорог и объема заказа.</span>
              </li>
              <li class="flex items-center space-x-2">
              
                <span class="text-[#003049]">Доставка через Яндекс еда</span>
              </li>
            </ul>
          </div>

         
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

        
        <div class="border rounded-lg p-6">
          <h2 class="text-lg font-semibold mb-4">Наши магазины на карте</h2>
          <div class="h-[360px] w-full overflow-hidden rounded-md bg-gray-100">
          
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

 

  <footer class="bg-[#C1121F] py-16">
    <div class="max-w-7xl mx-auto px-6 text-white grid grid-cols-1 md:grid-cols-4 gap-12">
      <div>
        <img src="/images/logo.png" class="h-10 mb-4" />
        <p class="font-bold">НАПОЛНЯЕМ ЖИЗНЬ</p>
      </div>
     <div>
  <h3 class="font-bold mb-4">Быстрые ссылки</h3>
  <ul class="space-y-2">
    <li>
      <NuxtLink to="/" class="hover:text-[#003049]">Главная</NuxtLink>
    </li>
    <li>
      <NuxtLink to="/catalog" class="hover:text-[#003049]">Каталог</NuxtLink>
    </li>
    <li>
      <NuxtLink to="/dashbord" class="hover:text-[#003049]">Дашборд</NuxtLink>
    </li>
    <li>
      <NuxtLink to="/contacts" class="hover:text-[#003049]">Контакты</NuxtLink>
    </li>
  </ul>
</div>

      <div>
        <h3 class="font-bold mb-4">Контакты</h3>
        <p>+7 776 624 82 55</p>
        <p>+7 776 777 77 09</p>
      </div>
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
  </footer>
</template>

<script setup>
import { onMounted } from 'vue'
import SnowEffect from '~/components/SnowEffect.vue'
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