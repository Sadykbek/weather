<script setup>
import { defineProps, ref, inject } from 'vue'
defineProps({
  items: Object
})
import hour from './hour.vue'
let is_day = inject('is_day')
let k = ref(1)
function splitHours(time) {
  return time.split('T')[1]
}
// https://api.open-meteo.com/v1/forecast?latitude=44.8252&longitude=65.4889&current=temperature_2m,relative_humidity_2m,is_day,precipitation,rain,showers,snowfall,surface_pressure,wind_speed_10m&timezone=auto&forecast_days=1
let imgContent = inject('imgContent')

</script>
<style>
/* Убираем стрелочки у скроллбара */
.no-scrollbar::-webkit-scrollbar-button {
  display: none;
}

/* Убираем фон дорожки скроллбара */
.no-scrollbar::-webkit-scrollbar-track {
  background: transparent;
}

/* Убираем углы у скроллбара */
.no-scrollbar::-webkit-scrollbar {
  background: transparent;
}

</style>

<template>
  <div :class="is_day==1 ? 'scrollbar-track-slate-300 scrollbar-thumb-sky-800 ' : 'scrollbar-track-slate-700 scrollbar-thumb-sky-500 '"  class="flex  py-4 px-4 gap-4 rounded-2xl w-10/12 mx-auto mt-10 overflow-auto scrollbar scrollbar-thin  scrollbar-thumb-rounded scrollbar-track-rounded ">
    <hour @v-if="items.apparent_temperature" class="w-fit flex-none"
      v-for="(time, index) in items.time"
      :key="index"
      :time="splitHours(time)"
      :temperature="items.apparent_temperature[index]"
      :precipitation="imgContent(items.precipitation[index])"
      :percent="items.precipitation_probability[index]"
    />
  </div>
</template>




