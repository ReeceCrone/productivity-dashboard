<template>
  <div class="bg-slate-900 text-white rounded-lg p-1 h-full flex items-center justify-center font-mono">
    <textarea
      v-model="note"
      placeholder="Write your label..."
      class="w-full h-full bg-slate-800 rounded p-2 resize-none focus:outline-none text-6xl text-center"
    ></textarea>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const note = ref('')
</script>