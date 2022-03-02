<template>
  <div>
    <canvas id="myChart" width="400" height="400"></canvas>
  </div>
</template>
<script>
import { Chart, registerables } from "chart.js";
Chart.register(...registerables);
import { onMounted } from "vue";

export default {
  name: "graficoPareto",
  props: {
    data: Array,
    mes: Array,
  },
  setup(props) {
    const total = props.data.reduce((x, y) => x + y, 0);

    const result = props.data.reduce(
      (acc, obj) => {
        let partialTotal = acc.valores.reduce((x, y) => x + y, 0);
        let percentage = Math.floor((100 * (obj + partialTotal)) / total);

        return acc = { valores: [...acc.valores, obj], porcentagens: [...acc.porcentagens, percentage] }
      },
      { valores: [], porcentagens: [] }
    );

    onMounted(() => {
      new Chart(document.getElementById("myChart"), {
        data: {
          labels: props.mes,
          datasets: [
            {
              type: "bar",
              label: "2018 Sales",
              borderWidth:1,
              borderColor: "rgba(50, 50, 220,0.7)",
              backgroundColor: "rgba(50, 115, 220, 0.5)",
              data: result.valores,
              barPercentage: 1,
              categoryPercentage: 1,
            },
            {
              type: "line",
              label: "2018 Sales",
              borderColor: "rgba(255, 56, 96, 0.5)",
              backgroundColor: "rgba(255, 56, 96, 0.1)",
              yAxisID: "percentageAxis",
              data: result.porcentagens,
            },
          ],
        },
        options: {
          plugins: {
            tootip: {
              anabled: false,
            },
          },
          scales: {
            y: {
              beginAtZero: true,
            },
            percentageAxis: {
              position: "right",
              beginAtZero: true,
              min: 0,
              max: 100,
              ticks: {
                callback: function (value) {
                  return value + "%";
                },
              },
            },
          },
        },
      });
    });
  },
};
</script>