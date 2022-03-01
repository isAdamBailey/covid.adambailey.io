<script setup>
import {onMounted, ref, watch} from "vue";

const props = defineProps(['county'])

let countyData = ref({})

const apiKey = "754ba7d2bbd5429ca95c4ee58b200e08"

watch(() => props.county, (newCounty) => {
  if (newCounty) {
    fetchCountyData(newCounty)
  }
})

onMounted(() => {
  fetchCountyData();
})

function fetchCountyData(countyCode = "53011") {
  return fetch(`https://api.covidactnow.org/v2/county/${countyCode}.json?apiKey=${apiKey}`, {})
      .then(res => {
        if (res.ok) {
          return res.json()
        }
      })
      .then(response => {
        countyData.value = response;
      })
      .catch((error) => {
        console.error('Looks like there was a problem: \n', error);
      });
}
</script>

<template>

</template>