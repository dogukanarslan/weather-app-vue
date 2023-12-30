<template>
  <div class="flex flex-col flex-1 items-center">
    <div class="text-white p-4 bg-weather-secondary w-full text-center">
      <p>
        You are currently previewing this city, click the "+" icon to start
        tracking this city
      </p>
    </div>
    <div class="text-white text-center">
      <p>{{ route.params.city }}</p>
      <div v-if="weatherData" class="space-y-4">
        <p class="text-8xl">{{ Math.round(weatherData.main.temp) }}&deg</p>
        <div class="text-sm">
          Feels like {{ Math.round(weatherData.main.feels_like) }}&deg
        </div>
        <img
          class="mx-auto"
          :src="`https://openweathermap.org/img/wn/${weatherData.weather[0].icon}@2x.png`"
          alt=""
        />
        <p>
          {{
            weatherData.weather[0].description
              .split(" ")
              .map((item) => item.charAt(0).toUpperCase() + item.slice(1))
              .join(" ")
          }}
        </p>
      </div>
    </div>
  </div>
</template>

<script setup>
import axios from "axios"
import { useRoute } from "vue-router"

const route = useRoute()
const { lat, lon } = route.query

const getWeatherData = async () => {
  try {
    const res = await axios.get(
      `https://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lon}&units=metric&appid=${
        import.meta.env.VITE_API_KEY
      }`
    )
    return res.data
  } catch {}
}

const weatherData = await getWeatherData()
</script>
