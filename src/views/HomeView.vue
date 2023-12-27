<template>
  <main class="container text-white">
    <div class="pt-4 mb-8 relative">
      <input
        type="text"
        @input="getSearchResults"
        v-model="searchQuery"
        placeholder="Search for a city or state"
        class="py-2 px-1 w-full bg-transparent border-b focus:border-weather-secondary focus:outline-none"
      />
      <p v-if="searchError">{{ searchError }}</p>
      <p v-if="!searchError && searchResults?.length === 0">No results found</p>
      <ul
        v-if="searchResults && searchResults.length !== 0"
        class="absolute bg-weather-secondary text-white shadow-md py-2 px-1 w-full"
      >
        <li
          v-for="result in searchResults"
          class="bg-weather-secondary p-2 rounded-md"
        >
          {{ result.country }}, {{ result.state }}, {{ result.name }}
        </li>
      </ul>
    </div>
  </main>
</template>

<script setup>
import { ref } from "vue"
import axios from "axios"

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
</script>
