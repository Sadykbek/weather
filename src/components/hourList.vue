<script setup>
import { defineProps, ref } from 'vue'
defineProps({
  items: Object
})
import hour from './hour.vue'
let k = ref(1)
function splitHours(time) {
  return time.split('T')[1]
}

function imgContent(precipitation) {
  if (precipitation < 10 && precipitation >= 0) {
    return 1
  } else if (precipitation < 20 && precipitation >= 10) {
    return 2
  } else if (precipitation < 30 && precipitation >= 20) {
    return 3
  } else if (precipitation >= 30) {
    return 4
  }
}
</script>
<template>
  <div class="flex gap-4 flex-wrap">
    <hour
      v-for="(time, index) in items.time"
      :key="index"
      :time="splitHours(time)"
      :temperature="items.apparent_temperature[index]"
      :precipitation="imgContent(items.precipitation[index])"
      :percent="items.precipitation_probability[index]"
    />
  </div>
</template>
