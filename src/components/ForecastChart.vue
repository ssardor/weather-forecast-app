<template>
    <div>
      <h4>Диаграмма температуры</h4>
      <canvas id="forecast-chart"></canvas>
    </div>
  </template>
  
  <script>
  import { Chart, registerables } from 'chart.js';
  import 'chartjs-adapter-date-fns';
  
  Chart.register(...registerables);
  
  export default {
    props: ['forecast'],
    mounted() {
      this.renderChart();
    },
    watch: {
      forecast() {
        this.renderChart();
      }
    },
    methods: {
      renderChart() {
        const ctx = document.getElementById('forecast-chart').getContext('2d');
        new Chart(ctx, {
          type: 'line',
          data: {
            labels: this.forecast.map(f => f.time),
            datasets: [{
              label: 'Температура (°C)',
              data: this.forecast.map(f => f.temperature),
              borderColor: 'rgba(75, 192, 192, 1)',
              borderWidth: 1
            }]
          },
          options: {
            scales: {
              x: {
                type: 'time',
                time: {
                  unit: 'hour',
                  tooltipFormat: 'PPpp'
                }
              },
              y: {
                beginAtZero: true
              }
            }
          }
        });
      }
    }
  };
  </script>
  