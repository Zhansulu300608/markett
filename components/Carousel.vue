<template>
  <div class="carousel-container max-w-7xl mx-auto px-4 py-4">

    <div class="relative overflow-hidden rounded-xl shadow-lg mb-4">
      <div class="w-full h-[100] bg-gray-200 flex items-center justify-center">
      <img 
  :key="currentSlideIndex"
  :src="currentSlide.image" 
  class="w-full h-full object-cover slide-animate"
/>

        
        <div class="absolute inset-0 bg-black bg-opacity-0 flex items-center justify-center">
          <div class="text-white text-center p-6">
            <h2 class="text-4xl md:text-5xl font-bold mb-4">{{ currentSlide.title }}</h2>
            <p class="text-xl md:text-2xl font-medium">{{ currentSlide.subtitle }}</p>
          </div>
        </div>
      </div>
      
    
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
    
<div class="relative overflow-hidden w-full mt-4">
  <div
    class="flex gap-3 whitespace-nowrap animate-marquee hover:[animation-play-state:paused]"
  >
    <button
      v-for="(tag, index) in animatedHashtags"
      :key="index"
      class="bg-white border border-gray-300 hover:bg-gray-50 text-gray-800 font-medium py-2 px-4 rounded-full transition"
    >
      {{ tag }}
    </button>
  </div>
</div>


  </div>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue'


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


const hashtags = ref([
  '#Выиграй автомобиль с Milka',
  '#Подарок от Barni',
  '#+21 Бонусы от Reyka',
  '#Скидки каждый день',
  '#Жуй в моменте',
  '#Покупай сникерс и собирай звезд',
  '#Подарки для всей семьи',
])


const animatedHashtags = computed(() => {
  return [...hashtags.value, ...hashtags.value]
})


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


const startAutoplay = () => {
  autoplayInterval = setInterval(() => {
    nextSlide()
  }, 4000) 
}

const stopAutoplay = () => {
  if (autoplayInterval) {
    clearInterval(autoplayInterval)
    autoplayInterval = null
  }
}


onMounted(() => {
  startAutoplay()
})

onUnmounted(() => {
  stopAutoplay()
})


const handleUserInteraction = () => {
  stopAutoplay()

}


</script>

<style scoped>
.carousel-container {
  position: relative;
}
@keyframes marquee {
  0% {
    transform: translateX(0);
  }
  100% {
    transform: translateX(-50%);
  }
}

.animate-marquee {
  animation: marquee 18s linear infinite;
}

@keyframes slideFadeZoom {
  0% {
    opacity: 0;
    transform: scale(1.05);
  }
  100% {
    opacity: 1;
    transform: scale(1);
  }
}

.slide-animate {
  animation: slideFadeZoom 0.8s ease-in-out;
}


</style>