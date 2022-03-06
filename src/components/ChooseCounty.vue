<script setup>
import {onMounted, ref, watch} from 'vue'
import Multiselect from '@vueform/multiselect'

const emit = defineEmits(['county-chosen'])

let chosenCounty = ref({})
let countyCodes = ref([])

watch(chosenCounty, (newCounty) => {
  emit('county-chosen', newCounty)
})

onMounted(() => {
  fetchCountyCodes();
})

function fetchCountyCodes() {
  fetch("https://api.census.gov/data/2010/dec/sf1?get=NAME&for=county:*")
    .then(res => {
      return res.json()
    })
    .then(response => {
      const array = Object.keys(response).map(k => response[k]);
      array.forEach(i => {
        if (i[0] !== "NAME") {
          return countyCodes.value.push({ ["label"]: i[0], ["value"]: i[1] + i[2] });
        }
      });
    })
    .catch((error) => {
      console.error('There was a problem fetching county codes: \n', error);
    });
}
</script>

<template>
  <div class="md:text-center md:px-32">
    <Multiselect
      v-model="chosenCounty"
      placeholder="Enter County Name"
      :searchable="true"
      track-by="label"
      limit="50"
      :options="countyCodes"
    />
  </div>
</template>

<style src="@vueform/multiselect/themes/default.css"></style>

