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
<style scoped>
textarea::-webkit-scrollbar {
  width: 6px;
}
textarea::-webkit-scrollbar-thumb {
  background-color: rgba(100, 116, 139, 0.5); /* slate-500 */
  border-radius: 10px;
}
textarea::-webkit-scrollbar-thumb:hover {
  background-color: rgba(148, 163, 184, 0.8); /* slate-400 */
}
</style>