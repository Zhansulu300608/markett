<template>
  <div class="carousel-container max-w-7xl mx-auto px-4 py-4">
    <!-- Main Carousel Image Area -->
    <div class="relative overflow-hidden rounded-xl shadow-lg mb-4">
      <div class="w-full h-[100] bg-gray-200 flex items-center justify-center">
        <!-- Placeholder for the main image -->
        <img 
          :src="currentSlide.image" 
          :alt="currentSlide.title" 
          class="w-full h-full object-cover"
        />
        
        <!-- Overlay text content -->
        <div class="absolute inset-0 bg-black bg-opacity-0 flex items-center justify-center">
          <div class="text-white text-center p-6">
            <h2 class="text-4xl md:text-5xl font-bold mb-4">{{ currentSlide.title }}</h2>
            <p class="text-xl md:text-2xl font-medium">{{ currentSlide.subtitle }}</p>
          </div>
        </div>
      </div>
      
      <!-- Navigation arrows -->
      <button 
        @click="prevSlide" 
        class="absolute left-4 top-1/2 transform -translate-y-1/2 bg-white bg-opacity-70 hover:bg-opacity-100 rounded-full p-3 transition-all duration-300 shadow-md"
      >
        <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-gray-800" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 19l-7-7 7-7" />
        </svg>
      </button>
      
      <button 
        @click="nextSlide" 
        class="absolute right-4 top-1/2 transform -translate-y-1/2 bg-white bg-opacity-70 hover:bg-opacity-100 rounded-full p-3 transition-all duration-300 shadow-md"
      >
        <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-gray-800" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7" />
        </svg>
      </button>
    </div>
    
    <!-- Bottom controls -->
    <div class="flex flex-wrap items-center justify-between gap-4">
      <!-- More details button -->
      <button class="bg-gray-200 hover:bg-gray-300 text-gray-800 font-medium py-2 px-4 rounded-lg transition-colors duration-300">
        Подробнее
      </button>
      
      <!-- Slide counter -->
      <div class="text-gray-600 font-medium">
        {{ currentSlideIndex + 1 }}/{{ slides.length }}
      </div>
      
      <!-- Hashtag buttons -->
      <div class="flex flex-wrap gap-2">
        <button 
          v-for="(tag, index) in hashtags" 
          :key="index"
          class="bg-white border border-gray-300 hover:bg-gray-50 text-gray-800 font-medium py-2 px-4 rounded-full transition-colors duration-300"
        >
          {{ tag }}
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue'

// Define your slides data
const slides = ref([
  {
    image: 'https://magnum.kz:1337/uploads/SNK_web_2360x1016_rus_e19f9cb3c7.png',
  },
   {
    image: 'https://magnum.kz:1337/uploads/akciyi_desk_ru_2360x1016_b8dcc0e078.jpg',
  },
   {
    image: 'https://magnum.kz:1337/uploads/2360x1016r_17bfdb60a1.jpg',
  },
  {
    image: 'https://magnum.kz:1337/uploads/2360h1016_713a0bbaa7.jpg',
  },
  {
    image: 'https://magnum.kz:1337/uploads/vk_2360h1016_kopiya_2_fcb745e516.jpg',
  },
   {
    image: 'https://magnum.kz:1337/uploads/Desktop_2360x1016_8defe50d24.png',
  },
  {
    image: 'https://magnum.kz:1337/uploads/Bannery2360h1016_ru_d80ebfe19b.jpg',
  },
   {
    image: 'https://magnum.kz:1337/uploads/2360h1016_kopiya_972e8a69ec.jpg',
  },
  {
    image: 'https://magnum.kz:1337/uploads/SNK_Kairat_Magnum_Web_desktop_banner_2360x1016_rus_17fa51c4e8.jpg',
  }
])

// Define hashtags
const hashtags = ref([
  '#Выиграй автомобиль с Milka',
  '#Подарок от Barni',
  '#+21 Бонусы от Reyka'
])

const currentSlideIndex = ref(0)
let autoplayInterval = null

const currentSlide = computed(() => {
  return slides.value[currentSlideIndex.value]
})

const nextSlide = () => {
  currentSlideIndex.value = (currentSlideIndex.value + 1) % slides.value.length
}

const prevSlide = () => {
  currentSlideIndex.value = (currentSlideIndex.value - 1 + slides.value.length) % slides.value.length
}

// Автоплей функциясы
const startAutoplay = () => {
  autoplayInterval = setInterval(() => {
    nextSlide()
  }, 4000) // 4 секунд сайын
}

const stopAutoplay = () => {
  if (autoplayInterval) {
    clearInterval(autoplayInterval)
    autoplayInterval = null
  }
}

// Lifecycle hooks
onMounted(() => {
  startAutoplay()
})

onUnmounted(() => {
  stopAutoplay()
})

// Опционал: тышқан немесе түрткен кезде автоплей тоқтатылып, қайта іске қосылуы мүмкін
// Пайдаланушы өзі басқарғанда автоплей тоқтату үшін:
const handleUserInteraction = () => {
  stopAutoplay()
  // Қажет болса, белгілі бір уақыттан кейін қайта іске қосуға болады:
  // setTimeout(startAutoplay, 5000)
}

// Егер қалаған болсаңыз, prev/next батырмаларына handleUserInteraction қосыңыз:
// @click="() => { prevSlide(); handleUserInteraction() }"
</script>

<style scoped>
.carousel-container {
  position: relative;
}
</style>