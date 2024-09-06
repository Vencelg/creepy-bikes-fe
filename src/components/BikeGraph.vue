<script setup>
import { ref, onMounted } from 'vue';
import { LineChart } from 'vue-chart-3';
import {
  Chart as ChartJS,
  Title,
  Tooltip,
  Legend,
  LineElement,
  PointElement,
  CategoryScale,
  LinearScale,
  LineController,
  Filler
} from 'chart.js';

ChartJS.register(
  Title,
  Filler,
  Tooltip,
  Legend,
  LineElement,
  PointElement,
  CategoryScale,
  LinearScale,
  LineController
);

const chartData = ref(null);
const chartOptions = ref(null);
const props = defineProps({
  bikeCount: Number
});

const generateTimeLabels = () => {
  const labels = [];
  const now = new Date();
  const startOfDay = new Date(now.getFullYear(), now.getMonth(), now.getDate(), 0, 0, 0);
  const diffInMinutes = Math.floor((now - startOfDay) / 60000);

  for (let i = 0; i <= diffInMinutes; i += 30) {
    const time = new Date(startOfDay.getTime() + i * 60000);
    const formattedTime = time.toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' });
    labels.push(formattedTime);
  }

  return labels;
};

const generateYValues = (length) => {
  return Array.from({ length }, () => Math.floor(Math.random() * 11));
};

onMounted(() => {
  const labels = generateTimeLabels();
  const data = generateYValues(labels.length);
  data.pop();
  data.push(props.bikeCount);
  console.log(data);

  chartData.value = {
    labels,
    datasets: [
      {
        label: 'Bike Count Over Time',
        data,
        borderColor: 'rgba(255, 0, 0, 1)',
        backgroundColor: 'rgba(255, 0, 0, 0.1)',
        fill: true,
        tension: 0.3,
        pointRadius: 3
      }
    ]
  };

  chartOptions.value = {
    responsive: true,
    scales: {
      x: {
        title: {
          display: true,
          text: 'Time of Day'
        }
      },
      y: {
        title: {
          display: true,
          text: 'Bike Count'
        },
        beginAtZero: true
      }
    }
  };
});
</script>

<template>
  <div>
    <h1>Bike availability during the day</h1>
    <LineChart :chart-data="chartData" :chart-options="chartOptions" />
  </div>
</template>
