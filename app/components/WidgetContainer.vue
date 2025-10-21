<template>
  <client-only>
    <div class="mb-4 flex gap-2">
      <button @click="addWidget" class="bg-blue-500 text-white px-4 py-2 rounded">
        Add Widget
      </button>
    </div>

    <GridLayout
      v-model:layout="layout"
      :col-num="12"
      :row-height="50"
      :is-draggable="true"
      :is-resizable="true"
      :vertical-compact="false"
      :margin="[10, 10]"
    >
      <GridItem v-for="item in layout" :key="item.i" :i="item.i" :x="item.x" :y="item.y" :w="item.w" :h="item.h">
        <div class="bg-slate-800 rounded-lg p-4 text-center h-full flex items-center justify-center">
          {{ item.i.toUpperCase() }}
        </div>
      </GridItem>
    </GridLayout>
  </client-only>
</template>

<script setup>
import { ref } from 'vue'
import { GridLayout, GridItem } from 'vue3-grid-layout'

const layout = ref([
  { i: 'a', x: 0, y: 0, w: 4, h: 3 },
  { i: 'b', x: 4, y: 0, w: 4, h: 3 },
  { i: 'c', x: 8, y: 0, w: 4, h: 3 },
  { x: 2, y: 0, w: 2, h: 4, i: '1'},
])

// simple counter for unique IDs
let widgetCounter = 1

function addWidget() {
  const newWidget = {
    i: `widget-${widgetCounter++}`,
    x: 0,      // default position, you can calculate next free spot
    y: 0,
    w: 4,
    h: 3
  }
  layout.value.push(newWidget)
}
</script>




