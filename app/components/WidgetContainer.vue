<template>
  <client-only>
    <GridLayout
      v-model:layout="layout"
      :col-num="12"
      :row-height="50"
      :is-draggable="true"
      :is-resizable="true"
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
])
</script>




