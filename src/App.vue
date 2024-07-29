<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';
import hourList from './components/hourList.vue'; // Ensure the path is correct

const weatherData = ref(null);
const newHourly = ref({
  time: [],
  apparent_temperature: [],
  precipitation_probability: [],
  precipitation: [],
  rain: [],
});

const fetchWeatherData = async () => {
  try {
    const response = await axios.get('https://api.open-meteo.com/v1/forecast', {
      params: {
        latitude: 44.8332,
        longitude: 65.4836,
        hourly: 'apparent_temperature,precipitation_probability,precipitation,rain',
        timezone: 'auto',
        forecast_days: 3,
      },
    });
    weatherData.value = response.data;

    const currentDateTime = new Date();
    const currentHour = currentDateTime.getHours();
    const currentDay = currentDateTime.getDate();

    const currentHourIndex = weatherData.value.hourly.time.findIndex(time => {
      const timeDate = new Date(time);
      return timeDate.getHours() === currentHour && timeDate.getDate() === currentDay;
    });

    newHourly.value = {
      time: weatherData.value.hourly.time.slice(currentHourIndex, currentHourIndex + 24),
      apparent_temperature: weatherData.value.hourly.apparent_temperature.slice(currentHourIndex, currentHourIndex + 24),
      precipitation_probability: weatherData.value.hourly.precipitation_probability.slice(currentHourIndex, currentHourIndex + 24),
      precipitation: weatherData.value.hourly.precipitation.slice(currentHourIndex, currentHourIndex + 24),
      rain: weatherData.value.hourly.rain.slice(currentHourIndex, currentHourIndex + 24),
    };

    console.log(newHourly.value);
  } catch (error) {
    console.error('Error fetching weather data:', error);
  }
};

onMounted(() => {
  fetchWeatherData();
});
</script>

<template>
  <div class="flex wrap">
    <hourList :items="newHourly"/>
  </div>
</template>
