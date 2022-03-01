<script setup>
import {ref} from "vue";
import ChooseCounty from './components/ChooseCounty.vue'
import CountyData from "./components/CountyData.vue";

const county = ref('')

function onCountyChosen(newCounty) {
  county.value = newCounty
}
</script>

<template>
  <div class="min-h-screen bg-gray-100">
    <div class="container mx-auto px-6 pb-10 space-y-10">
      <h1
          class="text-3xl text-gray-800 font-extrabold text-center pt-20 md:text-5xl"
      >
        COVID-19 Totals By US County
      </h1>
      <ChooseCounty @county-chosen="onCountyChosen" />
      <CountyData :county="county"/>
    </div>
  </div>
</template>

