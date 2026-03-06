---
title: 解析Pinia和Vuex
date: 2025-08-31
description: 解析 Pinia 和 Vuex
tags: [Vue, Vue3, vue]
categories: [Vue, Vue3]
---
# 解析 Pinia 和 Vuex

## <font style="color:rgb(255, 255, 255);background-color:rgb(33, 33, 34);">安装</font>

* **<font style="color:rgb(1, 1, 1);">Vuex</font>**

`npm i vuex -S`

* **<font style="color:rgb(1, 1, 1);">Pinia</font>**

`npm i pinia -S`

## <font style="color:rgb(255, 255, 255);background-color:rgb(33, 33, 34);">挂载</font>

### <font style="color:black;">Vuex</font>

<font style="color:black;">在src目录下新建vuexStore,实际项目中你只需要建一个store目录即可，由于我们需要两种状态管理器，所以需要将其分开并创建两个store目录</font>

<font style="color:black;">新建 </font><code><font style="color:black;">vuexStore/index.js</font></code>

import { createStore } from 'vuex'

export default createStore({
  //全局state，类似于vue种的data
  state() {
    return {
      vuexmsg: "hello vuex",
      name: "xiaoyue",
    };
  },


  //修改state函数
  mutations: {
  },

  //提交的mutation可以包含任意异步操作
  actions: {
  },

  //类似于vue中的计算属性
  getters: {
  },

  //将store分割成模块（module）,应用较大时使用
  modules: {
  }
})


<font style="color:black;">main.js引入</font>

import { createApp } from 'vue'
import App from './App.vue'
import store from '@/store'
createApp(App).use(store).mount('#app')


<font style="color:black;">App.vue测试</font>

<template>
  <div></div>
</template>
<script setup>
  import { useStore } from 'vuex'
  let vuexStore = useStore()
  console.log(vuexStore.state.vuexmsg); //hello vuex
</script>


<font style="color:black;">页面正常打印hello vuex说明我们的Vuex已经挂载成功了</font>

### <font style="color:black;">Pinia</font>

* **<font style="color:rgb(1, 1, 1);">main.js引入</font>**

import { createApp } from "vue";
import App from "./App.vue";
import {createPinia} from 'pinia'
const pinia = createPinia()
createApp(App).use(pinia).mount("#app");


* **<font style="color:rgb(1, 1, 1);">创建Store</font>**

<font style="color:black;">1）src下新建piniaStore/storeA.js</font>

import { defineStore } from "pinia";

export const storeA = defineStore("storeA", {
  state: () => {
    return {
      piniaMsg: "hello pinia",
    };
  },
  getters: {},
  actions: {},
});


<font style="color:rgb(44, 62, 80);"></font>

<font style="color:rgb(44, 62, 80);">2）为实现更多高级用法，你甚至可以使用一个函数(与组件 </font>setup()<font style="color:rgb(44, 62, 80);"> 类似)来定义一个 Store：</font>你也可以

export const useCounterStore = defineStore('counter', () => {
  const piniaMsg = ref("hello pinia")
  function increment() {
    count.value++
  }

  return { count, increment }
})


<font style="color:rgb(44, 62, 80);">在</font><font style="color:rgb(44, 62, 80);"> </font>*<font style="color:rgb(44, 62, 80);">Setup Store</font>*<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中：</font>

* <font style="color:#E8323C;">ref() 就是 state 属性</font>

* <font style="color:#E8323C;">computed()</font><font style="color:#E8323C;"> </font><font style="color:#E8323C;">就是</font><font style="color:#E8323C;"> </font><font style="color:#E8323C;">getters</font>

* <font style="color:#E8323C;">function() 就是 actions</font>

* **<font style="color:rgb(1, 1, 1);">App.vue使用</font>**

<template>
  <div></div>
</template>
<script setup>
  import { storeA } from '@/piniaStore/storeA'
  let piniaStoreA = storeA()
  console.log(piniaStoreA.piniaMsg); //hello pinia
</script>


<font style="color:black;">从这里我们可以看出</font><code><font style="color:black;">pinia</font></code><font style="color:black;">中没有了</font><code><font style="color:black;">mutations</font></code><font style="color:black;">和</font><code><font style="color:black;">modules</font></code><font style="color:black;">，</font><code><font style="color:black;">pinia</font></code><font style="color:black;">不必以嵌套（通过modules引入）的方式引入模块，因为它的每个store便是一个模块，如storeA，storeB... 。在我们使用Vuex的时候每次修改state的值都需要调用mutations里的修改函数（下面会说到），因为Vuex需要追踪数据的变化，这使我们写起来比较繁琐。而pinia则不再需要mutations，同步异步都可在actions进行操作，至于它没有了mutations具体是如何最终到state变化的，这里我们不过多深究,</font>~~<font style="color:black;">大概好像应该是通过hooks回调的形式解决的把（我也没研究过，瞎猜的</font>~~<font style="color:black;">。</font>

## <font style="color:rgb(255, 255, 255);background-color:rgb(33, 33, 34);">修改状态</font>

<font style="color:black;">获取state的值从上面我们已经可以一目了然的看到了，下面让我们看看他俩修改state的方法吧</font>

### <font style="color:black;">vuex</font>

<font style="color:black;">vuex在组件中直接修改state，如App.vue</font>

<template>
{% raw %}
  <div>{{vuexStore.state.vuexmsg}}</div>
{% endraw %}
</template>
<script setup>
  import { useStore } from 'vuex'
  let vuexStore = useStore()
  vuexStore.state.vuexmsg = 'hello juejin'
  console.log(vuexStore.state.vuexmsg)

</script>


<font style="color:black;">可以看出我们是可以直接在组件中修改state的而且还是响应式的，但是如果这样做了，vuex不能够记录每一次state的变化记录，影响我们的调试。当vuex开启严格模式的时候，直接修改state会抛出错误，所以官方建议我们开启严格模式，所有的state变更都在vuex内部进行，在mutations进行修改。例如</font>

<font style="color:black;">vuexStore/index.js:</font>

import { createStore } from "vuex";

export default createStore({
  strict: true,
  //全局state，类似于vue种的data
  state: {
    vuexmsg: "hello vuex",
  },

  //修改state函数
  mutations: {
    setVuexMsg(state, data) {
      state.vuexmsg = data;
    },
  },

  //提交的mutation可以包含任意异步操作
  actions: {},

  //类似于vue中的计算属性
  getters: {},

  //将store分割成模块（module）,应用较大时使用
  modules: {},
});


<font style="color:black;">当我们需要修改vuexmsg的时候需要提交setVuexMsg方法，如App.vue</font>

<template>
{% raw %}
  <div>{{ vuexStore.state.vuexmsg }}</div>
{% endraw %}
</template>
<script setup>
  import { useStore } from 'vuex'
  let vuexStore = useStore()
  vuexStore.commit('setVuexMsg', 'hello juejin')
  console.log(vuexStore.state.vuexmsg) //hello juejin

</script>


<font style="color:black;">或者我们可以在actions中进行提交mutations修改state:</font>

import { createStore } from "vuex";
export default createStore({
  strict: true,
  //全局state，类似于vue种的data
  state() {
    return {
      vuexmsg: "hello vuex",
    }
  },

  //修改state函数
  mutations: {
    setVuexMsg(state, data) {
      state.vuexmsg = data;
    },
  },

  //提交的mutation可以包含任意异步操作
  actions: {
    async getState({ commit }) {
      //const result = await xxxx 假设这里进行了请求并拿到了返回值
      commit("setVuexMsg", "hello juejin");
    },
  }
});


<font style="color:black;">组件中使用dispatch进行分发actions</font>

<template>
{% raw %}
  <div>{{ vuexStore.state.vuexmsg }}</div>
{% endraw %}
</template>
<script setup>
  import { useStore } from 'vuex'
  let vuexStore = useStore()
  vuexStore.dispatch('getState')

</script>


<font style="color:black;">一般来说，vuex中的流程是首先actions一般放异步函数，拿请求后端接口为例，当后端接口返回值的时候，actions中会提交一个mutations中的函数，然后这个函数对vuex中的状态（state）进行一个修改，组件中再渲染这个状态，从而实现整个数据流程都在vuex内部进行便于检测。直接看图，一目了然</font>

![1668132250604-0e0ac4aa-248d-4e4a-ab39-3f811ab9443d.jpeg](/assets/img/posts/Vue/1668132250604-0e0ac4aa-248d-4e4a-ab39-3f811ab9443d-938514.jpeg)

<font style="color:rgb(136, 136, 136);">1f0c7f44205b2a793829d22509fac74.png</font>

### <font style="color:black;">Pinia</font>

* **<font style="color:rgb(1, 1, 1);">直接修改</font>**

<font style="color:black;">相比于Vuex，Pinia是可以直接修改状态的，并且调试工具能够记录到每一次state的变化，如App.vue</font>

<template>
{% raw %}
  <div>{{ piniaStoreA.piniaMsg }}</div>
{% endraw %}
</template>
<script setup>
  import { storeA } from '@/piniaStore/storeA'
  let piniaStoreA = storeA()
  console.log(piniaStoreA.piniaMsg); //hello pinia

  piniaStoreA.piniaMsg = 'hello juejin'
  console.log(piniaStoreA.piniaMsg); //hello juejin

</script>


* **<font style="color:rgb(1, 1, 1);">$patch</font>**

<font style="color:black;">使用$patch方法可以修改多个state中的值,比如我们在piniaStore/storeA.js中的state增加一个name</font>

import { defineStore } from "pinia";

export const storeA = defineStore("storeA", {
  state: () => {
    return {
      piniaMsg: "hello pinia",
      name: "xiaoyue",
    };
  },
  getters: {},
  actions: {},
});


<font style="color:black;">然后我们在App.vue中进行修改这两个state</font>

import { storeA } from '@/piniaStore/storeA'
let piniaStoreA = storeA()
console.log(piniaStoreA.name); //xiaoyue
piniaStoreA.$patch({
  piniaMsg: 'hello juejin',
  name: 'daming'
})
console.log(piniaStoreA.name);//daming


<font style="color:black;">当然也是支持修改单个状态的如</font>

piniaStoreA.$patch({
  name: 'daming'
})


<font style="color:black;">$patch还可以使用函数的方式进行修改状态</font>

import { storeA } from '@/piniaStore/storeA'
let piniaStoreA = storeA()
cartStore.$patch((state) => {
  state.name = 'daming'
  state.piniaMsg = 'hello juejin'
})


* **<font style="color:rgb(1, 1, 1);">在actions中进行修改</font>**

<font style="color:black;">不同于Vuex的是，Pinia去掉了mutations，所以在actions中修改state就行Vuex在mutations修改state一样。其实这也是我比较推荐的一种修改状态的方式，就像上面说的，这样可以实现整个数据流程都在状态管理器内部，便于管理。</font>

<font style="color:black;">在piniaStore/storeA.js的actions添加一个修改name的函数</font>

import { defineStore } from "pinia";
export const storeA = defineStore("storeA", {
  state: () => {
    return {
      piniaMsg: "hello pinia",
      name: "xiao yue",
    };
  },
  actions: {
    setName(data) {
      this.name = data;
    },
  },
});


<font style="color:black;">组件App.vue中调用不需要再使用dispatch函数，直接调用store的方法即可</font>

import { storeA } from '@/piniaStore/storeA'
let piniaStoreA = storeA()
piniaStoreA.setName('daming')


* **<font style="color:rgb(1, 1, 1);">重置state</font>**

<font style="color:black;">Pinia可以使用$reset将状态重置为初始值</font>

import { storeA } from '@/piniaStore/storeA' 
let piniaStoreA = storeA()
piniaStoreA.$reset()


## <font style="color:rgb(255, 255, 255);background-color:rgb(33, 33, 34);">Pinia解构(storeToRefs)</font>

<font style="color:black;">当我们组件中需要用到state中多个参数时，使用解构的方式取值往往是很方便的，但是传统的ES6解构会使state失去响应式，比如组件App.vue,我们先解构取得name值，然后再去改变name值，然后看页面是否变化</font>

<template>
{% raw %}
  <div>{{ name }}</div>
{% endraw %}
</template>
<script setup>
  import { storeA } from '@/piniaStore/storeA'
  let piniaStoreA = storeA()
  let { piniaMsg, name } = piniaStoreA
  piniaStoreA.$patch({
    name: 'daming'
  })

</script>


<font style="color:black;">浏览器展示如下</font>

![1668132250855-c14f02ba-9282-4bfb-91c4-c6e9a3efe7e2.jpeg](/assets/img/posts/Vue/1668132250855-c14f02ba-9282-4bfb-91c4-c6e9a3efe7e2-205111.jpeg)

<font style="color:rgb(136, 136, 136);">1657813193335.jpg</font>

<font style="color:black;">我们可以发现浏览器并没有更新页面为daming</font>

<font style="color:black;">为了解决这个问题，Pinia提供了一个结构方法</font>**<font style="color:black;">storeToRefs</font>**<font style="color:black;">，我们将组件App.vue使用</font>**<font style="color:black;">storeToRefs</font>**<font style="color:black;">解构</font>

<template>
{% raw %}
  <div>{{ name }}</div>
{% endraw %}
</template>
<script setup>
  import { storeA } from '@/piniaStore/storeA'
  import { storeToRefs } from 'pinia'
  let piniaStoreA = storeA()
  let { piniaMsg, name } = storeToRefs(piniaStoreA)
  piniaStoreA.$patch({
    name: 'daming'
  })

</script>


<font style="color:black;">再看下页面变化</font>

<font style="color:rgb(136, 136, 136);">1657813178903.jpg</font>

<font style="color:black;">我们发现页面已经被更新成daming了</font>

## <font style="color:rgb(255, 255, 255);background-color:rgb(33, 33, 34);">getters</font>

<font style="color:black;">其实Vuex中的getters和Pinia中的getters用法是一致的，用于自动监听对应state的变化，从而动态计算返回值(和vue中的计算属性差不多),并且getters的值也具有缓存特性</font>

### <font style="color:black;">Pinia</font>

<font style="color:black;">我们先将piniaStore/storeA.js改为</font>

import { defineStore } from "pinia";

export const storeA = defineStore("storeA", {
  state: () => {
    return {
      count1: 1,
      count2: 2,
    };
  },
  getters: {
    sum() {
      console.log('我被调用了!')
      return this.count1 + this.count2;
    },
  },
});


<font style="color:black;">然后在组件App.vue中获取sum</font>

<template>
{% raw %}
  <div>{{ piniaStoreA.sum }}</div>
{% endraw %}
</template>
<script setup>
  import { storeA } from '@/piniaStore/storeA'
  let piniaStoreA = storeA()
  console.log(piniaStoreA.sum) //3

</script>


<font style="color:black;">让我们来看下什么是缓存特性。首先我们在组件多次访问sum再看下控制台打印</font>

import { storeA } from '@/piniaStore/storeA'
let piniaStoreA = storeA()
console.log(piniaStoreA.sum)
console.log(piniaStoreA.sum)
console.log(piniaStoreA.sum)
piniaStoreA.count1 = 2
console.log(piniaStoreA.sum)


![1668132250684-c6d149a2-45e2-4d6a-ab5c-de1bf41bfae8.jpeg](/assets/img/posts/Vue/1668132250684-c6d149a2-45e2-4d6a-ab5c-de1bf41bfae8-196785.jpeg)

<font style="color:rgb(136, 136, 136);">1657814372565.jpg</font>

<font style="color:black;">从打印结果我们可以看出只有在首次使用用或者当我们改变sum所依赖的值的时候，getters中的sum才会被调用</font>

### <font style="color:black;">Vuex</font>

<font style="color:black;">Vuex中的getters使用和Pinia的使用方式类似，就不再进行过多说明,写法如下vuexStore/index.js</font>

import { createStore } from "vuex";

export default createStore({
  strict: true,
  //全局state，类似于vue种的data
  state: {
    count1: 1,
    count2: 2,
  },

  //类似于vue中的计算属性
  getters: {
    sum(state){
      return state.count1 + state.count2
    }
  }
});


## <font style="color:rgb(255, 255, 255);background-color:rgb(33, 33, 34);">modules</font>

<font style="color:black;">如果项目比较大，使用单一状态库，项目的状态库就会集中到一个大对象上，显得十分臃肿难以维护。所以Vuex就允许我们将其分割成模块（modules），每个模块都拥有自己state，mutations,actions...。而Pinia每个状态库本身就是一个模块。</font>

### <font style="color:black;">Pinia</font>

<font style="color:black;">Pinia没有modules，如果想使用多个store，直接定义多个store传入不同的id即可，如：</font>

import { defineStore } from "pinia";

export const storeA = defineStore("storeA", {...});
export const storeB = defineStore("storeB", {...});
export const storeC = defineStore("storeB", {...});


### <font style="color:black;">Vuex</font>

<font style="color:black;">一般来说每个module都会新建一个文件，然后再引入这个总的入口index.js中，这里为了方便就写在了一起</font>

import { createStore } from "vuex";
const moduleA = {
  state: () => ({ 
    count:1
  }),
  mutations: {
    setCount(state, data) {
      state.count = data;
    },
  },
  actions: {
    getuser() {
      //do something
    },
  },
  getters: { ... }
}

const moduleB = {
  state: () => ({ ... }),
  mutations: { ... },
  actions: { ... }
}

export default createStore({
  strict: true,
  //全局state，类似于vue种的data
  state() {
    return {
      vuexmsg: "hello vuex",
      name: "xiaoyue",
    };
  },
  modules: {
    moduleA,
    moduleB
  },
});


<font style="color:black;">使用moduleA</font>

import { useStore } from 'vuex'
let vuexStore = useStore()
console.log(vuexStore.state.moduleA.count) //1
vuexStore.commit('setCount', 2)
console.log(vuexStore.state.moduleA.count) //2
vuexStore.dispatch('getuser')


<font style="color:black;">一般我们为了防止提交一些mutation或者actions中的方法重名，modules一般会采用命名空间的方式</font><font style="color:black;"> </font>**<font style="color:black;">namespaced: true</font>**<font style="color:black;"> </font><font style="color:black;">如moduleA：</font>

const moduleA = {
  namespaced: true,
  state: () => ({
    count: 1,
  }),
  mutations: {
    setCount(state, data) {
      state.count = data;
    },
  },
  actions: {
    getuser() {
      //do something
    },
  },
}


<font style="color:black;">此时如果我们再调用setCount或者getuser</font>

vuexStore.commit('moduleA/setCount', 2)
vuexStore.dispatch('moduleA/getuser')


## <font style="color:rgb(255, 255, 255);background-color:rgb(33, 33, 34);">写在最后</font>

<font style="color:black;">通过以上案例我们会发现Pinia比Vuex简洁许多，所以如果我们的项目是新项目的话建议使用Pinia。当然如果我们的项目体量不是很大，我们其实没必要引入vue的状态管理库，盲目的使用反而会徒增心智负担。</font>

<font style="color:black;">如果感觉这篇文章对你有帮助的话请点个赞吧orz</font>

<font style="color:rgb(0, 0, 0);">  
</font>


> 更新: 2022-11-11 10:30:12  
> 原文: <https://www.yuque.com/hutaoao/blog/pgk7ye>