# 基于响应式的简单计算器
```html
<template>
  <h1>{{message}}</h1>
  <p style="display:flex;align-items:center">
  	<button @click="addAndSub(true)">+1</button>
    	<span style="padding:0 10px">{{counter.count}}</span>
  	<button @click="addAndSub(false)">-1</button>
  </p>
</template>
```

## 选项式 Api
```html
<script>
import { ref, reactive } from 'vue'
export default {
  data() {
    return {
      message: ref('Hello,World'),
      counter: reactive({ count: 1 })
    }
  },
  methods: {
    addAndSub(type) {
      if (!type && this.counter.count === 1) {
        this.message = '不能再减了'
        return false
      }
      if (!type && this.counter.count > 1) this.counter.count--
      if (type && this.counter.count < 99) this.counter.count++
      this.message = this.counter.count
    }
  }
}
</script>
```

## 组合式Api
```html
<script setup>
import { ref, reactive } from 'vue'
const message = ref("Hello,World");
const counter = reactive({count:1})
let addAndSub = (type) => {
  let { count } = counter;
  if(!type && count == 1) {
    message.value = "不能再减了";
    return false;
  }
  if(!type && count > 1) count--;
  if(type && count < 99) count++;
  message.value = count;
  counter.count = count;
}
</script>
``` 

### setup 语法糖
`setup 语法糖` = `data()` + `methods` 
```html
<script>
import { ref, reactive } from 'vue'

export default {
  setup() {
      const message = ref('Hello,World')
      const counter = reactive({ count: 1 })

      function addAndSub(type) {
        if (!type && counter.count === 1) {
          message.value = '不能再减了'
          return false
        }
        if (!type && counter.count > 1) counter.count--
        if (type && counter.count < 99) counter.count++
        message.value = counter.count
      }
      return {
        message,
        counter,
        addAndSub
      }
  }
}
</script>
```