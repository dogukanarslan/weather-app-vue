<template>
  <div class="flex flex-1 flex-col items-center">
    <div
      class="w-full bg-weather-secondary p-4 text-center text-white"
      v-if="route.query.preview"
    >
      <p>
        You are currently previewing this city, click the "+" icon to start
        tracking this city
      </p>
    </div>
    <div class="text-center text-white">
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

    <div
      class="flex cursor-pointer items-center gap-2 py-12 text-white duration-150 hover:text-red-500"
      @click="removeCity"
    >
      <i class="fa-solid fa-trash"></i>
      <p>Remove City</p>
    </div>
  </div>
</template>

<script setup>
import axios from "axios"
import { useRoute, useRouter } from "vue-router"

const route = useRoute()
const router = useRouter()
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
const removeCity = () => {
  const cities = JSON.parse(localStorage.getItem("savedCities"))
  const updatedCities = cities.filter((city) => city.id !== route.query.id)
  localStorage.setItem("savedCities", JSON.stringify(updatedCities))
  router.push({ name: "home" })
}
</script>
