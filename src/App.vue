<template>
  <div class="flex flex-col min-h-screen bg-weather-primary">
    <Navigation />
    <RouterView />
  </div>
</template>

<script setup>
import Navigation from "./components/Navigation.vue"
</script>

<style lang="scss" scoped></style>
