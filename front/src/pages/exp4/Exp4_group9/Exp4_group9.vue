<template>
  <div style="width:80%;text-align: center;margin: 0 auto" >
    <a-row style="border: 1px solid lightgray;height:50px; border-radius:10px 10px 0px 0px;line-height: 50px;" justify="space-around" align="middle">
      <a-col :span="24" style="font-weight: 600;">步骤四：软件测试工具成本</a-col>
    </a-row>
    <a-row style="border: 1px solid lightgray;border-top:none;" justify="space-around" align="middle">
      <a-col :span="3">自有工具成本(OT)</a-col>
      <a-col :span="15">
        <a-descriptions :model="form" bordered>
          <a-descriptions-item label="固定资产应计折旧额" :span="3" style="width:65%;">
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
      <a-col :span="4">
        <a-statistic title="总计" :value=OT :value-style="{ fontSize:'20px'}" :precision="2" suffix="元">
        </a-statistic>
      </a-col>
    </a-row>
    <a-row style="border: 1px solid lightgray;border-top:none;" justify="space-around" align="middle">
      <a-col :span="3">租借工具成本(RT)</a-col>
      <a-col :span="15">
        <a-descriptions :model="form" bordered>
          <a-descriptions-item label="租赁费用" :span="3" style="width:65%;">
            <a-input-number v-model:value="form.rentExpense" :min="0" />&emsp;元/月
          </a-descriptions-item>
          <a-descriptions-item label="租赁期限" :span="3">
            <a-input-number v-model:value="form.termOfLease" :min="0" />&emsp;&emsp;&nbsp;月
          </a-descriptions-item>
        </a-descriptions>
      </a-col>
      <a-col :span="4">
        <a-statistic title="总计" :value=RT :value-style="{ fontSize:'20px'}" :precision="2" suffix="元">
        </a-statistic>
        
      </a-col>
    </a-row>

    <a-row style="border: 1px solid lightgray;border-top:none;height:100px; border-radius:0px 0px 10px 10px;" justify="space-around" align="middle">
      <a-statistic title="软件测试工具成本(IC)=OT+RT" :precision="2"  :value-style="{ fontSize:'20px'}" :value=IC  suffix="元">
        </a-statistic>
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