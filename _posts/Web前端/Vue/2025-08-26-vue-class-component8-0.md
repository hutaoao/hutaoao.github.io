---
title: vue-class-component8-0
date: 2025-08-26
description: vue-class-component 8.0
tags: [Vue, Vue3, vue]
categories: [Web前端, Vue]
---
# vue-class-component 8.0

### data
```javascript
import {Vue} from 'vue-class-component';

export default class EquipmentListIndex extends Vue {
  total = 0;
  size = 10;
  current = 1;
  devicesId = 0; // 列表ID
}
```

或

```vue
import {Vue} from 'vue-class-component';

export default class EquipmentListIndex extends Vue {
  data() {
    return {
      total = 0;
      size = 10;
      current = 1;
      devicesId = 0; // 列表ID
    }
  }
}
```

### Methods 属性
组件方法可以直接声明为类的原型方法：

```vue
<template>
  <button v-on:click="hello">Click</button>
</template>


<script>
import {Vue} from 'vue-class-component'

export default class HelloWorld extends Vue {
  hello() {
    console.log('Hello World!')
  }
}
</script>

```

### Computed (计算属性)
```vue
<template>
  <input v-model="name">
</template>


<script>
import {Vue} from 'vue-class-component'

export default class HelloWorld extends Vue {
  firstName = 'John'
  lastName = 'Doe'

  // Declared as computed property getter
  get name() {
    return this.firstName + ' ' + this.lastName
  }

  // Declared as computed property setter
  set name(value) {
    const splitted = value.split(' ')
    this.firstName = splitted[0]
    this.lastName = splitted[1] || ''
  }
}
</script>
```

### watch - 监听
```javascript
import {Options} from 'vue-class-component';

@Options({
  watch: {
    instCode: {
      handler() {
        this.resetForm();
        this.getPages();
      }
    }
  }
})
```

### hooks
```vue
import {Vue} from 'vue-class-component';
  
export default class HelloWorld extends Vue {
  // 所有Vue生命周期挂钩也可以直接声明为类原型方法，但是您不能在实例本身上调用它们。
  // 声明自定义方法时，应避免使用这些保留名称。
  mounted() {
    console.log('mounted')
  }
}
```

### 注册钩子
```vue
import {Vue} from 'vue-class-component';

Vue.registerHooks(['beforeRouteLeave']);

export default class HelloWorld extends Vue {
  
}
```

### 组件
```javascript
import {Options} from 'vue-class-component';

@Options({
  components: {
    FillInfoDialog,
    ChooseClassDialog
  }
})
```

### Props
```vue
import {Vue} from 'vue-class-component';
  
const GreetingProps = Vue.extend({
  props: {
    name: String
  }
})

export default class Greeting extends GreetingProps {
  
}
```



