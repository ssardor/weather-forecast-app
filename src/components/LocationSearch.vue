<!-- src/components/LocationSearch.vue -->
<template>
    <div class="center">
        <h1>Прогноз погоды</h1>
      <input v-model="query" @input="fetchLocations" placeholder="Введите местоположение" class="input"/>
      <ul v-if="locations.length">
        <li v-for="location in locations" :key="location.id" @click="selectLocation(location)">
          {{ location.name }} ({{ location.latitude }}, {{ location.longitude }})
        </li>
      </ul>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        query: '',
        locations: []
      };
    },
    methods: {
      async fetchLocations() {
        if (this.query.length < 3) return;
        const response = await axios.get(`https://geocoding-api.open-meteo.com/v1/search`, {
          params: {
            name: this.query,
            count: 10,
            language: 'en',
            format: 'json'
          }
        });
        this.locations = response.data.results;
      },
      selectLocation(location) {
        this.$emit('location-selected', location);
      }
    }
  };
  </script>
  