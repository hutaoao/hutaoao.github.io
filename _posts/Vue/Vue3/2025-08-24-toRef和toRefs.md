---
title: toRef和toRefs
date: 2025-08-24
description: toRef和toRefs
tags: [Vue, Vue3]
categories: [Vue, Vue3]
---
# toRef和toRefs

```vue
<script>
  import {reactive} from "vue";
  export default {
    name: 'Toreftemp',
    components: {

    },
    setup(){
      let listData = reactive({
        title: '2021年vue3你还不会吗，那你Out了',
        content: "撒看到啦奥斯卡大奖阿斯顿阿斯顿",
        list: {
          shareNUm: 20,
          countall: 15
        }
      })

      return{
        listData

      }
    }
  }
</script>

```



我们需要在模板中使用，那就得 listData.各种才能使用，listData.title listData.content

```vue
<template>
    <div>
        <h1>toRef和toRefs</h1>
      <div>
{% raw %}
          <h2>{{listData.title}}</h2>
{% endraw %}
{% raw %}
          <P>{{listData.content}}</P>
{% endraw %}
{% raw %}
          <span>分享次数:{{listData.list.shareNUm}}</span>
{% endraw %}
{% raw %}
          <span>收藏次数:{{listData.list.countall}}</span>
{% endraw %}
      </div>
    </div>
</template>
```



这样就显得很难受，不是不可以，有的小伙伴灵机一动。改造一下数据不就完事，已title标题为例

```vue
return {
	title: listData.title,
	content: listData.content,
}
```

{% raw %}
这样那我在模板中不是直接就用{{title}}完事，没错是可以的，但是问题来了，你修改数据的时候，你会发现你的UI没有跟新，傻了吧，
{% endraw %}



其实这已经是一个新的对象了，相当于进行了解构赋值，返回的对象已经不再是一个响应式对象了，而是一个新的普通对象，



从而我们就可以使用toRef和toRefs来解决这个问题，



1. 首先先看toRef  
里面有两个参数。参数一为一个响应对象，参数二为参数一这个对象中的某个属性。并将这个属性扎转换为响应式数据。

```vue
<script>

import {reactive,toRef} from "vue";

export default {
  name: 'Toreftemp',
  components: {

  },
  setup(){
   
    let listData = reactive({
      title: '2021年vue3你还不会吗，那你Out了',
      content: "撒看到啦奥斯卡大奖阿斯顿阿斯顿",
      list: {
        shareNUm: 20,
        countall: 15
      }
    })

    return{
        title: toRef(listData, 'title'),
        content: toRef(listData, 'content'),
        shareNUm: toRef(listData.list, 'shareNUm'),
        countall: toRef(listData.list, 'countall')
    }
  }
}
</script>
```

模板中使用

```vue
<template>
    <div>
        <h1>toRef和toRefs</h1>
      <div>
{% raw %}
          <h2>{{title}}</h2>
{% endraw %}
{% raw %}
          <P>{{content}}</P>
{% endraw %}
{% raw %}
          <span>分享次数:{{shareNUm}}</span>
{% endraw %}
{% raw %}
          <span>收藏次数:{{countall}}</span>
{% endraw %}
      </div>
    </div>

</template>
```



然而，假如我们的对象中有几百个属性那怎么办，那不是得写疯了，从而toRefs就用到了

```vue
<template>
    <div>
        <h1>toRef和toRefs</h1>
      <div>
{% raw %}
          <h2>{{title}}</h2>
{% endraw %}
{% raw %}
          <P>{{content}}</P>
{% endraw %}
{% raw %}
          <span>分享次数:{{list.shareNUm}}</span>
{% endraw %}
{% raw %}
          <span>收藏次数:{{list.countall}}</span>
{% endraw %}
      </div>
    </div>

</template>

<script>

import {reactive,toRef,toRefs} from "vue";

export default {
  name: 'Toreftemp',
  components: {

  },
  setup(){
   
    let listData = reactive({
      title: '2021年vue3你还不会吗，那你Out了',
      content: "撒看到啦奥斯卡大奖阿斯顿阿斯顿",
      list: {
        shareNUm: 20,
        countall: 15
      }
    })

    return{
      ...toRefs(listData)
    }
  }
}
</script>
```

哇是不是恍然大悟，原来就是这样哦，没错就是这样.



