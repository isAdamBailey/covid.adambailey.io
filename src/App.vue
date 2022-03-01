<script setup>
import ChooseCounty from './components/ChooseCounty.vue'
</script>

<template>
  <div class="min-h-screen bg-gray-100">
    <div class="container mx-auto px-6 pb-10 space-y-10">
      <img alt="Vue logo" src="./assets/logo.png" />
      <h1
          class="text-3xl text-gray-800 font-extrabold text-center pt-20 md:text-5xl"
      >
        COVID-19 Totals By US County
      </h1>
      <ChooseCounty />
    </div>
  </div>
</template>

