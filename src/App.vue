<script setup>
import { ref, onMounted, provide, watch } from 'vue'
import axios from 'axios'
import hourlyMenu from './components/hourlyMenu.vue'

let DayWeatherData = ref(null)
let k = ref(null)
let is_day = ref(null)

onMounted(async () => {
  try {
    const response = await axios.get('https://api.open-meteo.com/v1/forecast', {
      params: {
        latitude: 44.8252,
        longitude: 65.4889,
        current: 'temperature_2m,cloud_cover,relative_humidity_2m,is_day,precipitation,rain,showers,snowfall,surface_pressure,wind_speed_10m',
        timezone: 'auto',
        forecast_days: 1
      }
    })
    console.log(response.data)
    DayWeatherData.value = response.data
  } catch (error) {
    console.error('Error fetching weather data:', error)
  }
})

watch(DayWeatherData, (newData) => {
  if (newData && newData.current) {
    k.value = newData.current
    is_day.value = newData.current.is_day
  }
})

function imgContent(precipitation) {
  if (precipitation < 10 && precipitation >= 0) {
    return is_day.value != 1 ? 5 : 1
  } else if (precipitation < 20 && precipitation >= 10) {
    return is_day.value != 1 ? 6 : 2
  } else if (precipitation < 30 && precipitation >= 20) {
    return 3
  } else if (precipitation >= 30) {
    return 4
  }
}

function whichImg(precipitation) {
  return `/src/assets/weather-${precipitation}.png`
}

function weatherComent(rain, showers, snowfall, cloud_cover) {
  if (rain == 0 && showers == 0 && snowfall == 0 && cloud_cover == 0) {
    return is_day.value == 1 ? 'sunny' : 'clear'
  } else if (rain > 0 || showers > 0 || snowfall > 0 || cloud_cover > 0) {
    if (showers > 0) return 'showers'
    if (rain > 0) return 'rainy'
    if (snowfall > 0) return 'snowy'
    if (cloud_cover > 0) return 'cloudy'
  }
}

provide('is_day', is_day)
provide('imgContent', imgContent)
provide('whichImg', whichImg)
</script>

<template>
  <div v-if="k">
    <div class="pt-10" :class="k.is_day == 1 ? 'bgday' : 'bgnight'">
      <h1
        :class="k.is_day == 1 ? 'text-slate-700' : 'text-slate-200'"
        class="ml-32 text-3xl font-bold uppercase font-serif"
      >
        weather in Kyzylorda
      </h1>
      <div class="flex ml-32 mt-10">
        <img class="w-auto h-[200px]" :src="whichImg(imgContent(k.precipitation))" alt="">
        <div class="flex flex-col gap-4">
          <p :class="k.is_day == 1 ? 'text-slate-800' : 'text-slate-200'" class="text-3xl ml-4 font-semibold font-mono tracking-tighter">{{ k.temperature_2m }}°C</p>
          <div :class="k.is_day == 1 ? 'text-slate-800' : 'text-slate-200'" class="flex flex-col ml-4 text-lg gap-0">
            <p>precipitation: {{ k.precipitation }} mm</p>
            <p>wind speed: {{ k.wind_speed_10m }} km/h</p>
            <p>relative humidity: {{ k.relative_humidity_2m }} %</p>
          </div>
          <p class="ml-4 text-lg" :class="k.is_day == 1 ? 'text-slate-800' : 'text-slate-200'">{{ weatherComent(k.precipitation, k.showers, k.snowfall, k.cloud_cover) }}</p>
        </div>
      </div>
      <h2
        :class="k.is_day == 1 ? 'text-slate-700' : 'text-slate-200'"
        class="ml-32 mt-10 text-3xl font-bold uppercase font-serif"
      >
        weather in Kyzylorda for 24 hours
      </h2>
      <hourlyMenu />
    </div>
  </div>
</template>

<style>
.bgday {
  background: url('../public/bgday.jpg');
  background-repeat: no-repeat;
  background-size: cover;
  width: 100vw;
  min-height: 100vh;
}

.bgnight {
  background: url('../public/bgnight.png');
  background-repeat: no-repeat;
  background-size: cover;
  width: 100%;
  min-height: 100vh;
}
</style>
