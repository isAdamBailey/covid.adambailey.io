<script setup>
import { computed } from "vue";
import DataRow from "./DataRow.vue";
import Chart from "./Chart.vue";
import DataObject from "./DataObject.vue";

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
const percentHasBooster = computed(() => {
  return vaccinationsAdditionalDose.value
      ? percentOf(vaccinationsAdditionalDose.value, props.data.population)
      : 0;
})
const percentVaccinated = computed(() => {
  return vaccinationsCompleted.value
      ? percentOf(vaccinationsCompleted.value, props.data.population)
      : 0;
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
  <div class="p-6 md:px-12 mx-auto bg-white rounded-xl shadow-md overflow-hidden md:w-1/2 mt-3 md:mt-0">
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
    <DataObject
      v-if="cases"
      title="Total versus population"
    >
      <Chart
        :title="`Cases: ${cases.toLocaleString()}`"
        :data="[cases, population - cases]"
      />
      <Chart
        v-if="deaths"
        :title="`Deaths: ${deaths.toLocaleString()}`"
        :data="[deaths, population - deaths]"
      />
    </DataObject>

    <h1 class="text-3xl text-blue-600 font-bold mt-6">
      Vaccines
    </h1>
    <p>(Pfizer/ Moderna)</p>
    <DataObject title="Total versus population">
      <Chart
        v-if="vaccinationsInitiated"
        title="First Dose"
        :data="[vaccinationsInitiated, population - vaccinationsInitiated]"
      />
      <Chart
        v-if="vaccinationsCompleted"
        :title="`Second Dose ${percentVaccinated}%`"
        :data="[vaccinationsCompleted, population - vaccinationsCompleted]"
      />
      <Chart
        v-if="vaccinationsAdditionalDose"
        :title="`Boosters: ${percentHasBooster}%`"
        :data="[vaccinationsAdditionalDose, population - vaccinationsAdditionalDose]"
      />
    </DataObject>

    <h1 class="text-3xl text-blue-600 font-bold mt-6">
      ICU Beds
    </h1>
    <DataObject title="Total capacity">
      <Chart
        v-if="icuBeds.currentUsageTotal"
        :title="`Capacity : ${percentIcuCapacity}%`"
        :labels="['Usage', 'Capacity']"
        :data="[icuBeds.currentUsageTotal, icuBeds.capacity - icuBeds.currentUsageTotal]"
      />
      <Chart
        v-if="icuBeds.currentUsageCovid"
        title="COVID Cases"
        :labels="['COVID', 'NON-COVID']"
        :data="[icuBeds.currentUsageCovid, icuBeds.currentUsageTotal - icuBeds.currentUsageCovid]"
      />
    </DataObject>

    <h1 class="text-3xl text-blue-600 font-bold mt-6">
      Hospital Beds
    </h1>
    <DataObject title="Total capacity">
      <Chart
        v-if="hospitalBeds.currentUsageTotal"
        :title="`Capacity: ${percentBedCapacity}%`"
        :labels="['Usage', 'Capacity']"
        :data="[hospitalBeds.currentUsageTotal, hospitalBeds.capacity - hospitalBeds.currentUsageTotal]"
      />
      <Chart
        v-if="hospitalBeds.currentUsageCovid"
        title="COVID Cases"
        :labels="['COVID', 'NON-COVID']"
        :data="[hospitalBeds.currentUsageCovid, hospitalBeds.currentUsageTotal - hospitalBeds.currentUsageCovid]"
      />
    </DataObject>
  </div>
</template>
