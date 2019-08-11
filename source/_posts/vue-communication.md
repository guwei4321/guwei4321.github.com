title: vue组件通信
date: 2019-08-09 15:49:50
tags:
- Vue
categories:
- Vue
---

## 父子组件通信
### props / $emit
父组件通过`props`属性传值的方式给子组件，子组件通过`$emit`定义事件向父组件传递值。

#### 例子
使用一个简单的例子来验证下`props / $emit`，其中`List.vue`是`Bank.vue`的父组件。
````
// List.vue
<template>
  <div class="list">
    <bank-item :list="banks" @onEmitCurBankIndex="onEmitCurBankIndex">:</bank-item>
    <p>开户地：{{curCity}}</p>
  </div>
</template>

<script>
import bankItem from './Bank.vue'
export default {
  name: 'List',
  components: { bankItem },
  data() {
    return {
      banks: [
          {name: '众邦银行', info: '武汉'},
          {name: '营口银行', info: '营口'},
          {name: '金城银行', info: '天津'},
        ],
        curCity: ''
    }
  },
  methods: {
      onEmitCurBankIndex(index) {
          this.curCity = this.banks[index].info;
      }
  }
}
</script>


// Bank.vue
<template>
    <ul>
        <li v-for="(item, index) in list" :key="index" @click="emitCurBankIndex(index)">
            {{item.name}}
        </li>
    </ul>
</template>

<script>
export default {
  props: ['list'],
  methods: {
      emitCurBankIndex(index) {
          this.$emit('onEmitCurBankIndex', index)
      }
  }
}
</script>
````
注意：通过prop传递的数据，子组件只能使用，不能修改；而父组件也只能通过监听事件才能获取传递的值。


### ref / $refs
`ref`被用来给元素或者子组件注册引用信息。在父组件中可以使用`$refs`来获取引用信息。如果用在普通的DOM元素上，则引用信息指向DOM元素，如果是子元素，引用信息则指向子组件的实例。
#### 例子
我们拿`props / $emit`中的例子，当前银行索引从子元素中获取。
````
// List.vue
<template>
  <div class="list">
    <bank-item :list="banks" @onEmitCurBankIndex="onEmitCurBankIndex"  ref="bankItem">:</bank-item>
    <p>开户地：{{curCity}}</p>
  </div>
</template>

<script>
import bankItem from './Bank.vue'
export default {
  name: 'List',
  components: { bankItem },
  data() {
    return {
      banks: [
          {name: '众邦银行', info: '武汉'},
          {name: '营口银行', info: '营口'},
          {name: '金城银行', info: '天津'},
        ],
        curCity: ''
    }
  },
  methods: {
      // 父组件通过v-on监听并接受传参   
      onEmitCurBankIndex(index) {
          this.curCity = this.banks[index].info;
      }
  },
  mounted() {
      var bank = this.$refs.bankItem.curCity;
      this.curCity = this.banks[bank].info; // 默认选中通过$refs从子元素中获取
  }
}
</script>

// Bank.vue
<template>
    <ul>
        <li v-for="(item, index) in list" :key="index" @click="emitCurBankIndex(index)">
            {{item.name}}
        </li>
    </ul>
</template>

<script>
export default {
  props: ['list'],
  data(){
    return {
      curCity:0
    }
  },
  methods: {
        // 自定义时间，通过传参将值传给父组件 
        emitCurBankIndex(index) {
          this.$emit('onEmitCurBankIndex', index)
        }
  }
}
</script>
````

### $children / $parent
指定已创建的父实例，在两者之间建立父子关系。子实例可以用`this.$parent`访问父实例，子实例被推入父实例的`$children`数组中。`this.$parent`返回的是包含父组件实例的对象，而`$children`返回的是由该父组件所包含的所有子组件的数组。
#### 例子
使用一个简单的例子来验证下`$children / $parent`，其中`Parent.vue`是`Children.vue`的父组件。
````
// Parent.vue
<template>
    <div class="parent-el">
        <h1>我是父组件</h1>
        我是子组件的值{{tmp}}
        <children-a></children-a>
        <button @click="setChildtVal">点击改变子集的值</button>
    </div>
</template>

<script>
import ChildrenA from './ChildrenA.vue'
export default {
    name: 'Parent',
    components:{
        ChildrenA
    },
    data() {
        return {
            parentMsg: '我是父级的值',
            tmp: ''
        }
    },
    methods: {
        setChildtVal: function(){
            this.$children[0].childrenMsg = '改变子级的值'
        }
    }
}
</script>
// Children.vue
<template>
    <div>
        <h2>我是子组件</h2>
        {{childrenMsg}}
        <p>父组件的值：{{getParentVal}}</p>
    </div>
</template>

<script>
export default {
    data() {
        return {
            childrenMsg: '子组件的值'
        }
    },
    computed: {
        getParentVal: function(){
            return this.$parent.parentMsg
        }
    }
}
</script>
````
注意：官网建议要节制地使用`$parent`和`$children`- 它们的主要目的是作为访问组件的应急方法。更推荐用`props`和`events`实现父子组件通信。


### 父子通信小结
`$refs` 和 `$children` 都只有在组件渲染完成后才填充，都是在`mounted`之后才存在，所以

### 兄弟组件通信
父子通信可以通过官方的`API`一步来传递，但是兄弟间的数据传递要麻烦一点。得利用`vue`内部的一个事件机制，将其作为兄弟事件处理的中转站。

### 跨子传递
### provide/ inject
`provide/ inject` 是`vue2.2.0`新增的API。它解决了不管子组件嵌套多深，只要在父级组件中使用`provide`来提供变量，在子组件中就可以通过`inject`来注入变量。所以只要在祖先组件中使用`provide`定义了变量，子孙组件就能通过`inject`来获取，而不需要通过`prop`一层层的往下传递。

### 总结
1. `props / $emit`和`$children / $parent`只能解决父子组件的通信。
2. `$children`和`$refs`都不是响应式的，因此不能用它们在模板中做数据绑定