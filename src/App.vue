<script setup>
import {computed, onMounted, ref} from "vue";
import ChooseCounty from './components/ChooseCounty.vue'
import DisplayData from './components/DisplayData.vue'
import SourceData from './components/SourceData.vue'
import states from "./states";

const apiKey = "754ba7d2bbd5429ca95c4ee58b200e08"
const endpoint = "https://api.covidactnow.org/v2"

let countyData = ref({})
let stateData = ref({})

onMounted(() => {
  fetchCountyData();
  fetchStateData();
})

const sourceData = computed(() => {
  return countyData.value.annotations.hospitalBeds
      ? countyData.value
      : stateData.value;
})

function fetchStateData(state = "WA") {
  return fetch(`${endpoint}/state/${state}.json?apiKey=${apiKey}`, {})
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
      <h1 class="text-3xl text-gray-800 font-extrabold text-center pt-20 md:text-5xl">
        COVID-19 Totals By US County
      </h1>
      <div class="md:text-center">
        <ChooseCounty @county-chosen="onCountyChosen" />
        <p
          v-if="stateData"
          class="mt-3 mb-5 text-sm"
        >
          Last Updated:
          <span class="font-bold">{{ stateData.lastUpdatedDate }}</span>
        </p>
      </div>
      <div class=" p-6 md:px-32 mx-auto bg-white rounded-xl shadow-md overflow-hidden">
        <div class="md:flex">
          <DisplayData
            v-if="stateData.country"
            :title="`${stateData.state} State`"
            :data="stateData"
            class="md:mr-10"
          />
          <DisplayData
            v-if="countyData.state"
            :title="`${countyData.county}, ${countyData.state}`"
            :data="countyData"
            class="md:mr-10"
          />
        </div>
      </div>
      <div
        v-if="countyData.state"
        class="p-6 md:px-32 mx-auto bg-white rounded-xl shadow-md overflow-hidden"
      >
        <SourceData :data="sourceData" />
      </div>
    </div>
  </div>
</template>

