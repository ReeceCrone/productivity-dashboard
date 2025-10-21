<template>
  <div class="bg-slate-900 text-white rounded-lg flex items-center justify-center p-1">
    <p class="bg-slate-800 rounded w-full h-full flex items-center justify-center text-3xl font-mono">
      {{ time }}
    </p>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

// reactive variable to store current time
const time = ref('')

// function to update time every second
function updateTime() {
  time.value = new Date().toLocaleTimeString()
}

// start updating when component mounts
onMounted(() => {
  updateTime()               // set initial time
  setInterval(updateTime, 1000) // update every second
})
</script>

<style scoped>
/* optional extra styling */
</style>