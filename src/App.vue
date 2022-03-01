<script setup>
import { ref, onMounted } from 'vue'
import HelloWorld from './components/HelloWorld.vue'

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
      return countyCodes.value.push({ ["name"]: i[0], ["id"]: i[1] + i[2] });
    });
  }).catch((error) => {
    console.error('Looks like there was a problem: \n', error);
  });
}
</script>

<template>
  <img alt="Vue logo" src="./assets/logo.png" />
  <HelloWorld :county-codes="countyCodes" />
</template>

