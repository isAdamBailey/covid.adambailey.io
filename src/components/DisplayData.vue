<script setup>
import { computed } from "vue";
import DataRow from "./DataRow.vue";
import Chart from "./Chart.vue";

const props = defineProps({
  title: {
    type: String,
    default: "Title"
  },
  data: {
    type: Object,
    required: true
  }
})

function percentOf(amount, total) {
  return ((amount / total) * 100).toFixed(2);
}

const population = computed(() => {
  return props.data.population;
})
const cases = computed(() => {
  return props.data.actuals.cases;
})
const newCases = computed(() => {
  return props.data.actuals.newCases;
})
const deaths = computed(() => {
  return props.data.actuals.deaths;
})
const icuBeds = computed(() => {
  return props.data.actuals.icuBeds;
})
const hospitalBeds = computed(() => {
  return props.data.actuals.hospitalBeds;
})
const vaccinationsAdditionalDose = computed(() => {
  return props.data.actuals.vaccinationsAdditionalDose || "";
})
const vaccinationsCompleted = computed(() => {
  return props.data.actuals.vaccinationsCompleted || "";
})
const vaccinationsInitiated = computed(() => {
  return props.data.actuals.vaccinationsInitiated || "";
})
const percentIcuCapacity = computed(() => {
  return icuBeds.value.currentUsageTotal
      ? percentOf(icuBeds.value.currentUsageTotal, icuBeds.value.capacity)
      : 0;
})
const percentBedCapacity = computed(() => {
  return hospitalBeds.value.currentUsageTotal
      ? percentOf(hospitalBeds.value.currentUsageTotal, hospitalBeds.value.capacity)
      : 0;
})
</script>

<template>
  <div class="p-6 md:px-32 mx-auto bg-white rounded-xl shadow-md overflow-hidden md:w-1/2 mt-3 md:mt-0">
    <h3 class="text-4xl text-gray-800 font-bold text-center">
      {{ title }}
    </h3>
    <data-row
      title="Population:"
      :value="population.toLocaleString()"
    />

    <h1 class="text-3xl text-blue-600 font-bold mt-6">
      Cases
    </h1>
    <data-row
      v-if="newCases"
      title="New Cases:"
      :value="newCases.toLocaleString()"
    />
    <div class="flex justify-around align-center mt-3">
      <Chart
        v-if="cases"
        :title="`Cases: ${cases.toLocaleString()}`"
        :labels="['Cases', 'Population']"
        :data="[cases, population]"
      />
      <Chart
        v-if="deaths"
        :title="`Deaths: ${deaths.toLocaleString()}`"
        :labels="['Deaths', 'Population']"
        :data="[deaths, population]"
      />
    </div>

    <h1 class="text-3xl text-blue-600 font-bold mt-6">
      Vaccines
    </h1>
    <p>(Pfizer/ Moderna)</p>
    <div class="flex justify-around align-center mt-3">
      <Chart
        v-if="vaccinationsInitiated"
        title="First Dose"
        :labels="['First Dose', 'Population']"
        :data="[vaccinationsInitiated, population]"
      />
      <Chart
        v-if="vaccinationsCompleted"
        title="Second Dose"
        :labels="['Second Dose', 'Population']"
        :data="[vaccinationsCompleted, population]"
      />
      <Chart
        v-if="vaccinationsAdditionalDose"
        title="Boosters"
        :labels="['Boosters', 'Population']"
        :data="[vaccinationsAdditionalDose, population]"
      />
    </div>

    <h1 class="text-3xl text-blue-600 font-bold mt-6">
      ICU Beds
    </h1>
    <div class="flex justify-around align-center mt-3">
      <Chart
        v-if="icuBeds.currentUsageTotal"
        :title="`Capacity : ${percentIcuCapacity}%`"
        :labels="['Usage', 'Capacity']"
        :data="[icuBeds.currentUsageTotal, icuBeds.capacity]"
      />
      <Chart
        v-if="icuBeds.currentUsageCovid"
        title="COVID Cases"
        :labels="['COVID', 'NON-COVID']"
        :data="[icuBeds.currentUsageCovid, icuBeds.currentUsageTotal - icuBeds.currentUsageCovid]"
      />
    </div>

    <h1 class="text-3xl text-blue-600 font-bold mt-6">
      Hospital Beds
    </h1>
    <div class="flex justify-around align-center mt-3">
      <Chart
        v-if="hospitalBeds.currentUsageTotal"
        :title="`Capacity: ${percentBedCapacity}%`"
        :labels="['Usage', 'Capacity']"
        :data="[hospitalBeds.currentUsageTotal, hospitalBeds.capacity]"
      />
      <Chart
        v-if="hospitalBeds.currentUsageCovid"
        title="COVID Cases"
        :labels="['COVID', 'NON-COVID']"
        :data="[hospitalBeds.currentUsageCovid, hospitalBeds.currentUsageTotal - hospitalBeds.currentUsageCovid]"
      />
    </div>
  </div>
</template>
