<script setup lang="ts">
import { ref } from 'vue'
import VueUseDemo from './components/VueUseDemo.vue'
import AsyncStateDemo from './components/AsyncStateDemo.vue'
import OffsetPaginationDemo from './components/OffsetPaginationDemo.vue'

type PageType = 'vueuse' | 'async' | 'pagination'
const currentPage = ref<PageType>('vueuse')
</script>

<template>
  <div class="min-h-screen bg-gray-50">
    <nav class="bg-white shadow-sm">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <div class="flex justify-between h-16">
          <div class="flex">
            <button @click="currentPage = 'vueuse'" class="px-4 py-2 text-sm font-medium"
              :class="currentPage === 'vueuse' ? 'text-blue-600 border-b-2 border-blue-600' : 'text-gray-500 hover:text-gray-700'">
              VueUse 示範
            </button>
            <button @click="currentPage = 'async'" class="px-4 py-2 text-sm font-medium"
              :class="currentPage === 'async' ? 'text-blue-600 border-b-2 border-blue-600' : 'text-gray-500 hover:text-gray-700'">
              AsyncState 示範
            </button>
            <button @click="currentPage = 'pagination'" class="px-4 py-2 text-sm font-medium"
              :class="currentPage === 'pagination' ? 'text-blue-600 border-b-2 border-blue-600' : 'text-gray-500 hover:text-gray-700'">
              分頁示範
            </button>
          </div>
        </div>
      </div>
    </nav>

    <main class="max-w-7xl mx-auto py-6 sm:px-6 lg:px-8">
      <VueUseDemo v-if="currentPage === 'vueuse'" />
      <AsyncStateDemo v-if="currentPage === 'async'" />
      <OffsetPaginationDemo v-if="currentPage === 'pagination'" />
    </main>
  </div>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
