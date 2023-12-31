<template>
  <div class="flex min-h-screen flex-col bg-weather-primary">
    <Navigation />
    <RouterView />
  </div>
</template>

<script setup>
import Navigation from "./components/Navigation.vue"
</script>

<style lang="scss" scoped></style>
