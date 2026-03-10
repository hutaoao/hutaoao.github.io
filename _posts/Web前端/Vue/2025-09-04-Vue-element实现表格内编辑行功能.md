---
title: Vue-element实现表格内编辑行功能
date: 2025-09-04
description: Vue+element 实现表格内编辑行功能
tags: [Vue, vue]
categories: [Web前端, Vue]
---
# Vue+element 实现表格内编辑行功能
      <el-button type="primary" @click="save">保存</el-button>
    </div>
    <el-table :data="tableData" border show-summary :summary-method="getSummaries" style="width: 800px">
      <el-table-column prop="name" label="销售经理"></el-table-column>
      <el-table-column prop="total" label="人员年度目标"></el-table-column>
      <el-table-column prop="amount1" sortable label="1月">
        <template v-slot="scoped">
          <custom-input :show-input="scoped.row.showInput" :value.sync="scoped.row.amount1" @getValue="getValue($event, scoped.$index, 'amount1')"/>
        </template>
      </el-table-column>
      <el-table-column prop="amount2" sortable label="2月">
        <template v-slot="scoped">
          <custom-input :show-input="scoped.row.showInput" :value.sync="scoped.row.amount2" @getValue="getValue($event, scoped.$index, 'amount2')"/>
        </template>
      </el-table-column>
      <el-table-column prop="amount3" sortable label="3月">
        <template v-slot="scoped">
          <custom-input :show-input="scoped.row.showInput" :value.sync="scoped.row.amount3" @getValue="getValue($event, scoped.$index, 'amount3')"/>
        </template>
      </el-table-column>
      <el-table-column label="操作">
        <template v-slot="scoped">
          <el-button type="text" @click="edit(scoped.$index)" v-if="!scoped.row.showInput">edit</el-button>
          <el-button type="text" @click="save(scoped.$index)" v-if="scoped.row.showInput">save</el-button>
          <el-button type="text" @click="cancel(scoped.$index)" v-if="scoped.row.showInput">cancel</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
import CustomInput from './components/CustomInput';

export default {
  components: {
    CustomInput
  },
  data() {
    return {
      tableData: [{
        name: '张三',
        total: '24',
        showInput: false,
        amount1: '8',
        amount2: '8',
        amount3: '8',
      }, {
        name: '李四',
        total: '18',
        showInput: false,
        amount1: '6',
        amount2: '6',
        amount3: '6',
      }, {
        name: '王五',
        total: '12',
        showInput: false,
        amount1: '4',
        amount2: '4',
        amount3: '4',
      }],
      tableTemporary: [],
    };
  },
  methods: {
    getSummaries(param) {
      const {columns, data} = param;
      const sums = [];
      columns.forEach((column, index) => {
        if (index === 0) {
          sums[index] = '合计';
          return;
        }
        const values = data.map(item => Number(item[column.property]));
        if (!values.every(value => isNaN(value))) {
          sums[index] = values.reduce((prev, curr) => {
            const value = Number(curr);
            if (!isNaN(value)) {
              return prev + curr;
            } else {
              return prev;
            }
          }, 0);
          sums[index] += ' 元';
        } else {
          sums[index] = 'N/A';
        }
      });
      return sums;
    },
    edit(index) {
      this.tableTemporary[index] = new Map();
      this.tableData[index].showInput = true;
    },
    save(index) {
      this.tableData[index].showInput = false;
      for (const [key, value] of this.tableTemporary[index]) {
        this.tableData[index][key] = value;
        // this.tableData[index].total = 123;
      }
      console.log(this.tableData)
    },
    cancel(index) {
      this.tableData[index].showInput = false;
    },
    getValue(val, index, amount) {
      this.tableTemporary[index].set(amount, val);
    }
  }
};
</script>

<style>

</style>

```

```vue
<template>
  <div class="hello">
    <el-input v-model="inputValue" @input="changeInput" style="width: 80px;" v-if="showInput"/>
{% raw %}
    <i v-else>{{ value }}</i>
{% endraw %}
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    showInput: {
      type: Boolean,
      default: true,
    },
    value: String
  },
  watch: {
    showInput(val) {
      if (val) {
        this.inputValue = this.value;
      }
    }
  },
  data() {
    return {
      inputValue: '',
    }
  },
  methods: {
    changeInput(val) {
      this.$emit('getValue', val);
    }
  }
}
</script>

<style scoped>

</style>

```



