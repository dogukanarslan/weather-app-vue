<template>
  <main class="container text-white">
    <div class="relative mb-8 pt-4">
      <input
        type="text"
        @input="getSearchResults"
        v-model="searchQuery"
        placeholder="Search for a city or state"
        class="w-full border-b bg-transparent px-1 py-2 focus:border-weather-secondary focus:outline-none"
      />
      <p v-if="searchError">{{ searchError }}</p>
      <p v-if="!searchError && searchResults?.length === 0">No results found</p>
      <ul
        v-if="searchResults && searchResults.length !== 0"
        class="absolute w-full bg-weather-secondary px-1 py-2 text-white shadow-md"
      >
        <li
          v-for="result in searchResults"
          @click="previewCity(result)"
          class="cursor-pointer rounded-md bg-weather-secondary p-2"
        >
          {{ result.country }}, {{ result.name }}
        </li>
      </ul>
    </div>
    <div class="flex flex-col gap-4">
      <Suspense>
        <CityList />
        <template #fallback>
          <p>Loading...</p>
        </template>
      </Suspense>
    </div>
  </main>
</template>

<script setup>
import { ref } from "vue"
import axios from "axios"
import { useRouter } from "vue-router"
import CityList from "@/components/CityList.vue"

const router = useRouter()

const searchQuery = ref("")
const queryTimeout = ref(null)
const searchResults = ref(null)
const searchError = ref(null)

const getSearchResults = () => {
  clearTimeout(queryTimeout.value)
  queryTimeout.value = setTimeout(async () => {
    if (searchQuery.value !== "") {
      try {
        const result = await axios.get(
          `http://api.openweathermap.org/geo/1.0/direct?q=${
            searchQuery.value
          }&limit=10&appid=${import.meta.env.VITE_API_KEY}`
        )
        searchResults.value = result.data
      } catch {
        searchError.value = "An error happened"
      }

      return
    }

    searchResults.value = null
  }, 300)
}

const previewCity = (result) => {
  router.push({
    name: "cityView",
    params: { country: result.country, city: result.name },
    query: { lon: result.lon, lat: result.lat, preview: true }
  })
}
</script>
