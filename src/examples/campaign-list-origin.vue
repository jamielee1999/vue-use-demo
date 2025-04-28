<template>
  <!-- 要多包一層才能吃到 scss 的部分設定，原因待釐清 -->
  <div>
    <el-table :data="pageData" class="onead-table" style="width: 100%" size="large" :empty-text="word.noRecord" border>
      <!-- ... -->
    </el-table>
    <el-pagination layout="prev, pager, next" class="mt-2 justify-center" :total="filterData.length"
      :page-size="pageSize" :current-page="currentPage" @current-change="handleCurrentChange" background>
    </el-pagination>
  </div>
</template>

<script setup>
import { computed, ref, onMounted } from 'vue';
import { fetchCampaignList } from '@/services/superdsp_api/campaign';
const insertionOrderId = ref('');
const tableData = ref([]);
const search = ref('');

const filterData = computed(() => {
  const resultData = tableData.value
    .filter(data => data.name.toLowerCase().includes(search.value.toLowerCase()))
  return resultData;
});

const currentPage = ref(1);
const pageSize = ref(10);

const pageData = computed(() =>
  filterData.value.slice((currentPage.value - 1) * pageSize.value, currentPage.value * pageSize.value),
);

const handleCurrentChange = (pageNum) => {
  currentPage.value = pageNum;
};


const loadCampaignList = () => {
  fetchCampaignList(parseInt(insertionOrderId.value, 10))
    .then(res => {
      tableData.value = res.data.data;
      tableData.value.sort((a, b) => b.id - a.id);
    })
};


onMounted(() => {
  insertionOrderId.value = typeof route.params.oId === 'object' ? route.params.oId[0] : route.params.oId;
  loadCampaignList();
});
</script>
