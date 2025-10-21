<template>
  <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4">
    <component
      v-for="(widget, index) in widgets"
      :key="index"
      :is="widget.component"
    />
  </div>
</template>

<script setup>
import ClockWidget from '~/components/widgets/ClockWidget.vue'
// import other widgets here as you build them
// import TodoWidget from '~/components/widgets/TodoWidget.vue'

import { ref } from 'vue'

// list of widgets to display
const widgets = ref([
  { name: 'Clock', component: ClockWidget },
  // add other widgets here
])
</script>
