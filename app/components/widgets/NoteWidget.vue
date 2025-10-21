<template>
  <div class="bg-slate-900 text-white rounded-lg p-1 h-full flex flex-col">
    <textarea
      v-model="note"
      placeholder="Write your note..."
      class="w-full h-full bg-slate-800 rounded p-2 resize-none focus:outline-none"
    ></textarea>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const note = ref('')
</script>