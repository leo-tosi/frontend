<template>
    <div>
      <div class="mb-3">
        <label for="file_name">文件名:</label>
        <input type="text" class="form-control" v-model="fileName" required />
      </div>
  
      <div class="mb-3">
        <label for="total_columns">总列数:</label>
        <input type="number" class="form-control" v-model="totalColumns" required />
      </div>
  
      <button class="btn btn-primary" @click="addRow">添加列</button>
      <table class="table table-bordered mt-3">
        <thead>
          <tr>
            <th>列号</th>
            <th>列名</th>
            <th>数据类型</th>
            <th>参数</th>
            <th>操作</th>
          </tr>
        </thead>
        <tbody>
          <data-row v-for="(row, index) in rows" :key="index" :rowData="row" @delete-row="removeRow(index)" />
        </tbody>
      </table>
  
      <button class="btn btn-success" @click="submitData">生成数据</button>
    </div>
  </template>
  
  <script>
  import DataRow from './DataRow.vue';
  
  export default {
    components: { DataRow },
    props: ['templateData'],
    data() {
      return {
        fileName: '',
        totalColumns: '',
        rows: []
      };
    },
    methods: {
      addRow() {
        this.rows.push({ columnName: '', type: '', params: '' });
      },
      removeRow(index) {
        this.rows.splice(index, 1);
      },
      submitData() {
        const data = {
          fileName: this.fileName,
          totalColumns: this.totalColumns,
          rows: this.rows
        };
        this.$emit('generate', data);
      }
    },
    watch: {
      templateData: {
        immediate: true,
        handler(newTemplate) {
          if (newTemplate) {
            this.rows = newTemplate.rows || [];
          }
        }
      }
    }
  };
  </script>
  