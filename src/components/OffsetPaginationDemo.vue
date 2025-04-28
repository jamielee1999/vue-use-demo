<template>
  <div class="p-4">
    <div class="mb-4">
      <input type="text" v-model="search" placeholder="搜尋商品..."
        class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500" />
    </div>

    <!-- 載入中狀態 -->
    <div v-if="isLoading" class="flex justify-center items-center py-8">
      <div class="animate-spin rounded-full h-8 w-8 border-b-2 border-blue-500"></div>
    </div>

    <!-- 錯誤訊息 -->
    <div v-else-if="error" class="bg-red-50 border border-red-200 text-red-700 px-4 py-3 rounded-lg">
      <p>載入失敗：{{ error }}</p>
    </div>

    <!-- 無搜尋結果 -->
    <div v-else-if="filteredData.length === 0" class="text-center py-8 text-gray-500">
      沒有找到符合的商品
    </div>

    <!-- 資料表格 -->
    <div v-else class="overflow-x-auto">
      <table class="min-w-full bg-white border border-gray-200 rounded-lg">
        <thead class="bg-gray-50">
          <tr>
            <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">ID</th>
            <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">商品名稱</th>
            <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">價格</th>
            <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">分類</th>
          </tr>
        </thead>
        <tbody class="divide-y divide-gray-200">
          <tr v-for="item in pageData" :key="item.id" class="hover:bg-gray-50">
            <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">{{ item.id }}</td>
            <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-900">{{ item.title }}</td>
            <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">${{ item.price }}</td>
            <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">{{ item.category }}</td>
          </tr>
        </tbody>
      </table>
    </div>

    <!-- 分頁控制 -->
    <div v-if="!isLoading && !error && filteredData.length > 0" class="mt-4 flex items-center justify-center space-x-2">
      <button :disabled="isFirstPage" @click="prev"
        class="px-4 py-2 text-sm font-medium text-gray-700 bg-white border border-gray-300 rounded-md hover:bg-gray-50 disabled:opacity-50 disabled:cursor-not-allowed">
        上一頁
      </button>
      <button v-for="item in pageCount" :key="item" :disabled="currentPage === item" @click="currentPage = item"
        class="px-4 py-2 text-sm font-medium text-gray-700 bg-white border border-gray-300 rounded-md hover:bg-gray-50 disabled:opacity-50 disabled:cursor-not-allowed"
        :class="currentPage === item ? 'bg-blue-50 text-blue-600 border-blue-500' : ''">
        {{ item }}
      </button>
      <button :disabled="isLastPage" @click="next"
        class="px-4 py-2 text-sm font-medium text-gray-700 bg-white border border-gray-300 rounded-md hover:bg-gray-50 disabled:opacity-50 disabled:cursor-not-allowed">
        下一頁
      </button>
    </div>
  </div>
</template>

<script setup>
import { computed, ref } from 'vue';
import { useOffsetPagination, useAsyncState } from '@vueuse/core';

const search = ref('');
const pageSize = 10;

const fetchProductList = () => {
  return fetch('https://fakestoreapi.com/products')
    .then((res) => res.json())
    .then((data) => {
      return data;
    });
}

const { state, isLoading, error } = useAsyncState(
  fetchProductList,
  [],
  { immediate: true }
)

// filter search
const filteredData = computed(() => {
  if (!state.value) return [];
  return state.value.filter((item) =>
    item.title.toLowerCase().includes(search.value.toLowerCase())
  );
});

// 前端分頁
const pageData = computed(() => {
  const startIndex = (currentPage.value - 1) * pageSize;
  const endIndex = startIndex + pageSize;
  return filteredData.value.slice(startIndex, endIndex);
});

// 分頁控制 - 參數變化時會重新計算
const { currentPage, pageCount, isFirstPage, isLastPage, prev, next } = useOffsetPagination({
  pageSize,
  page: 1,
  total: () => filteredData.value.length,  // 使用函數才能夠維持響應式
});

</script>