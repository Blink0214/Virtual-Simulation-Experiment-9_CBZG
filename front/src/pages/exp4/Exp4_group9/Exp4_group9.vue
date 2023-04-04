<template>
  <a-table
    :columns="columns"
    :data-source="data"
    :pagination="false"
    bordered
    size="large"
  />
</template>
<script lang="ts">
import type { TableColumnsType } from 'ant-design-vue';
import { defineComponent } from 'vue';
const columns: TableColumnsType = [
  {
    title: 'OT',
    children: [
      {
        title: '固定资产应计折旧额',
        dataIndex: 'depreciation',
        key: 'depreciation',
        width: 200,
      },
      {
        title: '固定资产预计使用年限',
        dataIndex: 'usefulLife',
        key: 'usefulLife',
        width: 200,
      },
      {
        title: '维护费用',
        dataIndex: 'maintenanceCosts',
        key: 'maintenanceCosts',
        width: 200,
      },
      {
        title: '工具实际使用时间（工具使用每年按200个工作日算）',
        dataIndex: 'actualUsageTime',
        key: 'actualUsageTime',
        width: 200,
      },
    ],
  },
  {
    title: 'RT',
    children: [
      {
        title: '租赁费用',
        dataIndex: 'rentExpense',
        key: 'rentExpense',
      },
      {
        title: '租赁期限',
        dataIndex: 'termOfLease',
        key: 'termOfLease',
      },
    ],
  },
];
const data = ([
  {
    key: 1,
    depreciation: 10,
    usefulLife: 20,
    maintenanceCosts: 30,
    actualUsageTime: 40,
    rentExpense: 50,
    termOfLease: 60,
  },
]);
export default defineComponent({
  setup() {
    return {
      data,
      columns,
    };
  },
});
</script>