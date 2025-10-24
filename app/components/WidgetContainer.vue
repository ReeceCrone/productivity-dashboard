<template>
  <client-only>
    <div class="min-h-screen bg-slate-900 text-white p-6">
      <!-- Add Widget Button -->
      <button
    @click="addWidget"
    class="fixed top-4 left-4 z-10 bg-green-500 hover:bg-green-600 text-green-700 hover:text-white w-12 h-12 flex items-center justify-center rounded-full shadow transition text-2xl"
  >
     <PlusCircleIcon class="w-13 h-13" />
  </button>

      <!-- Grid -->
      <GridLayout
        v-model:layout="layout"
        :col-num="12"
        :row-height="30"
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
            :data-id="item.i"
            class="h-full flex flex-col justify-between relative bg-slate-800 rounded-2xl rounded-br-md p-3 shadow hover:shadow-lg transition widget-item"
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
                class="bg-slate-900 hover:bg-red-700 text-slate-800 hover:text-white w-8 h-8 flex items-center justify-center rounded-full shadow transition text-xl"
              >
                <XCircleIcon class="w-9 h-9" />
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
              <button
                v-if="!item.type"
                @click="selectNoteWidget(item)"
                class="pointer-events-auto bg-yellow-500 hover:bg-yellow-600 text-white px-3 py-1.5 rounded text-xs transition"
              >
                Note
              </button>
              <button
                v-if="!item.type"
                @click="selectLabelWidget(item)"
                class="pointer-events-auto bg-yellow-700 hover:bg-yellow-800 text-white px-3 py-1.5 rounded text-xs transition"
              >
                Label
              </button>
              <button
                v-if="!item.type"
                @click="selectToDoWidget(item)"
                class="pointer-events-auto bg-green-400 hover:bg-green-500 text-white px-3 py-1.5 rounded text-xs transition"
              >
                ToDo
              </button>
              <button
                v-if="!item.type"
                @click="selectSportsWidget(item)"
                class="pointer-events-auto bg-red-500 hover:bg-red-600 text-white px-3 py-1.5 rounded text-xs transition"
                >
                Sports
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
import NoteWidget from './widgets/NoteWidget.vue'
import LabelWidget from './widgets/LabelWidget.vue'
import ToDoWidget from './widgets/ToDoWidget.vue'
import SportsWidget from './widgets/SportsWidget.vue'

const layout = ref([
  { i: 'a', x: 0, y: 0, w: 4, h: 3, type: 'ClockWidget' },
  { i: 'b', x: 4, y: 0, w: 4, h: 3, type: 'NoteWidget' },
  { i: 'c', x: 8, y: 0, w: 4, h: 3, type: 'LabelWidget' },
  { i: 'd', x: 0, y: 3, w: 4, h: 3, type: "ToDoWidget" }
])

const widgetTypes = ['ClockWidget', "NoteWidget", "LabelWidget", "ToDoWidget", "SportsWidget"]

function getComponent(type) {
  const components = { ClockWidget, NoteWidget, LabelWidget, ToDoWidget, SportsWidget }
  return components[type] || null
}

function selectClockWidget(item) {
  item.type = 'ClockWidget'
}

function selectNoteWidget(item) {
  item.type = 'NoteWidget'
}

function selectLabelWidget(item) {
  item.type = 'LabelWidget'
}

function selectToDoWidget(item) {
  item.type = 'ToDoWidget'
}

function selectSportsWidget(item) {
  item.type = 'SportsWidget'
}

function removeWidget(id) {
  const el = document.querySelector(`[data-id="${id}"]`);
  if (el) {
    el.style.setProperty("--tx", "30px");
    el.style.setProperty("--ty", "-30px");

    // Add the leave animation class manually
    el.classList.add("suck-leave");

    // Wait for the animation to finish before removing
    setTimeout(() => {
      layout.value = layout.value.filter(item => item.i !== id);
    }, 600); // same as animation duration
  }
}

function addWidget() {
  const id = Date.now().toString()
  const cols = 12
  const widgetW = 4
  const widgetH = 3

  // Make a 2D array to track occupied cells
  const occupied = {}
  layout.value.forEach(item => {
    for (let x = item.x; x < item.x + item.w; x++) {
      for (let y = item.y; y < item.y + item.h; y++) {
        occupied[`${x},${y}`] = true
      }
    }
  })

  // Find the first free position
  let found = false
  let newX = 0
  let newY = 0

  for (let y = 0; !found; y++) {
    for (let x = 0; x <= cols - widgetW; x++) {
      let spaceFree = true
      for (let dx = 0; dx < widgetW; dx++) {
        for (let dy = 0; dy < widgetH; dy++) {
          if (occupied[`${x + dx},${y + dy}`]) {
            spaceFree = false
            break
          }
        }
        if (!spaceFree) break
      }
      if (spaceFree) {
        newX = x
        newY = y
        found = true
        break
      }
    }
  }

  layout.value.push({ i: id, x: newX, y: newY, w: widgetW, h: widgetH, type: null })
}
</script>
<style scoped>
.suck-leave {
  animation: suckToX 0.35s cubic-bezier(0.55, 0, 0.55, 1) forwards;
  transform-origin: var(--origin-x, 100%) var(--origin-y, 0%);
  z-index: 1000;
}

@keyframes suckToX {
  0% {
    transform: scale(1) translate(0, 0);
    opacity: 1;
  }
 
  100% {
    transform: scale(0);
    opacity: 1;
  }

  
}
</style>





