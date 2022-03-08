<template>
  <PieChart
    :chart-data="chartData"
    :options="options"
    class="w-1/3"
  />
</template>

<script setup>
import {computed, defineProps, ref} from "vue";
import { PieChart } from 'vue-chart-3';
import { Chart, registerables } from "chart.js";

Chart.register(...registerables);

const props = defineProps({
  title: {
    type: String,
    default: null
  },
  labels: {
    type: Array,
    default: []
  },
  data: {
    type: Array,
    default: []
  }
})

const options = ref({
  responsive: true,
  plugins: {
    legend: {
      display: false,
    },
    title: {
      display: !!props.title,
      text: props.title,
    },
  },
})

const chartData = computed(() => ({
  labels: props.labels,
  datasets: [
    {
      data: props.data,
      backgroundColor: ['#77CEFF', '#123E6B'],
    },
  ],
}));
</script>