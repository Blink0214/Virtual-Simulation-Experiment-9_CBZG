<template>
  <a-layout style="background-color:white;">
    <a-layout-header class="my-layout-header">软件测试成本度量实验</a-layout-header>
    <div class="GBlink" @click="jumpToGB">参考国家标准</div>
    <a-layout-content class="my-layout-content">

      <!--第一步：计算软件测试的人工成本工作量-->
      <div>
        <a-typography-title :level="5">步骤一：软件测试的人工成本工作量计算</a-typography-title>
        <a-row class="son-line" style="display: table-cell;vertical-align: middle;">
          <a-typography-text>（1）计算公式：UW = TW + SR + DR </a-typography-text>
            <a-popover title="说明：">
              <template #content>
                <p>UW —— 未调整的软件测试人工工作量【人日】</p>
                <p>TW —— 软件测试工作量【人日】</p>
                <p>SR —— 产品说明评审工作量【人日】</p>
                <p>DR —— 用户文档集评审工作量【人日】</p>
              </template>
              <question-circle-outlined />
            </a-popover>
        </a-row>
        <a-row class="son-line">（2）计算过程：</a-row>
        <a-row class="son-line" style="padding-left:10px ;">（a）软件测试工作量TW：
          <a-input-number id="inputTW" v-model:value="TW" size="small" :min="0" :max="100000" />（人日）
        </a-row>
        <a-row class="son-line" style="padding-left:10px ;">
          （b）未调整的软件测试人工工作量：UW = TW + TW * 10% + TW *20% = {{TW}} + {{TW}} * {{SR_weight}} + {{TW}} * {{DR_weight}} = {{UW}}（人日）
        </a-row>
      </div>

      <!--第三步：计算软件测试的人工成本-->
      <div>
        <a-typography-title :level="5">步骤三：软件测试的人工成本计算</a-typography-title>
        <a-row class="son-line" style="display: table-cell;vertical-align: middle;">
          <a-typography-text>（1）计算公式：LC = UW * DF * S </a-typography-text>
            <a-popover title="说明：">
              <template #content>
                <p>LC —— 测试人工成本【元】</p>
                <p>UW —— 未调整的软件测试人工工作量【人日】</p>
                <p>DF —— 软件测试成本调整因子</p>
                <p>S —— 工作量单价，依据当地平均收入水平调整【元/人日】</p>
              </template>
              <question-circle-outlined />
            </a-popover>
        </a-row>
        <a-row class="son-line">（2）计算过程：</a-row>
        <a-row class="son-line" style="padding-left:10px ;">（a）工作量单价S（依据当地平均收入水平调整）：
          <a-input-number id="inputS" v-model:value="S" size="small" :min="0" :max="100000" />（元/人日）
        </a-row>
        <a-row class="son-line" style="padding-left:10px ;">
          （b）测试人工成本：LC = {{UW}} * {{DF}} * {{S}} = {{LC}}（元）
        </a-row>
      </div>

      <!--第四步：计算测试工具成本-->
      <a-typography-title :level="5">步骤四：软件测试的工具成本计算</a-typography-title>
      <div style="width:80%;text-align: center;margin: 0 auto" >
        <a-row style="border: 1px solid lightgray;height:50px; border-radius:10px 10px 0px 0px;line-height: 50px;" justify="space-around" align="middle">
          <a-col :span="24" style="font-weight: 600;">步骤四：软件测试工具成本</a-col>
        </a-row>
        <a-row style="border: 1px solid lightgray;border-top:none;padding-top: 10px;" justify="space-around" align="middle">
          <a-col :span="3">
            自有工具成本(OT)
          </a-col>
          <a-col :span="16">
            <a-row v-for="(item,index) in OTForm" style="border: 1px solid lightgray;" justify="space-around" align="middle">
              <a-col :span="3"> 
                <a-row justify="space-around" align="middle">
                  自有工具{{ index+1 }}
                </a-row>
                <a-row justify="space-around" align="middle">
                  <a-button shape="circle" class="editable-add-btn" style="margin-top:5px" size=small danger @click="handleDeleteOT(index)"><minus-outlined /></a-button>
                </a-row>
              </a-col>
              <a-col :span="21">
                <a-descriptions :model="item" bordered size=small>
                  <a-descriptions-item label="固定资产应计折旧额" :span="3" style="width:65%;">
                    <a-input-number v-model:value="item.depreciation" :min="0" />&emsp;&emsp;&nbsp;元
                  </a-descriptions-item>
                  <a-descriptions-item label="固定资产预计使用年限" :span="3">
                    <a-input-number v-model:value="item.usefulLife" :min="0" />&emsp;&emsp;&nbsp;年
                  </a-descriptions-item>
                  <a-descriptions-item label="维护费用" :span="3">
                    <a-input-number v-model:value="item.maintenanceCosts" :min="0" />&emsp;&emsp;&nbsp;元
                  </a-descriptions-item>
                  <a-descriptions-item label="工具实际使用时间（工具使用每年按200个工作日算）" :span="3">
                    <a-input-number v-model:value="item.actualUsageTime" :min="0" />&emsp;&emsp;&nbsp;天
                  </a-descriptions-item>
                </a-descriptions>
              </a-col>
            </a-row>
            <a-row justify="space-around" align="middle">
              <a-button class="editable-add-btn" style="width: 100%;margin: 10px 0px 10px 0px;" @click="handleAddOT" ghost type="primary"><plus-outlined /></a-button>
            </a-row>
          </a-col>
          <a-col :span="3">
            <a-statistic v-if="OT!=-1" title="总计" :value=OT :value-style="{ fontSize:'20px'}" :precision="2" suffix="元"></a-statistic>
            <a-statistic v-if="OT==-1" :value='0' :value-style="{ fontSize:'20px',color:'red'}" :precision="2">
              <template #title>
                <a-tooltip placement="right">
                  <template #title>
                    <span>请检查输入！</span>
                  </template>
                  <question-circle-two-tone/>
                </a-tooltip>
              </template>
            </a-statistic>
          </a-col>
        </a-row>
        <a-row style="border: 1px solid lightgray;border-top:none;padding-top: 10px;" justify="space-around" align="middle">
          <a-col :span="3">
            租借工具成本(RT)
          </a-col>
          <a-col :span="16">
            <a-row v-for="(item,index) in RTForm" style="border: 1px solid lightgray;" justify="space-around" align="middle">
              <a-col :span="3">
                <a-row justify="space-around" align="middle">
                  租借工具{{ index+1 }}
                </a-row>
                <a-row justify="space-around" align="middle">
                  <a-button shape="circle" class="editable-add-btn" style="margin-top:5px" size=small danger @click="handleDeleteRT(index)"><minus-outlined /></a-button>
                </a-row>
              </a-col>
              <a-col :span="21">
                <a-descriptions :model="item" bordered size=small>
                  <a-descriptions-item label="租赁费用" :span="3" style="width:65%;">
                    <a-input-number v-model:value="item.rentExpense" :min="0" />&emsp;元/月
                  </a-descriptions-item>
                  <a-descriptions-item label="租赁期限" :span="3">
                    <a-input-number v-model:value="item.termOfLease" :min="0" />&emsp;&emsp;&nbsp;月
                  </a-descriptions-item>
                </a-descriptions>
              </a-col>
            </a-row>
            <a-row justify="space-around" align="middle">
              <a-button class="editable-add-btn" style="width: 100%;margin: 10px 0px 10px 0px;" @click="handleAddRT" ghost type="primary"><plus-outlined /></a-button>
            </a-row>
          </a-col>
          <a-col :span="3">
            <a-statistic title="总计" :value=RT :value-style="{ fontSize:'20px'}" :precision="2" suffix="元"></a-statistic>
          </a-col>
        </a-row>
        <a-row style="border: 1px solid lightgray;border-top:none;height:100px; border-radius:0px 0px 10px 10px;" justify="space-around" align="middle">
          <a-statistic title="软件测试工具成本(IC)=OT+RT" :precision="2"  :value-style="{ fontSize:'20px'}" :value=IC  suffix="元"></a-statistic>
        </a-row>
      </div>

      <!--第六步：计算软件测试成本-->
      <div>
        <a-typography-title :level="5">步骤六：软件测试成本计算</a-typography-title>
        <a-row class="son-line" style="display: table-cell;vertical-align: middle;">
          <a-typography-text>（1）计算公式：STC = DC + IDC </a-typography-text>
            <a-popover title="说明：">
              <template #content>
                <p>STC —— 软件测试成本【元】</p>
                <p>DC —— 直接成本【元】</p>
                <p>IDC —— 间接成本,宜不超过直接成本的20%【元】</p>
              </template>
              <question-circle-outlined />
            </a-popover>
        </a-row>
        <a-row class="son-line">（2）计算过程：</a-row>
        <a-row class="son-line" style="padding-left:10px ;">（a）间接成本 IDC（宜不超过直接成本 DC 的20%）：
            <a-input-number v-model:value="IDC" size="small" :min="0" :max="0.2*DC"/>（元）
        </a-row>
        <a-row class="son-line" style="padding-left:10px;">
          （b）软件测试成本：STC = {{DC}} + {{IDC}} = {{STC}}（元）
        </a-row>
      </div>
    </a-layout-content>
    <!--a-layout-footer class="my-layout-footer">实验结束！</!--a-layout-footer-->
  </a-layout>
<a-back-top/>
</template>

<script lang="ts">
import { defineComponent, reactive, computed, ref} from 'vue';
import type { Ref } from 'vue';

//自有工具数组元素类型
interface OTFormDataType {
  depreciation: number,
  usefulLife: number,
  maintenanceCosts: number,
  actualUsageTime: number,
}

//租借工具数组元素类型
interface RTFormDataType {
  rentExpense: number,
  termOfLease: number,
}

export default defineComponent({
  name: 'Exp4_group9',
  setup() {
    const GBtestlink = "http://c.gb688.cn/bzgk/gb/showGb?type=online&hcno=19B82C39FB3B9DF0A35D28122C07B053";
    const TW = ref<number>(0);//软件测试工作量
    const SR_weight = ref<number>(0.1);//产品说明评审工作量权重
    const DR_weight = ref<number>(0.2);//用户文档集评审工作量权重
    const S = ref<number>(0);//工作量单价
    const DC = ref<number>(0);//直接成本
    const IDC = ref<number>(0);//间接成本

    //未调整的软件测试人工工作量
    const UW = computed(() => {
      var x = Math.round((TW.value + TW.value * SR_weight.value + TW.value * DR_weight.value) * 100) / 100;
      return x;
    });

    //软件测试人工成本调整因子
    const DF = computed(() => {
      //修改这里
      var x = 2;
      return x;
    });

    //软件测试人工成本
    const LC = computed(() => {
      var x = Math.round((UW.value * DF.value * S.value) * 100) / 100;
      return x;
    });

    

    //自有工具数组
    const OTForm: Ref<OTFormDataType[]> = ref([
      {
        depreciation: 0,
        usefulLife: 1,
        maintenanceCosts: 0,
        actualUsageTime: 1,
      },
    ]);

    //租借工具数组
    const RTForm: Ref<RTFormDataType[]> = ref([
      {
        rentExpense: 0,
        termOfLease: 0,
      },
    ]);

    //自有工具成本
    const OT = computed(() => {
      var x=0;
      for(var i = 0; i<OTForm.value.length; i++) { 
        x += Math.round(((OTForm.value[i].depreciation / OTForm.value[i].usefulLife + OTForm.value[i].maintenanceCosts) / 200) *
        OTForm.value[i].actualUsageTime);
      }
      if(!x && x!=0 || x==Infinity){
        return -1
      }
      return x;
    });

    //租借工具成本
    const RT = computed(() => {
      var x=0;
      for(var i = 0; i<RTForm.value.length; i++) { 
        x += RTForm.value[i].rentExpense * RTForm.value[i].termOfLease;
      }
      if(!x && x!=0 || x==Infinity){
        return -1
      }
      return x;
    });

    //测试工具成本
    const IC = computed(() => {
      if(!(OT.value + RT.value)||OT.value<0){
        return 0;
      }
      else{
        return OT.value + RT.value;
      }
    });

    //增加自有工具
    const handleAddOT = () => {
      const newData = {
        depreciation: 0,
        usefulLife: 1,
        maintenanceCosts: 0,
        actualUsageTime: 1,
      };
      OTForm.value.push(newData);
    };

    //删除自有工具
    const handleDeleteOT = (index) => {
      OTForm.value.splice(index,1);
    };

    //增加租借工具
    const handleAddRT = () => {
      const newData = {
        rentExpense: 0,
        termOfLease: 0,
      };
      RTForm.value.push(newData);
    };

    //删除租借工具
    const handleDeleteRT = (index) => {
      RTForm.value.splice(index,1);
    };

    //软件测试成本
    const STC = computed(() => {
      var x= Math.round(DC.value+IDC.value);
      return x;
    });

    return {
      GBtestlink,
      UW,
      TW,
      SR_weight,
      DR_weight,
      DF,
      S,
      LC,
      OTForm,
      RTForm,
      OT,
      RT,
      handleAddOT,
      handleDeleteOT,
      handleAddRT,
      handleDeleteRT,
      IC,
      DC,
      IDC,
      STC,
    };
  },
  computed: {
  },
  methods: {
    jumpToGB() {
      window.open(this.GBtestlink, '_blank');
    },
  }
});
</script>

<style scoped lang="less">
.GBlink{
  margin-left: auto;
  color: cadetblue;
  font-style: italic;
  text-decoration: underline
}
.my-layout-header {
  text-align: center;
  font-size: large;
  font-weight:bold;
}
// .my-layout-content{
// }
.my-layout-footer{
  text-align: center;
  font-size: large;
  background-color: white;
}

.son-line{
  line-height: 30px;
}
</style>
