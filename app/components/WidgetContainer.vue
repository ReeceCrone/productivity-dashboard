<template>
  <client-only>
    <GridLayout
      v-model:layout="layout"
      :col-num="12"
      :row-height="50"
      :is-draggable="true"
      :is-resizable="true"
      :margin="[10, 10]"
      :vertical-compact="false"
    >
      <GridItem
        v-for="item in layout"
        :key="item.i"
        :i="item.i"
        :x="item.x"
        :y="item.y"
        :w="item.w"
        :h="item.h"
      >
        <div class="h-full flex flex-col justify-between relative">
          <!-- Dynamic component rendering -->
          <component
            v-if="item.type"
            :is="getComponent(item.type)"
            class="h-full"
          />
          <div v-else class="bg-slate-800 rounded-lg flex items-center justify-center h-full">
            Empty
          </div>

          <!-- Controls -->
          <div class="absolute top-1 right-1 flex gap-1">
            <button @click="selectWidgetType(item)" class="bg-blue-500 text-white px-2 py-1 rounded text-xs">
              Set
            </button>
            <button @click="removeWidget(item.i)" class="bg-red-500 text-white px-2 py-1 rounded text-xs">
              X
            </button>
          </div>
        </div>
      </GridItem>
    </GridLayout>

    <button @click="addWidget" class="absolute top-2 right-2 z-10 bg-green-500 text-white px-4 py-2 rounded shadow">
      Add Widget
    </button>
  </client-only>
</template>

<script setup>
import { ref } from 'vue'
import { GridLayout, GridItem } from 'vue3-grid-layout'
import ClockWidget from './widgets/ClockWidget.vue'

const layout = ref([
  { i: 'a', x: 0, y: 0, w: 4, h: 3, type: 'ClockWidget' },
  { i: 'b', x: 4, y: 0, w: 4, h: 3, type: null },
  { i: 'c', x: 8, y: 0, w: 4, h: 3, type: null }
])

const widgetTypes = ['ClockWidget'] // can expand later

function getComponent(type) {
  const components = {
    ClockWidget
  }
  return components[type] || null
}

function selectWidgetType(item) {
  const type = prompt(`Choose a widget type:\n${widgetTypes.join(', ')}`, item.type || '')
  if (type && widgetTypes.includes(type)) {
    item.type = type
  }
}

function removeWidget(id) {
  layout.value = layout.value.filter(item => item.i !== id)
}

function addWidget() {
  const id = Date.now().toString()
  layout.value.push({ i: id, x: 0, y: 0, w: 4, h: 3, type: null })
}
</script>




