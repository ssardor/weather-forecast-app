<!-- src/App.vue -->
<template>
  <div id="app">
    
    <location-search @location-selected="handleLocationSelected"></location-search>
    <weather-display v-if="selectedLocation" :location="selectedLocation"></weather-display>
  </div>
</template>

<script>
import LocationSearch from './components/LocationSearch.vue';
import WeatherDisplay from './components/WeatherDisplay.vue';

export default {
  data() {
    return {
      selectedLocation: null
    };
  },
  components: {
    LocationSearch,
    WeatherDisplay
  },
  methods: {
    handleLocationSelected(location) {
      this.selectedLocation = location;
    }
  }
};
</script>
<style>
    .input{
        padding: 10px 90px;
        border-radius: 10px;
    }
    .center{
        text-align: center;
    }
    li{
      list-style-type: none ;
      cursor: pointer;
}
</style>