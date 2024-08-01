
<script setup>
import {inject} from 'vue'
defineProps({
   temperature : Number,
   precipitation : Number,
   time : String,
   percent : Number
})
const is_day = inject('is_day')
const whichImg = inject('whichImg')
</script>

<template>
   
   <div :class="is_day==1 ? 'bg-sky-400' : 'bg-sky-700'" class="  flex flex-col items-center  justify-center border-2 px-4 py-2 border-slate-300 rounded-xl border-solid border-2 border-slate-500 ">
      <p :class="is_day==1 ? 'text-slate-700' : 'text-slate-300'" class="text-3xl font-bold ">{{ temperature }} °C</p>
      <img class="w-[100px]" :src="whichImg(precipitation)" alt="">
      <div  class="flex gap-4"> <p :class="is_day==1 ? 'text-slate-900' : 'text-slate-300'" class="text-lg font-bold">{{ percent }} %</p><p :class="is_day==1 ? 'text-slate-800' : 'text-slate-200'" >{{ time}}</p></div>
    </div>
</template>