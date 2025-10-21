<template>
  <div class="bg-slate-800 p-6 rounded-2xl text-center shadow-md">
    <h2 class="text-xl font-semibold mb-2">🕒 Clock</h2>
    <p class="text-3xl font-mono">{{ time }}</p>
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