<script setup>
 const props = defineProps({
   title: {
     type: String,
     default: ''
   }
 })
</script>
<template>
  <div class="bg-gray-100 p-3 rounded-xl shadow-md">
    <div class="text-sm text-blue-900">
      {{ props.title }}
    </div>
    <div class="flex justify-around align-center mt-3">
      <slot />
    </div>
  </div>
</template>