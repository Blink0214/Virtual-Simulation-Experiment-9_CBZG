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
          （b）未调整的软件测试人工工作量：UW = TW + TW * 10% + TW *20% = {{ TW }} + {{ TW }} * {{ SR_weight }} + {{ TW }} * {{ DR_weight
          }} =
          {{ UW }}（人日）
        </a-row>
      </div>

      <!--第二步：计算软件测试成本调整因子-->
      <div>
        <a-typography-title :level="5">步骤二：软件测试成本调整因子计算</a-typography-title>
        <a-row class="son-line" style="display: table-cell;vertical-align: middle;">
          <a-typography-text>（1）计算公式：DF = C * I * R * U * X * A * ( 1 + n * Tr ) </a-typography-text>
          <a-popover title="说明：">
            <template #content>
              <p>DF ———软件测试成本调整因子; </p>
              <p>C ———软件复杂性调整因子,取值范围1.0~1.5; </p>
              <p>I ———软件完整性调整因子,取值范围1.0~1.8;</p>
              <p>R ———测试风险调整因子,取值范围1.0~1.5; </p>
              <p>U ———加急测试调整因子,取值范围1.0~3.0; </p>
              <p>X ———现场测试调整因子,取值范围1.0~1.3; </p>
              <p>A ———评测机构资质调整因子,取值范围1.0~1.2; </p>
              <p>Tr ———回归测试调整因子,取值范围0.6~0.8; </p>
              <p>n ———回归测试次数</p>
            </template>
            <question-circle-outlined />
          </a-popover>
        </a-row>

        <a-row class="son-line">（2）计算过程：</a-row>
        <a-row class="son-line" style="padding-left:10px ;">（a）选择调整因子和回归测试次数取值</a-row>
        <a-row class="son-line" style="padding-left:10px ;">（b）根据计算公式得到DF的值</a-row>



        <div style="width:80%;text-align: center;margin: 0 auto">

          <a-row style="border: 1px solid lightgray;height:50px; border-radius:10px 10px 0px 0px;line-height: 50px;"
            justify="space-around" align="middle">
            <a-col :span="24" style="font-weight: 600;">软件成本调整因子</a-col>
          </a-row>

          <a-row style="border: 1px solid lightgray;border-top:none;" justify="space-around" align="middle">
            <a-col :span="4">调整因子</a-col>
            <a-col :span="20">
              <a-descriptions :model="form" bordered>
                <a-descriptions-item label="软件复杂性调整因子 C (1.0~1.5)" :span="3" style="width:65%;">
                  <a-input-number v-model:value="form.valueC" :min="1.0" :max="1.5" :step="0.1" />&emsp;&emsp;&nbsp;
                </a-descriptions-item>
                <a-descriptions-item label="软件完整性调整因子 I (1.0~1.8)" :span="3">
                  <a-input-number v-model:value="form.valueI" :min="1.0" :max="1.8" :step="0.1" />&emsp;&emsp;&nbsp;
                </a-descriptions-item>
                <a-descriptions-item label="测试风险调整因子 R (1.0~1.5)" :span="3">
                  <a-input-number v-model:value="form.valueR" :min="1.0" :max="1.5" :step="0.1" />&emsp;&emsp;&nbsp;
                </a-descriptions-item>
                <a-descriptions-item label="加急测试调整因子 U (1.0~3.0)" :span="3">
                  <a-input-number v-model:value="form.valueU" :min="1.0" :max="3.0" :step="0.1" />&emsp;&emsp;&nbsp;
                </a-descriptions-item>
                <a-descriptions-item label="现场测试调整因子 X (1.0~1.3)" :span="3">
                  <a-input-number v-model:value="form.valueX" :min="1.0" :max="1.3" :step="0.1" />&emsp;&emsp;&nbsp;
                </a-descriptions-item>
                <a-descriptions-item label="评测机构资质调整因子 A (1.0~1.2)" :span="3">
                  <a-input-number v-model:value="form.valueA" :min="1.0" :max="1.2" :step="0.1" />&emsp;&emsp;&nbsp;
                </a-descriptions-item>
                <a-descriptions-item label="回归测试调整因子 T (0.6~0.8)" :span="3">
                  <a-input-number v-model:value="form.valueT" :min="0.6" :max="0.8" :step="0.1" />&emsp;&emsp;&nbsp;
                </a-descriptions-item>
              </a-descriptions>
            </a-col>
          </a-row>

          <a-row style="border: 1px solid lightgray;border-top:none;" justify="space-around" align="middle">
            <a-col :span="4">回归测试次数</a-col>
            <a-col :span="20">
              <a-descriptions :model="form" bordered>
                <a-descriptions-item label="回归测试次数 n" :span="3" style="width:65%;">
                  <a-input-number v-model:value="form.valueN" :min="0" :step="1" />&emsp;&emsp;&nbsp;
                </a-descriptions-item>
              </a-descriptions>
            </a-col>
          </a-row>

          <a-row style="border: 1px solid lightgray;border-top:none;height:100px; border-radius:0px 0px 10px 10px;"
            justify="space-around" align="middle">
            <a-statistic title="软件测试成本调整因子(DF)=DF =C*I*R*U*X*A*(1+n*Tr)" :precision="2"
              :value-style="{ fontSize: '20px' }" :value=DF>
            </a-statistic>
          </a-row>
        </div>
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
          （b）测试人工成本：LC = {{ UW }} * {{ DF }} * {{ S }} = {{ LC }}（元）
        </a-row>
      </div>

      <!--第四步：计算测试工具成本
      IC ———测试工具成本,单位为元; 
      OT———自有工具成本,参见4.1.2进行度量,单位为元; 
      RT———租借工具成本,依据租借费用另行估计,单位为元。 
      年限平均法：将固定资产按预计使用年限平均计算折旧均衡地分摊到各期。 
      固定资产年折旧额 = 固定资产应计折旧额 / 固定资产预计使用年限
      测试工具成本=固定资产年折旧额+维护费用
      OT=（固定资产应计折旧额 / 固定资产预计使用年限+维护费用）/200*工具实际使用时间
      OT=(depreciation/usefulLife + maintenanceCosts)/200 * actualUsageTime-->
      <a-typography-title :level="5">步骤四：软件测试的工具成本计算</a-typography-title>
      <a-row class="son-line" style="display: table-cell;vertical-align: middle;">
        <a-typography-text>（1）计算公式：IC =OT +RT </a-typography-text>
        <a-popover title="说明：">
          <template #content>
            <p>IC ———测试工具成本【元】</p>
            <p>OT———自有工具成本【元】</p>
            <p>RT———租借工具成本【元】</p>
          </template>
          <question-circle-outlined />
        </a-popover>
      </a-row>
      <a-row class="son-line">（2）计算过程：年限平均法——将固定资产按预计使用年限平均计算折旧均衡地分摊到各期。</a-row>
      <a-row class="son-line" style="padding-left:10px ;">（a）OT=（固定资产应计折旧额 / 固定资产预计使用年限+维护费用）/200*工具实际使用时间</a-row>
      <a-row class="son-line" style="padding-left:10px ;">（b）RT= 租赁费用 * 租赁期限</a-row>

      <div style="width:80%;text-align: center;margin: 0 auto">
        <a-row style="border: 1px solid lightgray;height:50px; border-radius:10px 10px 0px 0px;line-height: 50px;"
          justify="space-around" align="middle">
          <a-col :span="24" style="font-weight: 600;">步骤四：软件测试工具成本</a-col>
        </a-row>
        <a-row style="border: 1px solid lightgray;border-top:none;padding-top: 10px;" justify="space-around"
          align="middle">
          <a-col :span="3">
            自有工具成本(OT)
          </a-col>
          <a-col :span="16">
            <a-row v-for="(item, index) in OTForm" style="border: 1px solid lightgray;" justify="space-around"
              align="middle">
              <a-col :span="3">
                <a-row justify="space-around" align="middle">
                  自有工具{{ index + 1 }}
                </a-row>
                <a-row justify="space-around" align="middle">
                  <a-button shape="circle" class="editable-add-btn" style="margin-top:5px" size=small danger
                    @click="handleDeleteOT(index)"><minus-outlined /></a-button>
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
              <a-button class="editable-add-btn" style="width: 100%;margin: 10px 0px 10px 0px;" @click="handleAddOT" ghost
                type="primary"><plus-outlined /></a-button>
            </a-row>
          </a-col>
          <a-col :span="3">
            <a-statistic v-if="OT != -1" title="总计" :value=OT :value-style="{ fontSize: '20px' }" :precision="2"
              suffix="元"></a-statistic>
            <a-statistic v-if="OT == -1" :value='0' :value-style="{ fontSize: '20px', color: 'red' }" :precision="2">
              <template #title>
                <a-tooltip placement="right">
                  <template #title>
                    <span>请检查输入！</span>
                  </template>
                  <question-circle-two-tone />
                </a-tooltip>
              </template>
            </a-statistic>
          </a-col>
        </a-row>
        <a-row style="border: 1px solid lightgray;border-top:none;padding-top: 10px;" justify="space-around"
          align="middle">
          <a-col :span="3">
            租借工具成本(RT)
          </a-col>
          <a-col :span="16">
            <a-row v-for="(item, index) in RTForm" style="border: 1px solid lightgray;" justify="space-around"
              align="middle">
              <a-col :span="3">
                <a-row justify="space-around" align="middle">
                  租借工具{{ index + 1 }}
                </a-row>
                <a-row justify="space-around" align="middle">
                  <a-button shape="circle" class="editable-add-btn" style="margin-top:5px" size=small danger
                    @click="handleDeleteRT(index)"><minus-outlined /></a-button>
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
              <a-button class="editable-add-btn" style="width: 100%;margin: 10px 0px 10px 0px;" @click="handleAddRT" ghost
                type="primary"><plus-outlined /></a-button>
            </a-row>
          </a-col>
          <a-col :span="3">
            <a-statistic title="总计" :value=RT :value-style="{ fontSize: '20px' }" :precision="2" suffix="元"></a-statistic>
          </a-col>
        </a-row>
        <a-row style="border: 1px solid lightgray;border-top:none;height:100px; border-radius:0px 0px 10px 10px;"
          justify="space-around" align="middle">
          <a-statistic title="软件测试工具成本(IC)=OT+RT" :precision="2" :value-style="{ fontSize: '20px' }" :value=IC
            suffix="元"></a-statistic>
        </a-row>
      </div>


      <!--第五步：计算软件测试直接成本
      DC ———直接成本,单位为元; 
      LC ———测试人工成本,单位为元; 
      EC ———测试环境成本,宜不超过软件测试人工成本的20%,单位为元; 
      IC ———测试工具成本,单位为元。 
      -->
      <div>
        <a-typography-title :level="5">步骤五：软件测试直接成本计算</a-typography-title>
        <a-row class="son-line" style="display: table-cell;vertical-align: middle;">
          <a-typography-text>（1）计算公式：DC =LC +EC +IC </a-typography-text>
          <a-popover title="说明：">
            <template #content>
              <p>DC ———直接成本【元】</p>
              <p>LC ———测试人工成本【元】</p>
              <p>EC ———测试环境成本,宜不超过软件测试人工成本的20%【元】</p>
              <p>IC ———测试工具成本【元】</p>
            </template>
            <question-circle-outlined />
          </a-popover>
        </a-row>
        <a-row class="son-line">（2）计算过程：</a-row>
        <a-row class="son-line" style="padding-left:10px ;">（a）测试环境成本 EC（宜不超过测试人工成本 LC 的20%）：
          <a-input-number v-model:value="EC" size="small" :min="0" :max="(0.2 * LC).toFixed(2)" />（元）
        </a-row>
        <a-row class="son-line" style="padding-left:10px;">
          （b）软件测试直接成本：DC = {{ LC }} + {{ EC }} +{{ IC }} = {{ DC }}（元）
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
          <a-input-number v-model:value="IDC" size="small" :min="0" :max="(0.2 * DC).toFixed(2)" />（元）
        </a-row>
        <a-row class="son-line" style="padding-left:10px;">
          （b）软件测试成本：STC = {{ DC }} + {{ IDC }} = {{ STC }}（元）
        </a-row>
      </div>
    </a-layout-content>
    <!--a-layout-footer class="my-layout-footer">实验结束！</!--a-layout-footer-->
  </a-layout>
  <a-back-top />
</template>

<script lang="ts">
import { ECDH } from 'crypto';
import { defineComponent, reactive, computed, ref } from 'vue';
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
    const EC = ref<number>(0);//测试环境成本
    const IDC = ref<number>(0);//间接成本

    const form = reactive({

      valueC: 1.0,
      valueI: 1.0,
      valueR: 1.0,
      valueU: 1.0,
      valueX: 1.0,
      valueA: 1.0,
      valueT: 0.6,
      valueN: 1,
    });

    // const DF = computed(() => {
    //       return form.valueC*form.valueI*form.valueR*form.valueU*form.valueX*form.valueA*(1+form.valueN*form.valueT);
    //     });



    //未调整的软件测试人工工作量
    const UW = computed(() => {
      var x = Math.round((TW.value + TW.value * SR_weight.value + TW.value * DR_weight.value) * 100) / 100;
      return x;
    });

    //软件测试人工成本调整因子
    const DF = computed(() => {
      //修改这里
      var x = form.valueC * form.valueI * form.valueR * form.valueU * form.valueX * form.valueA * (1 + form.valueN * form.valueT);
      return x;
    });

    const calculate = (event: Event) => {
      event.preventDefault();
      console.log('DF', DF.value);
    };

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
      var x = 0;
      for (var i = 0; i < OTForm.value.length; i++) {
        x += Math.round(((OTForm.value[i].depreciation / OTForm.value[i].usefulLife + OTForm.value[i].maintenanceCosts) / 200) *
          OTForm.value[i].actualUsageTime);
      }
      if (!x && x != 0 || x == Infinity) {
        return -1
      }
      return x;
    });

    //租借工具成本
    const RT = computed(() => {
      var x = 0;
      for (var i = 0; i < RTForm.value.length; i++) {
        x += RTForm.value[i].rentExpense * RTForm.value[i].termOfLease;
      }
      if (!x && x != 0 || x == Infinity) {
        return -1
      }
      return x;
    });

    //测试工具成本
    const IC = computed(() => {
      if (!(OT.value + RT.value) || OT.value < 0) {
        return 0;
      }
      else {
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
      OTForm.value.splice(index, 1);
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
      RTForm.value.splice(index, 1);
    };

    //软件测试直接成本 DC = {{LC}} + {{EC}} +{{IC}} 
    const DC = computed(() => {
      var x = Math.round(LC.value + EC.value + IC.value);
      return x;
    });

    //软件测试成本
    const STC = computed(() => {
      var x = Math.round(DC.value + IDC.value);
      return x;
    });

    return {
      GBtestlink,
      UW,
      TW,
      SR_weight,
      DR_weight,
      form,
      DF,
      calculate,
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
      EC,
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
.GBlink {
  margin-left: auto;
  color: cadetblue;
  font-style: italic;
  text-decoration: underline
}

.my-layout-header {
  text-align: center;
  font-size: large;
  font-weight: bold;
}

// .my-layout-content{
// }
.my-layout-footer {
  text-align: center;
  font-size: large;
  background-color: white;
}

.son-line {
  line-height: 30px;
}
</style>
