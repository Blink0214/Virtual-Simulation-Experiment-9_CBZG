<template>
    <a-collapse :bordered="false">
      <template #expandIcon="{ isActive }">
        <right-outlined style="fontSize:20px;height: 30px;line-height: 30px;" :rotate="isActive ? 90 : 0" />
      </template>
      <a-collapse-panel style="background-color: white;">
        <template #header>
          <span style="font-size:20px;">hhh</span>
        </template>
        <p>hhhhhhhhh</p>
      </a-collapse-panel>
    </a-collapse>
  </template>
  <script lang="ts">
  import { CaretRightOutlined } from '@ant-design/icons-vue';
  import { defineComponent, ref } from 'vue';
  
  export default defineComponent({
    components: {
      CaretRightOutlined,
    },
    setup() {
      const activeKey = ref(['1']);
      const text = `A dog is a type of domesticated animal.Known for its loyalty and faithfulness,it can be found as a welcome guest in many households across the world.`;
      const customStyle =
        'background: #f7f7f7;border-radius: 4px;margin-bottom: 24px;border: 0;overflow: hidden';
      return {
        activeKey,
        text,
        customStyle,
      };
    },
  });
  </script>
  
  