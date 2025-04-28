<template>
  <div class="p-8">
    <h1 class="text-3xl font-bold mb-8">VueUse 功能示範</h1>

    <!-- 滑鼠追蹤示範 -->
    <div class="mb-8 p-4 border rounded-lg">
      <h2 class="text-xl font-semibold mb-4">滑鼠位置追蹤</h2>
      <div class="h-40 bg-gray-100 rounded relative" ref="mouseArea">
        <div class="w-4 h-4 bg-blue-500 rounded-full absolute transform -translate-x-1/2 -translate-y-1/2"
          :style="{ left: `${x - 150}px`, top: `${y - 230}px` }"></div>
        <p class="mt-2">X: {{ x }}, Y: {{ y }}</p>
      </div>
    </div>

    <!-- 本地存儲示範 -->
    <div class="mb-8 p-4 border rounded-lg">
      <h2 class="text-xl font-semibold mb-4">本地存儲</h2>
      <input v-model="name" class="border p-2 rounded" placeholder="輸入名字">
      <p class="my-2">儲存的名字: {{ name }}</p>

      <h3 class="text-lg font-semibold mt-4">使用者資訊</h3>
      <input v-model="user.name" class="border p-2 rounded" placeholder="輸入使用者名稱">
      <input v-model="user.age" type="number" class="border p-2 rounded" placeholder="輸入年齡">
      <p class="my-2">儲存的使用者: {{ user.name }}, {{ user.age }}</p>
    </div>

    <!-- 視窗大小示範 -->
    <div class="mb-8 p-4 border rounded-lg">
      <h2 class="text-xl font-semibold mb-4">視窗大小</h2>
      <p>寬度: {{ width }}px</p>
      <p>高度: {{ height }}px</p>
    </div>

    <!-- 防抖動示範 -->
    <div class="mb-8 p-4 border rounded-lg">
      <h2 class="text-xl font-semibold mb-4">防抖動搜尋</h2>
      <input v-model="searchQuery" class="border p-2 rounded" placeholder="輸入搜尋關鍵字">
      <p class="mt-2">搜尋結果: {{ debouncedSearch }}</p>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { useMouse, useStorage, useWindowSize, useDebounce } from '@vueuse/core'

// 滑鼠追蹤
const mouseArea = ref<HTMLElement | null>(null)
const { x, y } = useMouse({ target: mouseArea })

// 本地存儲
const name = useStorage<string>('name', '')

// 若 storage 已存在會自動讀取，否則會使用預設值
const user = useStorage<{ name: string; age: number }>('user', {
  name: 'John Doe',
  age: 30,
})

// 視窗大小
const { width, height } = useWindowSize()

// 防抖動搜尋
const searchQuery = ref('')
const debouncedSearch = useDebounce(searchQuery, 500)
</script> 