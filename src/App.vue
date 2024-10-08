<template>
  <div class="container">
    <h1>测试数据生成器</h1>
    <!-- 确保组件在这里被使用 -->
    <template-selector @template-selected="loadTemplate" />
    <config-form :template-data="templateData" @generate="generateData" />
   </div>
</template>

<script>
import ConfigForm from './components/ConfigForm.vue';
import TemplateSelector from './components/TemplateSelector.vue';

export default {
  components: {
    ConfigForm,
    TemplateSelector,
  },
  data() {
    return {
      templateData: null,
    };
  },
  methods: {
    loadTemplate(template) {
      this.templateData = template;
    },
    generateData(data) {
      console.log('生成的数据:', data);
    },
  },
};
</script>
