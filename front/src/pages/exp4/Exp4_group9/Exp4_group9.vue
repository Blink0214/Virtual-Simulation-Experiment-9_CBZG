<template>
  <a-table bordered :data-source="dataSource" :columns="columns" :pagination="false">
    <template #bodyCell="{ column, text, record }">
      <template v-if="['depreciation'].includes(column.dataIndex)">
        <div class="editable-cell">
          <div v-if="editableData[record.key]" class="editable-cell-input-wrapper">
            <!-- <a-input v-model:value="editableData[record.key].depreciation" @pressEnter="save(record.key)" /> -->
            <a-input v-model:value="editableData[record.key].depreciation"/>
          </div>
          <div v-else class="editable-cell-text-wrapper">
            {{ text || 0 }}
          </div>
        </div>
      </template>
      <template v-if="['usefulLife'].includes(column.dataIndex)">
        <div class="editable-cell">
          <div v-if="editableData[record.key]" class="editable-cell-input-wrapper">
            <a-input v-model:value="editableData[record.key].usefulLife"/>
          </div>
          <div v-else class="editable-cell-text-wrapper">
            {{ text || 0 }}
          </div>
        </div>
      </template>
      <template v-if="['maintenanceCosts'].includes(column.dataIndex)">
        <div class="editable-cell">
          <div v-if="editableData[record.key]" class="editable-cell-input-wrapper">
            <a-input v-model:value="editableData[record.key].maintenanceCosts"/>
          </div>
          <div v-else class="editable-cell-text-wrapper">
            {{ text || 0 }}
          </div>
        </div>
      </template>
      <template v-if="['actualUsageTime'].includes(column.dataIndex)">
        <div class="editable-cell">
          <div v-if="editableData[record.key]" class="editable-cell-input-wrapper">
            <a-input v-model:value="editableData[record.key].actualUsageTime"/>
          </div>
          <div v-else class="editable-cell-text-wrapper">
            {{ text || 0 }}
          </div>
        </div>
      </template>
      <template v-if="['rentExpense'].includes(column.dataIndex)">
        <div class="editable-cell">
          <div v-if="editableData[record.key]" class="editable-cell-input-wrapper">
            <a-input v-model:value="editableData[record.key].rentExpense"/>
          </div>
          <div v-else class="editable-cell-text-wrapper">
            {{ text || 0 }}
          </div>
        </div>
      </template>
      <template v-if="['termOfLease'].includes(column.dataIndex)">
        <div class="editable-cell">
          <div v-if="editableData[record.key]" class="editable-cell-input-wrapper">
            <a-input v-model:value="editableData[record.key].termOfLease"/>
          </div>
          <div v-else class="editable-cell-text-wrapper">
            {{ text || 0 }}
          </div>
        </div>
      </template>
      <template v-else-if="column.dataIndex === 'operation'">
        <div v-if="editableData[record.key]" class="editable-cell-input-wrapper">
          <check-outlined class="editable-cell-icon-check" @click="save(record.key)" />
        </div>
        <div v-else class="editable-cell-text-wrapper">
          <edit-outlined class="editable-cell-icon" @click="edit(record.key)" />
        </div>
      </template>
    </template>
  </a-table>

</template>
<script lang="ts">
import { computed, defineComponent, reactive, ref } from 'vue';
import type { Ref, UnwrapRef } from 'vue';
import { CheckOutlined, EditOutlined } from '@ant-design/icons-vue';
import { cloneDeep } from 'lodash-es';

interface DataItem {
  key: string;
  depreciation: number,
  usefulLife: number,
  maintenanceCosts: number,
  actualUsageTime: number,
  rentExpense: number,
  termOfLease: number,
}

export default defineComponent({
  components: {
    CheckOutlined,
    EditOutlined,
  },
  setup() {
    const columns = [
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
          width: 200,
        },
        {
          title: '租赁期限',
          dataIndex: 'termOfLease',
          key: 'termOfLease',
          width: 200,
        },
      ],
    },
    {
      title: '操作',
      dataIndex: 'operation',
    },
    ];
    const dataSource: Ref<DataItem[]> = ref([
      {
        key: '0',
        depreciation: 0,
        usefulLife: 0,
        maintenanceCosts: 0,
        actualUsageTime: 0,
        rentExpense: 0,
        termOfLease: 0,
      },
    ]);
    var OT = 0;
    var RT = 0;
    const editableData: UnwrapRef<Record<string, DataItem>> = reactive({});

    const edit = (key: string) => {
      editableData[key] = cloneDeep(dataSource.value.filter(item => key === item.key)[0]);
    };
    const save = (key: string) => {
      Object.assign(dataSource.value.filter(item => key === item.key)[0], editableData[key]);
      var depreciation=dataSource.value.filter(item => key === item.key)[0].depreciation
      var usefulLife=dataSource.value.filter(item => key === item.key)[0].usefulLife
      var maintenanceCosts=dataSource.value.filter(item => key === item.key)[0].maintenanceCosts
      var actualUsageTime=dataSource.value.filter(item => key === item.key)[0].actualUsageTime
      OT=(Number((Number( depreciation / usefulLife) + Number(maintenanceCosts))) / 200) * actualUsageTime;
      RT=editableData[key].rentExpense*editableData[key].termOfLease;
      delete editableData[key];
    };

    return {
      columns,
      dataSource,
      editableData,
      OT,
      RT,
      edit,
      save,
    };
  },
});
</script>
<style lang="less">
.editable-cell {
  position: relative;
  .editable-cell-input-wrapper,
  .editable-cell-text-wrapper {
    padding-right: 24px;
  }

  .editable-cell-text-wrapper {
    padding: 5px 24px 5px 5px;
  }

  .editable-cell-icon,
  .editable-cell-icon-check {
    position: absolute;
    right: 0;
    width: 20px;
    cursor: pointer;
  }

  .editable-cell-icon {
    margin-top: 4px;
  }

  .editable-cell-icon-check {
    line-height: 34px;
  }

  .editable-cell-icon:hover,
  .editable-cell-icon-check:hover {
    color: #108ee9;
  }
}
.editable-cell:hover .editable-cell-icon {
  display: inline-block;
}
</style>