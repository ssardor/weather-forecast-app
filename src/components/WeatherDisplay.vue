<!-- src/components/WeatherDisplay.vue -->
<template>
    <div v-if="weather" class="center">
      <h2>Текущие погодные условия в {{ location.name }}</h2>
      <p>Температура: {{ weather.current }}°C</p>
      <h3>Прогноз</h3>
      <forecast-chart :forecast="weather.forecast"></forecast-chart>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  import ForecastChart from './ForecastChart.vue';
  
  export default {
    components: {
      ForecastChart
    },
    props: ['location'],
    data() {
      return {
        weather: null
      };
    },
    watch: {
      location: 'fetchWeather'
    },
    methods: {
  async fetchWeather() {
    if (!this.location) return;
    const { latitude, longitude } = this.location;
    const response = await axios.get('https://api.open-meteo.com/v1/forecast', {
      params: {
        latitude,
        longitude,
        hourly: 'temperature_2m',
        start_date: new Date().toISOString().slice(0, 10),
        end_date: new Date(new Date().setDate(new Date().getDate() + 1)).toISOString().slice(0, 10)
      }
    });

    const data = response.data.hourly.temperature_2m;
    const current = data[new Date().getHours()];
    const forecast = data.map((temp, index) => ({
      time: new Date(new Date().setHours(new Date().getHours() + index)),
      temperature: temp
    }));

    this.weather = { current, forecast };
  }
}

  };
  </script>