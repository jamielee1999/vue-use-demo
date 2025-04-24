<template>
  <div class="p-8">
    <h1 class="text-3xl font-bold mb-8">VueUse AsyncState 示範</h1>

    <!-- 基本使用示範 -->
    <div class="mb-8 p-4 border rounded-lg">
      <h2 class="text-xl font-semibold mb-4">基本使用</h2>
      <div class="space-y-4">
        <button @click="() => execute()" class="px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600"
          :disabled="isLoading">
          {{ isLoading ? '載入中...' : '獲取數據' }}
        </button>

        <div v-if="error" class="text-red-500">
          錯誤: {{ error }}
        </div>

        <div v-if="data" class="p-4 bg-gray-100 rounded">
          <pre>{{ JSON.stringify(data, null, 2) }}</pre>
        </div>
      </div>
    </div>

    <!-- 自動執行示範 -->
    <div class="mb-8 p-4 border rounded-lg">
      <h2 class="text-xl font-semibold mb-4">自動執行</h2>
      <div class="space-y-4">
        <div v-if="autoData" class="p-4 bg-gray-100 rounded">
          <pre>{{ JSON.stringify(autoData, null, 2) }}</pre>
        </div>
      </div>
    </div>

    <!-- 補充說明 -->
    <div class="mb-8 p-4 border rounded-lg">
      <h2 class="text-xl font-semibold mb-4">補充說明：shallowRef vs ref</h2>
      <div class="space-y-4">
        <p class="text-gray-700">
          VueUse 核心開發團隊在許多地方使用 <code class="bg-gray-100 px-1 rounded">shallowRef</code> 而不是 <code class="bg-gray-100 px-1 rounded">ref</code>，特別是用於簡單的布林值（如 <code class="bg-gray-100 px-1 rounded">isLoading</code>）。
        </p>
        <div class="bg-gray-50 p-4 rounded">
          <h3 class="font-semibold mb-2">主要差異：</h3>
          <ul class="list-disc list-inside space-y-2">
            <li><code class="bg-gray-100 px-1 rounded">ref</code> 會對值進行深層響應式轉換</li>
            <li><code class="bg-gray-100 px-1 rounded">shallowRef</code> 只會對值進行淺層響應式轉換</li>
            <li>對於簡單類型（如布林值、數字、字符串），兩者性能差異不大</li>
            <li>對於複雜對象，<code class="bg-gray-100 px-1 rounded">shallowRef</code> 性能更好</li>
          </ul>
        </div>
        <p class="text-gray-700">
          在 VueUse 中，像 <code class="bg-gray-100 px-1 rounded">isLoading</code> 這樣的布林值使用 <code class="bg-gray-100 px-1 rounded">shallowRef</code> 是為了優化性能，因為這些值不需要深層響應式轉換。
        </p>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { useAsyncState } from '@vueuse/core'

interface DataType {
  id: number
  title: string
  timestamp: string
}

// 模擬 API 請求
const fetchData = async (): Promise<DataType> => {
  await new Promise(resolve => setTimeout(resolve, 1000))
  return {
    id: 1,
    title: '測試數據',
    timestamp: new Date().toISOString()
  }
}

// 基本使用
const { state: data, isLoading, error, execute } = useAsyncState<DataType | null>(
  fetchData,
  {
    id: 0,
    title: '預設值',
    timestamp: '???',
  },
  {
    immediate: false,
    onSuccess: (data) => {
      console.log('數據獲取成功:', data)
    },
  }
)

// 自動執行
const { state: autoData } = useAsyncState<DataType | null>(
  async () => {
    await new Promise(resolve => setTimeout(resolve, 500))
    return {
      id: 2,
      title: '自動載入的數據',
      timestamp: new Date().toISOString()
    }
  },
  null,
  { immediate: true }
)
</script>