<template>
    <div class="mb-3">
      <label for="template-select">选择模板:</label>
      <select id="template-select" class="form-control" @change="selectTemplate">
        <option value="">请选择模板</option>
        <option v-for="template in templates" :key="template.id" :value="template">{{ template.name }}</option>
      </select>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        templates: [
          { id: 1, name: '模板1' },
          { id: 2, name: '模板2' }
        ]
      };
    },
    methods: {
      selectTemplate(event) {
        const selected = this.templates.find(t => t.name === event.target.value);
        this.$emit('template-selected', selected);
      }
    }
  };
  </script>
  