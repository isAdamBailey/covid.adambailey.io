<script setup>
import { computed } from "vue";
import DataRow from "./DataRow.vue";

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
const percentCases = computed(() => {
  return cases.value ? percentOf(cases.value, population.value) : 0;
})
const percentDeaths = computed(() => {
  return deaths.value ? percentOf(deaths.value, population.value) : 0;
})
const percentHasBooster = computed(() => {
  return vaccinationsAdditionalDose.value
      ? percentOf(vaccinationsAdditionalDose.value, population.value)
      : 0;
})
const percentVaccinated = computed(() => {
  return vaccinationsCompleted.value
      ? percentOf(vaccinationsCompleted.value, population.value)
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
  <div>
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

    <data-row
      v-if="cases"
      title="Current Cumulative Cases:"
      :value="`${cases.toLocaleString()} (${percentCases}%)`"
    />

    <data-row
      v-if="deaths"
      title="Current Cumulative Deaths:"
      :value="`${deaths.toLocaleString()} (${percentDeaths}%)`"
    />

    <h1 class="text-3xl text-blue-600 font-bold mt-6">
      ICU Beds
    </h1>
    <data-row
      v-if="icuBeds.currentUsageCovid"
      title="Current COVID Cases In ICU:"
      :value="icuBeds.currentUsageCovid.toLocaleString()"
    />

    <data-row
      v-if="icuBeds.currentUsageTotal"
      title="Current NON-COVID Cases In ICU:"
      :value="`${(icuBeds.currentUsageTotal - icuBeds.currentUsageCovid).toLocaleString()}`"
    />

    <data-row
      v-if="percentIcuCapacity"
      title="Capacity:"
      :value="`${percentIcuCapacity}%`"
    />

    <h1 class="text-3xl text-blue-600 font-bold mt-6">
      Hospital Beds
    </h1>
    <data-row
      v-if="hospitalBeds.currentUsageCovid"
      title="Current COVID Cases In Hospitals:"
      :value="hospitalBeds.currentUsageCovid.toLocaleString()"
    />

    <data-row
      v-if="hospitalBeds.currentUsageCovid"
      title="Current NON-COVID Cases In Hospitals:"
      :value="`${(hospitalBeds.currentUsageTotal - hospitalBeds.currentUsageCovid).toLocaleString()}`"
    />

    <data-row
      v-if="percentBedCapacity"
      title="Capacity:"
      :value="`${percentBedCapacity}%`"
    />

    <h1 class="text-3xl text-blue-600 font-bold mt-6">
      Vaccines
    </h1>
    <p>(Pfizer/ Moderna)</p>
    <data-row
      v-if="vaccinationsInitiated"
      title="First shots:"
      :value="vaccinationsInitiated.toLocaleString()"
    />

    <data-row
      v-if="vaccinationsCompleted"
      title="Second shots:"
      :value="vaccinationsCompleted.toLocaleString()"
    />

    <data-row
      v-if="vaccinationsCompleted"
      title="Percent fully vaxxed:"
      :value="`${percentVaccinated}%`"
    />

    <data-row
      v-if="vaccinationsAdditionalDose"
      title="Percent with booster:"
      :value="`${percentHasBooster}%`"
    />
  </div>
</template>
