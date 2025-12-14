<script setup>
const props = defineProps({
  product: Object
})

const discount = computed(() => {
  return Math.round(
    ((props.product.start_price - props.product.final_price) /
      props.product.start_price) * 100
  )
})
</script>

<template>
  <div class="relative bg-white rounded-xl border hover:shadow-md transition">

    <!-- DISCOUNT -->
    <div
      v-if="discount > 0"
      class="absolute top-2 left-2 bg-[#C1121F] text-white text-sm font-bold px-2 py-1 rounded-lg"
    >
      -{{ discount }}%
    </div>

    <!-- DATE -->
    <div class="absolute top-2 right-2 text-xs text-gray-400">
      до {{ product.action_end }}
    </div>

    <!-- IMAGE -->
    <img
      :src="product.image"
      class="w-full h-40 object-contain p-4"
      alt=""
    />

    <!-- CONTENT -->
    <div class="p-3 space-y-2">
      <h3 class="text-sm font-semibold line-clamp-2">
        {{ product.name }}
      </h3>

      <!-- PRICE -->
      <div class="bg-yellow-400 rounded-xl p-2 text-center">
        <div class="text-xs text-gray-700 line-through">
          {{ product.start_price }} ₸
        </div>
        <div class="text-lg font-bold">
          {{ product.final_price }} ₸
        </div>
      </div>
    </div>
  </div>
</template>
