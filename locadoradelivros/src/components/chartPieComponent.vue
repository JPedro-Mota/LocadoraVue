<template>
  <div>
    <div class="chart-container">
      <div class="title">Livros mais alugados</div>
      <canvas id="LivrosChart"></canvas>
    </div>
  </div>
</template>

<script setup>
import { useQuasar } from 'quasar';
import { ref, onMounted } from 'vue';
import { Chart, registerables } from 'chart.js';
import { api, authenticate } from 'src/boot/axios';

const $q = useQuasar();

const showNotification = (type, msg) => {
  $q.notify({
    type: type,
    message: msg,
    position: 'bottom-right',
    timeout: 3000
  });
};

Chart.register(...registerables);

const mostRented1 = ref({});
const mostRented2 = ref({});
const mostRented3 = ref({});

const getMostRentedData = async () => {
  try {
    await authenticate();
    const responses = await Promise.all([
      api.get('/rent/most-rented/1'),
      api.get('/rent/most-rented/2'),
      api.get('/rent/most-rented/3')
    ]);
    mostRented1.value = responses[0].data;
    mostRented2.value = responses[1].data;
    mostRented3.value = responses[2].data;
  } catch (error) {
    showNotification('negative', "Erro ao obter dados!");
    console.error("Erro ao obter dados:", error);
  }
};

onMounted(async () => {
  await getMostRentedData();

  const ctx2 = document.getElementById('LivrosChart').getContext('2d');
  new Chart(ctx2, {
    type: 'pie',
    data: {
      labels: [mostRented1.value.bookName, mostRented2.value.bookName, mostRented3.value.bookName],
      datasets: [{
        label: 'Livros mais alugados',
        data: [mostRented1.value.rentedNumber, mostRented2.value.rentedNumber, mostRented3.value.rentedNumber],
        backgroundColor: ['#509358', '#B22222', '#46769A'],
        borderWidth: 0
      }]
    },
    options: {
      responsive: true,
      maintainAspectRatio: false
    }
  });
});
</script>

<style scoped>
.chart-container {
  border-radius: 5px;
  padding: 15px;
  box-shadow: 1px 2px 6px 2px rgba(87, 87, 87, 0.51);
  background-color: rgb(255, 255, 255);
  width: 100%;
  min-width: 200px;
  height: 300px;
  text-align: center;
}

.title {
  margin-bottom: 1px;
  font-weight: bold;
}

canvas {
  width: 100%;
  height: 100%; /* Ajuste conforme necessário */
}
</style>
