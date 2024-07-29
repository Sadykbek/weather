
<script setup>
defineProps({
   temperature : Number,
   precipitation : Number,
   time : String,
   percent : Number
})
function whichImg(precipitation) {
   return `/src/assets/weather-${precipitation}.png`
}
</script>

<template>
   
   <div class=" bg-slate-400 flex flex-col items-center  justify-center border-2 px-4 py-2 border-slate-300 rounded-xl">
      <p class="text-3xl font-bold text-slate-700">{{ temperature }} °C</p>
      <img class="w-[100px]" :src="whichImg(precipitation)" alt="">
      <div class="flex gap-4"> <p class="text-blue-900">{{ percent }} %</p><p class="text-slate-600">{{ time}}</p></div>
    </div>
</template>