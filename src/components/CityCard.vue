<template>
  <div
    class="flex cursor-pointer rounded-md bg-weather-secondary px-3 py-6 shadow-md"
  >
    <div class="flex flex-1 flex-col">
      <h2 class="text-3xl">{{ city.city }}</h2>
      <h3>{{ city.country }}</h3>
    </div>

    <div class="flex flex-col gap-2">
      <p class="self-end text-3xl">
        {{ Math.round(city.weather.main.temp) }}&deg;
      </p>
      <div>
        <span> H: {{ Math.round(city.weather.main.temp_max) }}&deg;</span>
        <span> L: {{ Math.round(city.weather.main.temp_min) }}&deg;</span>
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps({
  city: {
    type: Object,
    default: () => {}
  }
})
</script>
