title: vue组件通信
date: 2019-08-09 15:49:50
categories:
- Vue
tags:
- Vue
- Vue实操
---

`Vue`组件是开发，又是靠数据去驱动视图更新的。所以在开发过程中，会经常遇到组件间的通信的问题。以下内容我们从 父子组件通信 、跨级传递 、兄弟组件通信三个方面来举例说明。

## 父子组件通信
### props / $emit
父组件通过`props`属性传值的方式给子组件，子组件通过`$emit`定义事件向父组件传递值。
<!--more-->

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

## 跨级传递
### provide/ inject
`provide/ inject` 是`vue2.2.0`新增的API。它解决了不管子组件嵌套多深，只要在父级组件中使用`provide`来提供变量，在子组件中就可以通过`inject`来注入变量。所以只要在祖先组件中使用`provide`定义了变量，子孙组件就能通过`inject`来获取，而不需要通过`prop`一层层的往下传递。
#### 例子
`Provider.vue`使用`provide`定义`title/titleA`属性，`ChildrenB.vue`通过`inject`获取`titleA`值，`ChildrenB.vue`通过`inject`获取`title`值。如下：
````
// Provider.vue
<template>
    <div>
        <p>父组件</p>
        <child-b></child-b>
    </div>
</template>

<script>
import ChildB from './ChildrenB.vue'
export default {
    provide: {
        title: '跨级传递',
        titleA: '子组件传递'
    },
    components: {
        ChildB
    }
}
</script>
// ChildrenB.vue
<template>
    <div>
        {{tittle}}
        <child-c></child-c>    
    </div>
</template>

<script>
    import ChildC from "./ChildrenC.vue"
    export default {
        name: 'ChildB',
        inject: [ 'titleA' ], // 获取父组件定义的titleA值
        components: {
            ChildC
        },
        data() {
            return {
                tittle: this.titleA
            }
        }
    }
</script>
// InjectChild.vue
<template>
    <div>
        {{title}}
    </div>
</template>

<script>
export default {
    name: 'grand',
    inject: ['title'], // 获取祖先组件定义的title值
    data() {
        return {
            titie: this.titie
        }
    }
}
</script>
````

### $attrs  $listeners
`provide/ inject`解决了`props`一级一级往下传递信息的问题，但是没解决子孙组件通过事件通信祖先组件也得一级一级网上传递。所以，在`vue2.4`中又引入了`$attrs` 和 `$listeners`。

#### 例子
`ChildrenD`组件调用`ChildrenE`组件时，使用`v-on`绑定了`$listeners`,`Ancestors`组件中能直接触发`ChildrenE`组件定义的的`postName`方法；`v-bind`绑定 `$attrs`属性，`ChildrenE`组件可以直接获取到`Ancestors`组件中传递下来的属性值。

````
// Ancestors.vue
<template>
    <div>
        <p>祖先组件</p>
        <child-d :titled="titleD" :titlee="titleE" v-on:postName="postName"></child-d>
    </div>
</template>

<script>
import ChildD from './ChildrenD'
export default {
    data() {
        return {
            titleD: '传递给childrenD的值',
            titleE: '传递给childrenE的值'
        }
    },
    components: {
        ChildD
    },
    methods: {
        postName(val) {
            console.log('从childrenE传递过来的值：' + val)
        }
    }
}
</script>
// ChildrenD.vue
<template>
    <div>
        <p>{{title}}</p>
        <child-e v-bind="$attrs" v-on="$listeners"></child-e>
    </div>
</template>

<script>
import ChildE from './ChildrenE.vue'
export default {
    data() {
        return {
            title: this.$attrs.titled
        }
    },
    components: {
        ChildE
    }
}
</script>
// ChildrenE.vue
<template>
    <div>
        <p>{{title}}</p>
        <input type="text" v-model="name" @input="postName(name)">
    </div>
</template>

<script>
export default {
    data(){
        return{
            name: '',
            title: this.$attrs.titlee
        }
    },
    methods: {
        postName(val){
            this.$emit('postName',val)
        }
    }
}
</script>
````

## 兄弟组件通信
没有`API`能直接实现兄弟组件的通信。

### 事件中转
父子通信可以通过官方的`API`一步来传递，但是兄弟间的数据传递要麻烦一点。得利用`vue`内部的一个事件机制，将其作为兄弟事件处理的中转站。
#### 例子
将`EventBus`定义在父级元素，这样子的话子元素可以直接获取到时间对象。
````
//EventBus.vue
<template>
    <div>
        <brother-a></brother-a>
        <brother-b></brother-b>
    </div>
</template>
<script>
import Vue from 'vue'
import BrotherA from './BrotherA.vue'
import BrotherB from './BrotherB.vue'

const EventBus = new Vue()
Vue.prototype.$EventBus = EventBus; // 将vue内部的事件赋值给vue的原型

export default {
    data() {
        return {

        }
    },
    components: {
        BrotherA,
        BrotherB
    }
}
</script>  
//BrotherA.vue
<template>
    <div>
        <input type="text" v-model="title" @input="passTitle(title)">
    </div>  
</template>

<script>
export default {
    data() {
        return {
            title: ''
        }
    },
    methods: {
        passTitle(val) {
            this.$EventBus.$emit('passTitle', val) // 子元素获取到$EventBus对象，然后定义事件名passTitle
        }
    }
}
</script>
//BrotherB.vue
<template>
    <div>
        <h4>BrotherB组件</h4>
        <h4>BrotherA组件传递过来的值：{{title}}</h4>
    </div>
</template>
<script>
export default {
    data(){
        return {
            title: ''
        }
    },
    mounted() {
        this.$EventBus.$on('passTitle', (val)=> { // 接受passTitle事件
            this.title = val
        })
    }
}

</script>
````
当然也可以定义一个`eventBus.js`
````javascript
import Vue from 'vue'
export const EventBus = new Vue()
````
然后每次使用引入使用
````
import { EventBus } from './event-bus.js'
````

### Vuex
`Vuex`是一个专为 Vue.js 应用程序开发的状态管理模式。具体怎么使用移步[官网](https://vuex.vuejs.org/zh/)。

### 兄弟组件通信小结
事件中转和`Vuex`能实现跨级通信，所以父子间通信也是能实现的，但是相比于其他直接能实现父子通信的`API`，用事件中转和`Vuex`去实现就有点不够优雅了。

### 总结
1. `props / $emit`和`$children / $parent`只能解决父子组件的通信。
2. `$children`和`$refs`都不是响应式的，因此不能用它们在模板中做数据绑定。
3. `Vuex`能解决所有关系组件通信问题，但是要根据实际项目情况去使用。