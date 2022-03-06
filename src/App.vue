<script setup>
import {onMounted, ref} from "vue";
import ChooseCounty from './components/ChooseCounty.vue'
import CountyData from "./components/CountyData.vue";
import StateData from "./components/StateData.vue";
import states from "./states";

const apiKey = "754ba7d2bbd5429ca95c4ee58b200e08"
const endpoint = "https://api.covidactnow.org/v2"

let countyData = ref({})
let stateData = ref({})

onMounted(() => {
  fetchCountyData();
  fetchStateData();
})

function fetchStateData(state = "WA") {
  return fetch(`${endpoint}/state/${state}.json?apiKey=${apiKey}`)
      .then(res => {
        if (res.ok) {
          return res.json()
        }
      })
      .then(response => {
        stateData.value = response;
      })
      .catch((error) => {
        console.error('There was a problem fetching state data: \n', error);
      });
}

function fetchCountyData(countyCode = "53011") {
  return fetch(`${endpoint}/county/${countyCode}.json?apiKey=${apiKey}`, {})
      .then(res => {
        if (res.ok) {
          return res.json()
        }
      })
      .then(response => {
        countyData.value = response;
      })
      .catch((error) => {
        console.error('There was a problem fetching county data: \n', error);
      });
}

function onCountyChosen(newCounty) {
  if (newCounty) {
    fetchCountyData(newCounty)
    const state = states.fips.filter(
        // get state FIPS from first 2 numbers in county FIPS
        state => state.id === newCounty.substring(0, 2)
    );
    fetchStateData(state[0].name)
  } else {
    fetchCountyData();
    fetchStateData();
  }
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
      <CountyData :county="countyData"/>
      <StateData :state="stateData"/>
    </div>
  </div>
</template>

