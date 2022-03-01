<script setup>
import {onMounted, ref} from 'vue'
import Multiselect from '@vueform/multiselect'

let chosenCounty = ref({})
let countyCodes = ref([])

onMounted(() => {
  fetchCountyCodes();
})

function fetchCountyCodes() {
  fetch("https://api.census.gov/data/2010/dec/sf1?get=NAME&for=county:*", {
  })
      .then(res => {
        return res.json()
      }).then(response => {
    const array = Object.keys(response).map(k => response[k]);
    array.forEach(i => {
      return countyCodes.value.push({ ["label"]: i[0], ["value"]: i[1] + i[2] });
    });
  }).catch((error) => {
    console.error('Looks like there was a problem: \n', error);
  });
}
</script>

<template>
    <div class="md:text-center md:px-32">
      <Multiselect
          size="lg"
          placeholder="Enter County Name"
          :searchable="true"
          track-by="label"
          v-model="chosenCounty"
          :options="countyCodes"
      />
    </div>
</template>

<style src="@vueform/multiselect/themes/default.css"></style>

