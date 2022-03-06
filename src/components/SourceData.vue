<script setup>
import {computed} from "vue";

const props = defineProps({
  data: {
    type: Object,
    required: true
  }
})

const annotations = computed(() => {
  return props.data?.annotations;
})
const cases = computed(() => {
  return annotations.value?.cases?.sources[0] || null;
})
const icuBeds = computed(() => {
  return annotations.value?.icuBeds?.sources[0] || null;
})
const hospitalBeds = computed(() => {
  return annotations.value?.hospitalBeds?.sources[0] || null;
})
const vaccinations = computed(() => {
  return annotations.value?.vaccinationsCompleted?.sources[0] || null;
})
</script>

<template>
  <div class="space-y-2">
    <h3 class="text-4xl text-gray-800 font-bold text-center">
      Sources
    </h3>
    <p v-if="cases">
      <span class="text-gray-600 font-bold mr-2 block md:inline md:text-xl">Cases:</span>
      <a
        :href="cases.url"
        class="link"
      >
        {{ cases.name }}
      </a>
    </p>
    <p v-if="icuBeds">
      <span class="text-gray-600 font-bold mr-2 block md:inline md:text-xl">ICU Beds:</span>
      <a
        :href="icuBeds.url"
        class="link"
      >
        {{ icuBeds.name }}
      </a>
    </p>
    <p v-if="hospitalBeds">
      <span class="text-gray-600 font-bold mr-2 block md:inline md:text-xl">Hospital Beds:</span>
      <a
        :href="hospitalBeds.url"
        class="link"
      >
        {{ hospitalBeds.name }}
      </a>
    </p>
    <p v-if="vaccinations">
      <span class="text-gray-600 font-bold mr-2 block md:inline md:text-xl">Vaccinations:</span>
      <a
        :href="vaccinations.url"
        class="link"
      >
        {{ vaccinations.name }}
      </a>
    </p>
  </div>
</template>
