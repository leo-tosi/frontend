<template>
    <tr>
        <td><input type="text" class="form-control" v-model="localRowData.columnName" /></td>
        <td>
            <select v-model="localRowData.type" @change="handleTypeChange" class="form-control">
                <option value="fixed">固定值</option>
                <option value="random_number">随机数</option>
                <option value="patterned_number">规律数字</option>
                <option value="text">文字</option>
                <option value="date">日期</option>
            </select>
        </td>
        <td>
            <div v-if="localRowData.type === 'random_number'">
                <label>起始数字:</label>
                <input type="number" v-model="localRowData.start" class="form-control" />
                <label>结束数字:</label>
                <input type="number" v-model="localRowData.end" class="form-control" />
            </div>
            <div v-if="localRowData.type === 'patterned_number'">
                <label>起始数字:</label>
                <input type="number" v-model="localRowData.start" class="form-control" />
                <label>步长:</label>
                <input type="number" v-model="localRowData.step" class="form-control" />
                <label>接头字符:</label>
                <input type="text" v-model="localRowData.prefix" class="form-control" />
                <label>接尾字符:</label>
                <input type="text" v-model="localRowData.suffix" class="form-control" />
            </div>
            <div v-if="localRowData.type === 'text'">
                <label>类型:</label>
                <select v-model="localRowData.textType" class="form-control">
                    <option value="hiragana">平假名</option>
                    <option value="katakana">片假名</option>
                    <option value="mixed">混合</option>
                </select>
                <label>长度:</label>
                <input type="number" v-model="localRowData.length" class="form-control" />
            </div>
            <div v-if="localRowData.type === 'date'">
                <label>类型:</label>
                <select v-model="localRowData.dateType" class="form-control">
                    <option value="Date">Date</option>
                    <option value="Datetime">Datetime</option>
                    <option value="Timestamp">Timestamp</option>
                </select>
                <label>生成方式:</label>
                <select v-model="localRowData.generationMethod" class="form-control">
                    <option value="current">当前日期</option>
                    <option value="random">随机日期</option>
                    <option value="specific">指定日期</option>
                </select>
            </div>
        </td>
        <td><button class="btn btn-danger" @click="$emit('delete-row')">删除</button></td>
    </tr>
</template>

<script>
export default {
    props: {
        rowData: {
            type: Object,
            required: true
        }
    },
    data() {
        return {
            localRowData: { ...this.rowData } // 通过本地副本管理数据，以避免直接修改 props
        };
    },
    watch: {
        localRowData: {
            handler(newValue) {
                this.$emit('update:rowData', newValue); // 每次修改时发射事件更新父组件数据
            },
            deep: true
        }
    },
    methods: {
        handleTypeChange() {
            // 当类型改变时，清除不必要的参数
            this.clearParamsForType(this.localRowData.type);
        },
        clearParamsForType(type) {
            if (type === 'random_number') {
                this.localRowData.start = null;
                this.localRowData.end = null;
            } else if (type === 'patterned_number') {
                this.localRowData.start = null;
                this.localRowData.step = null;
                this.localRowData.prefix = '';
                this.localRowData.suffix = '';
            } else if (type === 'text') {
                this.localRowData.textType = 'mixed';
                this.localRowData.length = 10;
            } else if (type === 'date') {
                this.localRowData.dateType = 'Datetime';
                this.localRowData.generationMethod = 'current';
            }
        }
    }
};
</script>
