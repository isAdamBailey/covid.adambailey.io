<script setup>
const props = defineProps({
  title: {
    type: String,
    default: "Title"
  },
  value: {
    type: String,
    required: true
  }
})
</script>

<template>
  <p>
    <span class="text-gray-600 font-bold mr-2 block md:inline md:text-xl">
      {{ props.title }}
    </span>
    <span class="text-gray-800 text-2xl font-bold mr-2">
      {{ props.value }}
    </span>
  </p>
</template>

