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
  },
  color: {
    type: String,
    default: null
  }
})

const options = ref({
  responsive: true,
  plugins: {
    legend: {
      display: false,
      font: {
        weight: 'bold'
      }
    },
    title: {
      display: !!props.title,
      text: props.title,
    },
    tooltip: {
      enabled: !!props.labels.length
    }
  },
})

function getBackgroundColor() {
  const blue = ['#60a5fa', '#1e3a8a'];
  const purple = ['#c084fc', '#581c87'];
  const red = ['#fb7185', '#881337'];
  const randomColor = color => color[Math.floor(Math.random() * color.length)];
  switch (props.color) {
    case 'purple':
      return purple
    case 'red':
      return red
    case 'blue':
      return blue
    default:
      return randomColor([blue, purple,   red])
  }
}

const chartData = computed(() => ({
  labels: props.labels,
  datasets: [
    {
      data: props.data,
      backgroundColor: getBackgroundColor(),
    },
  ],
}));
</script>

<template>
  <PieChart
    :chart-data="chartData"
    :options="options"
    class="w-1/3"
  />
</template>