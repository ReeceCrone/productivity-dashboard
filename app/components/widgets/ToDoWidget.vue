<template>
  <div class="bg-slate-900 text-white rounded-lg h-full flex flex-col p-3 font-mono">
    <!-- Input row -->
    <div class="flex mb-3 gap-2">
      <input
        v-model="newItem"
        placeholder="Add a new task..."
        class="flex-1 bg-slate-800 rounded p-2 focus:outline-none text-lg"
        @keydown.enter.prevent="addTodo"
      />
      <button
        @click="addTodo"
        class="bg-green-500 hover:bg-green-600 text-green-700 hover:text-white w-10 h-10 rounded-full transition"
      >
        <plus-circle-icon class="w-10 h-10" />
      </button>
    </div>

    <!-- To-do list -->
    <ul class="space-y-2 overflow-y-auto flex-1">
      <li
        v-for="(item, index) in todoItems"
        :key="index"
        class="flex items-center justify-between bg-slate-800 rounded p-2 transition hover:bg-slate-700"
      >
        <div class="flex items-center gap-2">
          <input
            type="checkbox"
            v-model="item.done"
            class="w-5 h-5 accent-blue-500 cursor-pointer"
          />
          <span
            :class="[
              'text-lg transition-all duration-200 select-none',
              item.done ? 'line-through text-slate-400' : 'text-white'
            ]"
          >
            {{ item.text }}
          </span>
        </div>
        <button
          @click="removeTodo(index)"
          class="bg-slate-900 hover:bg-red-500 text-slate-800 hover:text-white w-10 h-10 rounded-full transition"
        >
          <XCircleIcon class="w-10 h-10" />
        </button>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import {XCircleIcon, PlusCircleIcon} from '@heroicons/vue/24/solid'

const newItem = ref('')
const todoItems = ref([

])

function addTodo() {
  const text = newItem.value.trim()
  if (text) {
    todoItems.value.push({ text, done: false })
    newItem.value = ''
  }
}

function removeTodo(index) {
  todoItems.value.splice(index, 1)
}
</script>

<style scoped>
ul::-webkit-scrollbar {
  width: 6px;
}
ul::-webkit-scrollbar-thumb {
  background-color: rgba(100, 116, 139, 0.5); /* slate-500 */
  border-radius: 10px;
}
ul::-webkit-scrollbar-thumb:hover {
  background-color: rgba(148, 163, 184, 0.8); /* slate-400 */
}
</style>