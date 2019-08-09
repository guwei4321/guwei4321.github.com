title: vue组件通信
date: 2019-08-09 15:49:50
tags:
- Vue
categories:
- Vue
---


## 父子间通信

````
// 父组件
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


// 子组件
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