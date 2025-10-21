<template>
  <client-only>
    <div class="min-h-screen bg-slate-900 text-white p-6">
      <!-- Add Widget Button -->
      <button
    @click="addWidget"
    class="absolute top-4 left-4 z-10 bg-green-500 hover:bg-green-600 text-green-700 hover:text-white w-12 h-12 flex items-center justify-center rounded-full shadow transition text-2xl"
  >
     <PlusCircleIcon class="w-10 h-10" />
  </button>

      <!-- Grid -->
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
          <div
            class="h-full flex flex-col justify-between relative bg-slate-800 rounded-lg p-3 shadow hover:shadow-lg transition"
          >
            <!-- Dynamic widget -->
            <component
              v-if="item.type"
              :is="getComponent(item.type)"
              class="h-full"
            />

            <!-- Empty widget placeholder -->
            <div
              v-else
              class="h-full flex items-center justify-center text-slate-400"
            >
              Empty
            </div>

            <!-- Remove button -->
            <div class="absolute top-1 right-1 flex gap-1">
              <button
                @click="removeWidget(item.i)"
                class="bg-blue-900 hover:bg-red-700 text-slate-800 hover:text-white w-7 h-7 flex items-center justify-center rounded-full shadow transition text-2xl"
              >
                <XCircleIcon class="w-6 h-6" />
              </button>
            </div>

            <!-- Set Widget Button -->
            <div class="absolute inset-0 flex items-center justify-center pointer-events-none">
              <button
                v-if="!item.type"
                @click="selectClockWidget(item)"
                class="pointer-events-auto bg-blue-500 hover:bg-blue-600 text-white px-3 py-1.5 rounded text-xs transition"
              >
                Clock
              </button>
            </div>
          </div>
        </GridItem>
      </GridLayout>
    </div>
  </client-only>
</template>

<script setup>
import { ref } from 'vue'
import { GridLayout, GridItem } from 'vue3-grid-layout'
import { PlusCircleIcon, XCircleIcon} from '@heroicons/vue/24/solid'
import ClockWidget from './widgets/ClockWidget.vue'

const layout = ref([
  { i: 'a', x: 0, y: 0, w: 4, h: 3, type: 'ClockWidget' },
  { i: 'b', x: 4, y: 0, w: 4, h: 3, type: null },
  { i: 'c', x: 8, y: 0, w: 4, h: 3, type: null }
])

const widgetTypes = ['ClockWidget']

function getComponent(type) {
  const components = { ClockWidget }
  return components[type] || null
}

function selectClockWidget(item) {
  item.type = 'ClockWidget'
}

function removeWidget(id) {
  layout.value = layout.value.filter(item => item.i !== id)
}

function addWidget() {
  const id = Date.now().toString()
  layout.value.push({ i: id, x: 0, y: 0, w: 4, h: 3, type: null })
}
</script>





