<template>
  <header class="sticky top-0 bg-weather-primary shadow-lg">
    <nav
      class="container flex flex-col items-center justify-between gap-4 py-6 text-white sm:flex-row"
    >
      <RouterLink :to="{ name: 'home' }">
        <div class="flex flex-1 items-center gap-3">
          <i class="fa-solid fa-sun text-2xl"></i>
          <p>Weather App</p>
        </div>
      </RouterLink>

      <div class="flex gap-3">
        <i
          @click="toggleModal"
          class="fa-solid fa-circle-info cursor-pointer text-xl duration-150 hover:text-weather-secondary"
        ></i>
        <i
          class="fa-solid fa-plus cursor-pointer text-xl duration-150 hover:text-weather-secondary"
          @click="addCity"
        ></i>
      </div>
      <BaseModal :modalActive="modalActive" @close-modal="toggleModal">
        <div class="text-black">
          <h1 class="font-bold">About</h1>
          <p>
            Lorem, ipsum dolor sit amet consectetur adipisicing elit. Ratione
            velit eos soluta iste voluptatum quia iure aliquam ullam cum
            suscipit reprehenderit itaque, voluptas tempore sit labore? Sapiente
            debitis fugiat tenetur.
          </p>
          <h1 class="font-bold">How it works?</h1>
          <p>
            Lorem ipsum dolor sit amet consectetur adipisicing elit. Blanditiis
            est atque sed debitis iste fugiat odit repudiandae soluta? Quaerat
            neque illo necessitatibus nemo quibusdam facilis ex natus at a
            ipsam.
          </p>
          <h1 class="font-bold">Removing a city</h1>
          <p>
            Lorem ipsum dolor sit amet consectetur adipisicing elit.
            Perspiciatis ratione itaque dolore ullam aut quod voluptates quo
            laboriosam nobis beatae sit minima, laborum recusandae distinctio
            voluptatum? Similique saepe corporis numquam.
          </p>
        </div>
      </BaseModal>
    </nav>
  </header>
</template>

<script setup>
import { RouterLink, useRoute, useRouter } from "vue-router"
import BaseModal from "./BaseModal.vue"
import { ref } from "vue"
import { uid } from "uid"
const router = useRouter()

const modalActive = ref(false)
const toggleModal = () => {
  modalActive.value = !modalActive.value
}

const savedCities = ref([])
const route = useRoute()

const addCity = () => {
  if (localStorage.getItem("savedCities")) {
    savedCities.value = JSON.parse(localStorage.getItem("savedCities"))
  }

  const locationObj = {
    id: uid(),
    country: route.params.country,
    city: route.params.city,
    coords: {
      lat: route.query.lat,
      lon: route.query.lon
    }
  }

  savedCities.value.push(locationObj)
  localStorage.setItem("savedCities", JSON.stringify(savedCities.value))

  let query = { ...route.query }
  delete query.preview
  query.id = locationObj.id
  router.replace({ query })
}
</script>
