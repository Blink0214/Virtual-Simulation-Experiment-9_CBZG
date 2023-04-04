<template>
  <div style="width:70%;text-align: center;margin: 0 auto;">
    <a-row style="border: 1px solid lightgray;height:50px;line-height: 50px;" justify="space-around" align="middle">
      <a-col :span="19" style="border-right: 1px solid lightgray;margin-right: -18px;font-size: large;font-weight: 600;">软件测试工具成本</a-col>
      <a-col :span="3" style="font-size: large;font-weight: 600;">总计</a-col>
    </a-row>
    <a-row style="border: 1px solid lightgray;border-top:none;" justify="space-around" align="middle">
      <a-col :span="1">OT</a-col>
      <a-col :span="18">
        <a-descriptions :model="form" bordered>
          <a-descriptions-item label="固定资产应计折旧额" :span="3" style="width:72%;">
            <a-input-number v-model:value="form.depreciation" :min="0" />&emsp;&emsp;&nbsp;元
          </a-descriptions-item>
          <a-descriptions-item label="固定资产预计使用年限" :span="3">
            <a-input-number v-model:value="form.usefulLife" :min="0" />&emsp;&emsp;&nbsp;年
          </a-descriptions-item>
          <a-descriptions-item label="维护费用" :span="3">
            <a-input-number v-model:value="form.maintenanceCosts" :min="0" />&emsp;&emsp;&nbsp;元
          </a-descriptions-item>
          <a-descriptions-item label="工具实际使用时间（工具使用每年按200个工作日算）" :span="3">
            <a-input-number v-model:value="form.actualUsageTime" :min="0" />&emsp;&emsp;&nbsp;天
          </a-descriptions-item>
        </a-descriptions>
      </a-col>
      <a-col :span="3">
        <a-statistic :value=OT />
      </a-col>元
    </a-row>
    <a-row style="border: 1px solid lightgray;border-top:none;" justify="space-around" align="middle">
      <a-col :span="1">RT</a-col>
      <a-col :span="18">
        <a-descriptions :model="form" bordered>
          <a-descriptions-item label="租赁费用" :span="3" style="width:72%;">
            <a-input-number v-model:value="form.rentExpense" :min="0" />&emsp;元/月
          </a-descriptions-item>
          <a-descriptions-item label="租赁期限" :span="3">
            <a-input-number v-model:value="form.termOfLease" :min="0" />&emsp;&emsp;&nbsp;月
          </a-descriptions-item>
        </a-descriptions>
      </a-col>
      <a-col :span="3">
        <a-statistic :value=RT />
      </a-col>元
    </a-row>
    <a-row style="border: 1px solid lightgray;border-top:none;height:50px;" justify="space-around" align="middle">
      IC = OT + RT = {{ IC }}
    </a-row>
  </div>

</template>

<script lang="ts">
import { defineComponent, reactive, computed } from 'vue';

export default defineComponent({
  setup() {
    const form = reactive({
      depreciation: 1000000,
      usefulLife: 5,
      maintenanceCosts: 200000,
      actualUsageTime: 5,
      rentExpense: 0,
      termOfLease: 0,
    });

    const OT = computed(() => {
      return (
        Math.round(((form.depreciation / form.usefulLife + form.maintenanceCosts) / 200) *
        form.actualUsageTime)
      );
    });

    const RT = computed(() => {
      return form.rentExpense * form.termOfLease;
    });

    const IC = computed(() => {
      return OT.value + RT.value;
    });

    const calculate = (event: Event) => {
      event.preventDefault();
      console.log('OT:', OT.value);
      console.log('RT:', RT.value);
      console.log('IC:', IC.value);
    };

    return {
      form,
      OT,
      RT,
      IC,
      calculate,
    };
  },
});
</script>
<style>

</style>