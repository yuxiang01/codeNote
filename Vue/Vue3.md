# 一、邂逅 `Vue.js` 开发
- 使用 Vue 这个 js 库开发方式
	- 方式一：在页面中通过 CDN 的方式来引入；
		![[Pasted image 20230207224418.png]]
	- 方式二：下载 Vue 的 JavaScript 文件，并且自己手动引入；
		 ![[Pasted image 20230207224433.png]]
	- 方式三：通过 npm 包管理工具 or 脚手架工具安装使用它
		[[Vue study#基于响应式的简单计算器]]
- Vue 单文件组件 (Single-File Component，缩写为 SFC)
	- SFC 是一种可复用的代码组织形式，它将从属于同一个组件的 HTML、CSS 和 JavaScript 封装在使用 `.vue` 后缀的文件中
- 命令式编程和声明式编程
	- 通过 JavaScript 编写一条代码，来给浏览器一个指令
		- 这样的编写代码的过程，我们称之为**命令式编程**
			- 命令式编程关注的是 “how to do”自己完成整个 how 的过程
	- 在 createApp 传入的对象中声明需要的内容，模板 template、数据 data、方法 methods
		- 这样的编写代码的过程，我们称之为是**声明式编程**
			- 声明式编程关注的是 “what to do”，由框架(机器)完成 “how”的过程
## Vue 的 options 
### data 属性
- data 属性是传入一个函数，并且该函数需要返回一个对象：
	- 在 Vue2.x 的时候，也可以传入一个对象（虽然官方推荐是一个函数）
	- 在 Vue3.x 的时候，必须传入一个函数，否则就会直接在浏览器中报错
- data 中返回的对象会被 Vue 的响应式系统劫持，之后对该对象的修改或者访问都会在劫持中被处理：
	- 所以我们在 template 或者 app 中通过 {{counter}} 访问 counter，可以从对象中获取到数据；
	- 所以我们修改 counter 的值时，app 中的 {{counter}} 也会发生改变；
### methods 属性
- methods 属性是一个对象
	- 这些方法可以被绑定到模板中；
	- 在该方法中，我们可以使用 this 关键字来直接访问到 data 中返回的对象的属性
	![[Pasted image 20230208001922.png]]
# 二、Vue 基础-模板语法 & vue 指令  
## 2.1 Mustache 双大括号语法

使用最多的语法是 “Mustache”语法 (双大括号) 的文本插值

![[Pasted image 20230208002723.png]]
![[Pasted image 20230208002743.png]]
另外这种用法是错误的: 
![[Pasted image 20230208002815.png]]

## 2.2 v-once 指令, 只渲染一次
- 当数据发生变化时，元素或者组件以及其所有的子元素将视为静态内容并且跳过；
- 该指令可以用于性能优化
- 同时子节点也是只渲染一次 
```html
<script setup>
import { ref } from "vue"

const count = ref(0)

setInterval(() => {
  count.value++
}, 1000)
</script>

<template>
  <span v-once>使它从不更新: {{ count }}</span>
</template>
```

## 2.3 v-text 指令, 等价于双括号
用于更新元素的 textContent
![[Pasted image 20230208003607.png]]

## 2.4 v-html 指令, 插入标签元素
- 默认情况下，如果我们展示的内容本身是 html 的，那么 vue 并不会对其进行特殊的解析。
- 如果我们希望这个内容被 Vue 可以解析出来，那么可以使用 v-html 来展示
![[Pasted image 20230208003903.png]]

## 2.5 v-pre 指令, 略过元素渲染
- v-pre 用于跳过元素和它的子元素的编译过程，显示原始的 Mustache 标签
- 跳过不需要编译的节点，加快编译的速度
```html
<h1 v-pre>{{ message }}</h1>
```
## 2.6 v-cloak 指令, 隐藏未编译
![[Pasted image 20230208004857.png]]
## 2.7 v-memo 指令, 优化渲染性能
```html
<div v-for="item in list" :key="item.id" v-memo="[item.id === selected]">
  <p>ID: {{ item.id }} - selected: {{ item.id === selected }}</p>
  <p>...more child nodes</p>
</div>
```
- 当组件的 selected 状态改变，默认会重新创建大量的 vnode，
	- 尽管绝大部分都跟之前是一模一样的
- v-memo 用在这里本质上是在说“只有当该项的被选中状态改变时才需要更新”
	- 这使得, 每个选中状态没有变的项, 能完全重用之前的 vnode 并跳过差异比较
		- 从而达到性能优化的效果

## 2.8 v-bind 的绑定属性 `:`
### 绑定基本属性
v-bind 用于绑定一个或多个属性值，或者向另一个组件传递 props 值
![[Pasted image 20230208090441.png]]

### 绑定 class
- 对象语法：我们可以传给 `:class` (v-bind: class 的简写) 一个对象，以动态地切换 class。
	![[Pasted image 20230208091540.png]]
- 数组语法：我们可以把一个数组传给 : class，以应用一个 class 列表
	![[Pasted image 20230208091602.png]]

### 绑定 style
- 对象语法
	![[Pasted image 20230208092527.png]]
- 数组语法
	![[Pasted image 20230208095532.png]]
	```html
    <template>
        <div :class = "[flag ? 'one' : 'two', 'three']"></div>
        <h2 :style="[objStyle, { backgroundColor: 'purple' }]">嘿嘿嘿嘿</h2>
    </template>
    
    <script setup lang="ts">
        const flag: boolean = false;
        const objStyle = {fontSize: '50px',color: "green"};
    </script>
    
    <style>
        .one {
            color: red;
        }
        .two {
            color: blue;
        }
        .three {
            height: 300px;
            weight: 300px;
            border: 1px solid #ccc;
        
    </style>
	```

### 动态绑定属性
如果属性名称不是固定的，我们可以使用 :[属性名]=“值” 的格式来定义, 这种绑定的方式，我们称之为动态绑定属性
![[Pasted image 20230208092908.png]]

### 绑定一个对象
将一个对象的所有属性，绑定到元素上的所有属性, 可以直接使用 v-bind 绑定一个对象.
```html
<template>
  <h2 :name="name" :age="age" :height="height">Hello World</h2>

  <!-- v-bind绑定对象: 给组件传递参数 -->
  <h2 v-bind="infos">Hello Bind</h2>

  <!-- 动态绑定属性 -->
  <h2 :[name]="name">动态绑定属性</h2>
</template>

<script setup>
// 组合式Api
const name = 'why'
const age = 18
const height = 1.88
const infos = { name: 'why', age: 18, height: 1.88, address: '广州市' }
</script>
```
![[Pasted image 20230208094558.png]]

## 2.9 v-on 事件绑定 `@`
```html
<!--v-on修饰符 ---冒泡案列-->
-------------------------------------------------
<template>
    <div @click = "parent">
        <div @click.stop = "child">Child</div>
    </div>
</template>

<script setup lang="ts">
    const child = () => {
        console.log('child');
    }
    const parent = () => {
        console.log('parent');
    }
</script>
------------------------------------------------
1.stop ==> 阻止冒泡
2.prevent ==> 阻止提交
```

## 2.10 v-for 指令, 列表渲染
```html
<template>
    <div v-for="(item,index) in list">
	    {{item.name}}, 今年{{item.age}}了
    </div>
    
    <!-- 2.遍历对象 -->
    <ul>
      <li v-for="(value, key, index) in info">
	      {{value}}-{{key}}-{{index}}
      </li>
    </ul>

    <!-- 3.遍历字符串(iterable) -->
    <ul>
      <li v-for="item in message">{{item}}</li>
    </ul>

    <!-- 4.遍历数字 -->
    <ul>
      <li v-for="item in 100">{{item}}</li>
    </ul>
</template>

<script setup lang="ts">
    type List = {
      age: number
      name: string
    }
    type Info = {
	  name: String
	  age: number
	  height: number
    }
    const list: List[] = [
      { name: '刘莺莺', age: 19 },
      { name: '王冰冰', age: 22 },
      { name: '范冰冰', age: 30 },
      { name: '刘亦菲', age: 18 }
    ]
    const message:String = "Hello Vue";
    const info:Info = { name: "why", age: 18, height: 1.88 };
</script>
```

>[!note] - v-for 中的 key 是什么作用?
> 	- key 属性主要用在 Vue 的**虚拟 DOM 算法**，在**新旧 nodes** 对比时辨识 **VNodes**；
> 	- 如果不使用 key，Vue 会使用一种最大限度减少动态元素并且尽可能的尝试就地**修改/复用相同类型元素**的算法；
> 	- 而**使用 key** 时，它会基于 key 的变化**重新排列元素顺序**，并且会**移除/销毁 key** 不存在的元素
> - VNode 的全称是 Virtual Node，也就是虚拟节点 
> 	- 事实上，无论是组件还是元素，它们最终在 Vue 中表示出来的都是一个个 VNode；
> 	- VNode 的本质是一个 JavaScript 的对象
> 	- 多个 VNode 树化, 形成一个 VNode Tree
## 2.11 v-show 指令, 显示和隐藏
控制元素的显示和隐藏 => `display: none` block css 切换 

## 2.12 v-if 指令, 显示与隐藏
- 控制元素的显示/隐藏 => 注释 => 频繁切换浪费性能
- v-if 能使得组件重新渲染和销毁

## 2.13 v-else-if
- v-if 的“else if 块”。可以链式调用

## 2.14 v-else
v-if 条件收尾语句

## 2.15 template 元素
类似于 v-if，你可以使用 template 元素来循环渲染一段包含多个元素的内容
![[Pasted image 20230208103609.png]]
```html
<template>
  <el-button @click="toggle">切换</el-button>
  <template v-if="flag">
    <img src="https://game.gtimg.cn/images/wzry_qrcode.jpg"/>
  </template>
</template>

<script setup>
import { ref } from 'vue'
const flag = ref(false)
let toggle = () => (flag.value = !flag.value)
</script>
```

## 2.16 数组更新检测
- Vue 将被侦听的数组的变更方法进行了包裹，所以它们也将会触发视图更新
	- `push()`
	- `pop()`
	- `shift()`
	- `unshift()`
	- `splice()`
	- `sort()`
	- `reverse()`
- 替换数组的方法
	- 上面的方法会直接修改原来的数组
	- 但是某些方法不会替换原来的数组，而是会生成新的数组
		- 比如 `filter()`、`concat()` 和 `slice()`

## 2.17 computed 计算属性
![[学习Vue3#学习Vue3 第九章（认识computed计算属性）]]
- 计算属性值会基于其响应式依赖被缓存 (computed 计算属性是有缓存的)
	- 也就是说只要不是响应式 (ref、reactive) 动态改变, computed 计算属性值不会发生改变

## 2.18 watch 侦听器
> - 侦听一个或多个响应式数据源，并在数据源变化时调用所给的回调函数
>	- 监听数据改变, 动态...
- `watch(source,callback,options)`
	- `source 数据源`
		- 可以是不同形式的“数据源”
			- 一个 ref (包括计算属性)、一个响应式对象、一个 getter 函数、或多个数据源组成的数组
	- `callback(新值,旧值,onCleanup()) 回调函数`
		- `onCleanup()` 用于注册副作用清理的回调函数
			- 该回调函数会在副作用下一次重新执行前调用，
			   - 用来清除无效的副作用，例如<i>等待中的异步请求</i>
	- `options 参数(可选)`
		- 第三个可选的参数是一个对象
			- immediate：在侦听器创建时立即触发回调。第一次调用时旧值是 undefined。
			- deep：如果源是对象，强制深度遍历，以便在深层级变更时触发回调。参考深层侦听器。
			- flush：调整回调函数的刷新时机。参考回调的刷新时机及 watchEffect ()。
			- onTrack / onTrigger：调试侦听器的依赖。参考调试侦听器。
- 解构 Proxy 对象
	```html
	<script>
	  // option api
	  data() {
	        return {
	          message: "Hello Vue",
	          info: { name: "why", age: 18 }
	        }
	  },
	  watch: {
	    // 默认watch监听不会进行深度监听
	    info(newValue,oldValue) {
	      // 获取原生对象
	      console.log(Vue.toRaw(newValue)); // 对象本身
	      console.log({...newValue}); // 解构成了新对象
	    }
	  }
	</script>
	```
- watch 侦听选项
	```js
	// data: option api
	data() {
	  return {
	    info: { name: "why", age: 18 }
	  }
	},
	methods: {
	  changeInfo() {
	    // 1.创建一个新对象, 赋值给info
	    // this.info = { name: "kobe" }
	    // 2.直接修改原对象某一个属性
	    this.info.name = "kobe"
	  }
	},
	watch: {
	  // 进行深度监听
	  info: {
	    handler(newValue, oldValue) {
	      console.log("侦听到info改变:", newValue, oldValue)
	      console.log(newValue === oldValue)
	    },
	    // 监听器选项:
	    // info进行深度监听
	    deep: true,
	    // 第一次渲染直接执行一次监听器
	    immediate: true
	  },
	  "info.name": function(newValue, oldValue) {
	    console.log("name发生改变:", newValue, oldValue)
	  }
	}
	```

## 2.19 v-model 双向数据绑定
### v-model 的原理
- v-bind 绑定 value 属性的值；
- v-on 绑定 input 事件监听到函数中，函数会获取最新的值赋值到绑定的属性中
	![[Pasted image 20230226215348.png]]
### V-model 的使用
- 绑定 input 输入框
	```html
	<template>
		<input type="text" v-model="counter.count" @blur="verification" />
	</template>
	<script setup lang="ts">
		import { reactive } from 'vue'
		const counter = reactive({ count: 1 })
		const verification = () => {
		  if (!(counter.count > 1)) counter.count = 1
		}
	</script>
	```
- 绑定 textarea
	![[Pasted image 20230226220022.png]]
- v-model 绑定 checkbox (`属性name` 替换成 `v-model`)
	- 单个 checkbox 可不加 `value 属性` 
	- 多个 checkbox 必须要加
- v-model 绑定 radio，用于选择其中一项
	![[Pasted image 20230226220426.png]]
- v-model 绑定 select
	- 单选：只能选中一个值
		 ![[Pasted image 20230226222947.png]]
	- 多选：可以选中多个值 (数组)
		 ![[Pasted image 20230226222954.png]]
### v-model 的修饰符
- `lazy`: 从绑定 `@input` 改成绑定 `@change` 事件
	- 默认情况下，v-model 在进行双向绑定时，绑定的是 input 事件
		- 那么会在每次内容输入后就将最新的值和绑定的属性进行同步
	- 添上 lazy 修饰符，那么会将绑定的事件切换为 change 事件
		- 只有在提交时（比如回车、失去焦点）才会触发 
- `number`: 隐式将数据转换成 number 类型
- `trim`: 去除前后空格
# 三、深入 vue 组件
## 3.1 组件基础
组件允许我们将 UI 划分为独立的、可重用的部分，并且可以对每个部分进行单独的思考。
在实际应用中，组件常常被组织成层层嵌套的树状结构：
![[Pasted image 20230226232204.png]]
## 3.2 组件的生命周期
![[学习Vue3#2.组件的生命周期]]
## 3.3 注册组件
### 全局注册
```js
import MyComponent from './App.vue'

// app.component() 方法可以被链式调用
app.component('MyComponent', MyComponent)
```
全局注册的组件可以在此应用的任意组件的模板中使用 

- 出现的问题: 
	1. 打包时全局组件, 即使从没使用过, 也会生成相应文件 
	2. 全局组件会使得组件间的依赖关系混乱, 不利于后期维护
### 局部注册
```html
<script setup>
import ComponentA from './ComponentA.vue'
</script>

<template>
  <ComponentA />
</template>
```
请注意：**局部注册的组件在后代组件中并不可用**
## 3.4 组件间通信
![[vue3-父子组件传参.excalidraw | 500]]

### 1. 传递 Props (父传子参) 
[详情](https://cn.vuejs.org/guide/components/props.html#prop-passing-details)
```ts
export interface product {
  name: string
  price: number
  img: string
}
```
#### 父组件
```html
<template>
  <div id="productList">
    <ul id="list">
      <li v-for="(item, index) in data" :key="index">
	    <!--<Product :name="item.name" :img="item.img" :price="item.price"/>-->
        <Product :data="item" />
      </li>
    </ul>
  </div>
</template>

<script setup lang="ts">
import { reactive } from 'vue'
import Product from './Product.vue'
import { product } from './product'
const data = reactive<product[]>([
  { name: '内墙漆', price: 41.5, img: 'neiqiang.jpeg' },
  { name: '多功能排插', price: 52.0, img: 'paichafanglei.jpg' },
  { name: '万用表', price: 32.5, img: 'wyb.jpeg' },
  { name: '金杯电线', price: 75.5, img: 'xinlan.jpeg' }
])
</script>
```

#### 子组件
```html
<template>
  <div id="product">
    <img :src="getImgUrl(data.img)" alt="" width="100" height="100" />
    <p>{{ data.name }}</p>
    <p class="price"><span class="symbol">¥</span>{{ data.price.toFixed(1) }}</p>
  </div>
</template>

<script setup lang="ts">
import { product } from './product'

// 接收一个对象,对象是product => 传递一个data且符合product类型的对象
defineProps<{ data: product }>()

/* 
接收product类型 => 直接传递product类型(即name、price、img三个字段)
	interface product {
	  name: string
	  price: number
	  img: string
	}
	defineProps<product>()
*/

const getImgUrl = (url: string) => {
  return '/src/assets/product/' + url
}
</script>
```

注意: *传递给 defineProps 的<u>泛型参数本身</u>不能是一个导入的类型*

### 2. 监听事件 emit (子传父参) 
`emit(事件名, ...可变参数)`
#### 子组件
```html
<template>
  <div id="input_number">
    <button @click="addAndSub(true)">+1</button>
    <span>
      <input type="text" v-model.number="counter.count" @blur="verification" />
    </span>
    <button @click="addAndSub(false)">-1</button>
  </div>
</template>

<script setup lang="ts">
import { reactive, watch } from 'vue'
const emit = defineEmits(['response'])
const counter = reactive({ count: 1 })
watch(counter, () => {
  emit('response', counter.count)
})
</script>
```
#### 父组件
```html
<template>
	<p><InputNumber :num="count" @response="(num) => (count = num)" /></p>
	<p>{{ count }} —— {{ typeof count }}</p>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import InputNumber from '@/components/InputNumber.vue'
const count = ref<number>(2)
</script>
```

### 案例: 购物车、步进器 (父传子, 子传父)
> - 单向数据流
> 	- 所有的 props 都遵循着单向绑定原则，props 因父组件的更新而变化，自然地将新的状态向下流往子组件，而不会逆向传递

`props 属性值` 是只读的

父组件传递 `props` 作为初始值, 子组件通过 `emit` 更新值

```html
<!--子组件-->
<template>
  <div id="input_number">
    <button @click="addAndSub(true)">+1</button>
    <span>
      <input type="text" v-model.number="counter.count" @blur="verification" />
    </span>
    <button @click="addAndSub(false)">-1</button>
  </div>
</template>

<script setup lang="ts">
import { reactive, watch } from 'vue'
const props = defineProps<{ num: number }>()
const emit = defineEmits(['response'])
const counter = reactive({ count: props.num })
let addAndSub = (type: boolean) => {
  if (!type) {
    if (counter.count <= 1) return
    counter.count--
  } else {
    counter.count++
  }
}
const verification = () => {
  if (!(counter.count > 1)) counter.count = 1
}
watch(counter, () => {
  emit('response', counter.count)
})
</script>
```

```html
<!--父组件-->
<template>
  <div id="cart">
    <table border="1" cellpadding="0" cellspacing="0" width="660">
      <thead>
        <tr>
          <th>名称</th>
          <th>数量</th>
          <th>单价</th>
          <th colspan="2">操作</th>
        </tr>
      </thead>
      <tbody align="center">
        <tr v-for="(item, index) in data" :key="index">
          <td>{{ item.name }}</td>
          <td>{{ item.num }}</td>
          <td>¥ {{ item.price }}</td>
          <td>
	        <!--:key="item.name"-->
            <InputNumber :num="item.num" @response="(num) => (item.num = num)" />
          </td>
          <td><a href="#" @click.stop="del(item)">删除</a></td>
        </tr>
      </tbody>
      <tfoot align="center">
        <td colspan="2">总价</td>
        <td colspan="3">{{ tweened.$total.toFixed(2) }}</td>
      </tfoot>
    </table>
  </div>
</template>

<script setup lang="ts">
import { computed, reactive, ref, watch } from 'vue'
import gsap from 'gsap'
import InputNumber from '../components/InputNumber.vue'
type Shop = {
  name: string
  num: number
  price: number
}
let data = ref<Shop[]>([
  {
    name: '香蕉',
    num: 2,
    price: 25.5
  },
  {
    name: '汉堡包',
    num: 1,
    price: 10.5
  },
  {
    name: '苹果',
    num: 4,
    price: 18.4
  }
])
let $total = ref(0)
$total = computed<number>(() => total())
const total = () => {
  return data.value.reduce((prev, next) => {
    return prev + next.num * next.price
  }, 0)
}
const tweened = reactive({ $total: total() })
watch($total, (n) => {
  gsap.to(tweened, { duration: 0.6, $total: Number(n) || 0 })
})
const del = (item: Shop) => {
  data.value = data.value.filter((t) => t !== item)
}
</script>
```

- **出现 Bug**: 当删除一项时，`counter.count` 的值会顺延到下一项
	- 因为 `InputNumber` 组件的 `num` 属性是通过父组件的 `data` 数组中对应项的 `num` 字段传入的
		- 而 `counter` 对象中的 `count` 属性则是通过 `v-model.number` 双向绑定的。
	- 所以，当您删除某一项时，`data` 数组中对应项的 `num` 字段被删除，`counter.count` 的值并不会随之改变
		- 因此 `InputNumber` 组件的 `num` 属性会传入下一项的 `num` 字段的值，导致 `counter.count` 的值顺延到下一项。
- **解决**: 父组件中给 `InputNumber` 组件添加一个唯一的 `key` 属性来解决问题
	- 将 `InputNumber` 组件的 `key` 属性绑定到 `item.name`
		- 为什么不能使用 `index` 作为 `key` 值?
			- 因为删除会对数组的索引造成索引始终不改变, diff 算法导致组件不更新
	- 这样在删除一项时，`InputNumber` 组件的 `key` 属性会随之改变，从而强制重新创建一个新的 `InputNumber` 组件实例

![[学习Vue3#学习Vue3 第二十四章（兄弟组件传参和Bus）]]

### 3.`v-model `语法糖——父子组件通信 
[[学习Vue3#2.v-model(是props 和 emit的组合) | v-model(是props 和 emit的组合]]

```html
<!--父组件-->
<template>
    <td>
        <Stepper v-model:num="item.num" :key="item.name" />
    </td>
</template>

<!--子组件-->
<template>
  <div id="input_number">
    <button @click="addAndSub(true)">+1</button>
    <span>
      <input type="text" v-model.number="counter.count" @blur="verification" />
    </span>
    <button @click="addAndSub(false)">-1</button>
  </div>
</template>

<script setup lang="ts">
import { reactive, watch } from 'vue'
const props = defineProps<{ num: number }>()
const emit = defineEmits(['update:num'])
const counter = reactive({ count: props.num })
watch(counter, () => {
  emit('update:num', counter.count)
})
</script>
```

## 3.5 通过插槽来分配内容
![[学习Vue3#学习Vue3 第十七章（插槽slot）]]

## 3.6 依赖注入
![[学习Vue3#学习Vue3 第二十三章（依赖注入Provide / Inject）]]
## 3.7 动态组件
[[学习Vue3#学习Vue3 第十六章（动态组件）| 第十六章（动态组件）]]
```html
<!-- 父组件 -->
<template>
  <div id="tabview">
    <TabControl :list="data" @tab-item-click="tabItemClick" />
    <div class="page-content">
      <component :is="page" />
    </div>
  </div>
</template>

<script setup lang="ts">
import { List } from '@/components/Tab/tab'
import TabControl from '@/components/Tab/TabControl.vue'
import { markRaw, reactive, ref } from 'vue'
import Clothes from '@/view/dynamicView/Clothes.vue'
import Shoes from '@/view/dynamicView/Shoes.vue'
import Pants from '@/view/dynamicView/Pants.vue'

const data = reactive<List[]>([
  { name: '热门', url: markRaw(Clothes) },
  { name: '畅销', url: markRaw(Shoes) },
  { name: '推荐', url: markRaw(Pants) }
])
const page = ref<any>(markRaw(Clothes))
const tabItemClick = (index: number) => {
  page.value = data[index].url
}
</script>
```

```html
<!-- 子组件 -->
<template>
  <div class="tab">
    <div class="tab-head">
      <div
        class="tab-item"
        :class="{ active: index === currentIndex }"
        v-for="(item, index) in list"
        :key="index"
        @click.stop="liClick(index)"
      >
        <span>{{ item.name }}</span>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { List } from './tab'
defineProps<{ list: List[] }>()
const emit = defineEmits(['tab-item-click'])
const currentIndex = ref(0)
const liClick = (index: number) => {
  currentIndex.value = index
  emit('tab-item-click', index)
}
</script>
```

## 3.8 keep-alive (组件缓存)
[[学习Vue3#学习Vue3 第二十章（keep-alive缓存组件）]]

- 案例: 在路由视图下, 只缓存 Home 组件
	- 实现: 使用 keep-alive 组件缓存 + component 动态组件
	```html
	<RouterView v-slot="props">
	    <KeepAlive include="Home">
	      <component :is="props.Component" />
	    </KeepAlive>
	</RouterView>
	```

>[!warning]- 注意: include 包含指定的组件名的大小写
>- `<KeepAlive/>` 会根据组件的 name 选项进行匹配, <u>区分大小写</u>
>- 在 3.2.34 或以上的版本中，使用 `<script setup>` 的单文件组件
>	- 会自动根据文件名生成对应的 name 选项，无需再手动声明。

# 四、(组合式)响应式 api 
- 能在改变时触发更新的状态被称作是**响应式**的 
	- `reactive()` API 来声明响应式状态
		- 由 `reactive()` 创建的对象都是 JavaScript [Proxy](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy)，其行为与普通对象一样
		- 但 `reactive()` 只适用于对象 (包括数组和内置类型，如 `Map` 和 `Set`)
		> 返回一个对象的响应式代理
	- 而 `ref()` 则可以接受任何值类型
		- `ref` 会返回一个包裹对象，并在 `.value` 属性下暴露内部值
		> 接受一个内部值，返回一个响应式的、可更改的 ref 对象，此对象只有一个指向其内部值的属性 `.value`
	-  应用实例
		- 在模板中访问 `message` ref 时不需要使用 `.value` 它会被自动解包
		- 通过 ES6 解耦的值不会影响到解耦本身，因为它们是独立的副本
			- 当你使用 ES6 解构语法解耦一个对象时，你实际上是在创建一个新的变量，该变量的值是对象的属性的副本。
				- 当你更改解耦的值时，不会影响到原始对象
		-  [[Vue study#基于响应式的简单计算器|应用实例: 基于响应式的简单计算器]]
## 4.1 reactive vs ref
- Vue 3 中的 `reactive` 和 `ref` 是 Vue 的两个辅助函数，用于在函数式组件中管理状态 
	- reactive 
		- reactive 的参数一般是**对象或者数组**, 他能够将复杂数据类型变为响应式数据。
		- reactive 的响应式是深层次的，底层本质是将传入的数据转换为 Proxy 对象
	- ref
		- ref 的参数一般是基本数据类型，也可以是对象类型
		- 如果参数是对象类型，其实底层的本质还是 reactive，系统会自动将 ref 转换为 reactive
			- 例如 `ref(1) ===> reactive({value:1})`
		- 在模板中访问 ref 中的数据，系统会自动帮我们添加 `.value`
			- 但在 JS 中访问 ref 中的数据，需要手动添加 `.value`
		- ref 的底层原理同 reactive 一样，都是 Proxy
- `reactive` 和 `ref` 使用场景
	1. reactive 的应用场景
		1. 条件一: reactive 应用于本地的数据
		2. 条件二: 多个数据之间是有关系/联系 (聚合的数据, 组织在一起会有特定的作用)
	2. ref 的应用场景: 其他的场景基本都用 ref (computed)
		1. 定义本地的一些简单数据
		2. 定义从网络中获取的数据也是使用 ref

## 4.2 ref 引用组件
- 基本使用
	```html
	<script setup lang="ts">
	const homeRef = ref()
	onActivated(() => {
	  homeRef.value?.scrollTo({
	    top: scrollTop.value
	  })
	})
	</script>
	
	<template>
	  <div id="home" ref="homeRef">
	</template>
	```

- `ref` 引用的是组件
	- 获取元素: 通过 `$el` 获取组件的根元素
	- 访问属性/对象/方法: 需要引用组件暴露 `defineExpose`,才能访问

## 4.3 响应式原理


# 五、Router 路由
* 安装
	```shell
	npm install vue-router@4
	```
* 使用:
	* 通过 createRouter 创建 router 对象, 并且传入 routes 和 history 模式
	    * routes:  配置映射关系
	    * history: 指定采用的模式  hash 模式和 history 模式
	      * hash 模式: `history: createWebHashHistory()`
	      * history 模式: `history: createWebHistory()` 
	* `app.use(router)`使用 app 注册路由对象  
	* 使用路径:
	    * router-view: 占位
	    * router-link  进行路由的切换
	      * 编程式导航
	      * to 属性, 跳转到哪一个路由
- 什么是前端路由？前端路由的发展历程是怎么样的？
	- 前端路由: 
		- 由前端来维护映射关系, 在不同的 URL 显示不同的组件
	- 发展历程:
		- 后端路由: 
			- 当我们页面中需要请求不同的路径内容时, 交给服务器来进行处理, 服务器渲染好整个页面, 并且将页面返回给客户端.
		* 前后端分离: 
			* 后端只提供 API 来返回数据，前端通过 Ajax 获取数据，并且可以通过 JavaScript 将数据渲染到页面中
	* 单页面富应用
		* SPA: single page web application
		    * SPA 最主要的特点就是在前后端分离的基础上加了一层前端路由.
		* 前端路由
		    * 由前端来维护映射关系, 在不同的 URL 显示不同的组件
		    * 核心 :  改变 URL，但是页面不进行整体的刷新
		    * 监听 URL 的改变
## 5.1 不同的历史模式
在创建路由器实例时，`history` 配置允许我们在不同的历史模式中进行选择。

### Hash 模式
hash 模式是用 `createWebHashHistory()` 创建的
```js
import { createRouter, createWebHashHistory } from 'vue-router'

const router = createRouter({
  history: createWebHashHistory(),
  routes: [
    //...
  ],
})
```
它在内部传递的实际 URL 之前使用了一个哈希字符（`#`）。由于这部分 URL 从未被发送到服务器，所以它不需要在服务器层面上进行任何特殊处理

**弊端:** 不利于 SEO

### History (HTML5) 模式
用 `createWebHistory()` 创建 HTML5 模式 **`推荐`**
```js
import { createRouter, createWebHistory } from 'vue-router'

const router = createRouter({
  history: createWebHistory(),
  routes: [
    //...
  ],
})
```
**弊端:** 地址可控, 有可能出现 404 

### Memory 模式
用 `createMemoryHistory()` 创建内存模式
```js
import { createRouter, createMemoryHistory } from 'vue-router'

const router = createRouter({
  history: createMemoryHistory(),
  routes: [
    //...
  ],
})
```
**弊端:** 路由地址不可控, 适用于 SSR 服务端渲染

## 5.2 动态路由匹配
|             匹配模式              |         匹配路径         |              $route.Params              |
|:---------------------------------:|:------------------------:|:----------------------------------------:|
|        `/users/:username`         |      `/users/eduardo`      |        `{ username: 'eduardo' }`         |
| `/users/: username/posts/:postId` | `/users/eduardo/posts/123` | `{ username: 'eduardo', postId: '123' }` |


```js
// router/index.js
export const router = createRouter({
  history: createWebHistory(),
  routes: [
    {
      path: '/users/:username',
      component: User,
      children: [
        // 匹配 '/users/:username' 时, 组件UserHome将在User的＜router-view＞中呈现
        { path: '', component: UserHome },
        
        // 匹配 /users/:username/profile 时
        { path: 'profile', component: UserProfile },
        
        // 匹配 /users/:username/posts 时
        { path: 'posts', component: UserPosts },
      ],
    },
  ]
})
```

RouterView
```html
<template>
  <div class="user">
    <h2>User {{ $route.params.username }}</h2>
    <router-view></router-view>
  </div>
</template>

<template>
  <div>Home/Posts/Profile</div>
</template>
```

App.vue
```html
<template>
  <h1>Nested Views</h1>
  <p>
    <router-link to="/users/fishx">/users/fishx</router-link>
	<router-link to="/users/fishx/profile">/users/fishx/profile</router-link>
    <router-link to="/users/fishx/posts">/users/fishx/posts</router-link>
  </p>
  <router-view></router-view>
</template>
```

- 获取路由参数 `/input/:id` => `/input/12`
	```html
	<script setup lang='ts'>
	 import { useRoute } from 'vue-router' 
	 const route = useRoute()
	 const id = route.params.id
	</script>
	
	<template>
	  <div>模版中获取路由参数: {{ $route.params.id }}</div>
	</template>
	
	<style scoped lang='less'></style>
	```

## 5.3 捕获 404 
```js
const router = createRouter({
  history: createWebHashHistory(),
  routes: [
    {
      path: '/:pathMatch(.*)',
      name: '404',
      component: () => import('@/view/NotFound.vue')
    }
  ]
})
```

- 匹配规则加 `*`
	- `path: '/:pathMatch(.*)*'`
	- 对 url 做解析
		![[Pasted image 20230301144753.png]]

## 5.4 编程式导航

```html
<script setup lang='ts'>
import { useRouter } from 'vue-router'
const router = useRouter()

// 字符串路径
router.push('/users/eduardo')

// 带有路径的对象
router.push({ path: '/users/eduardo' })

// 命名的路由，并加上参数，让路由建立 url
router.push({ name: 'user', params: { username: 'eduardo' } })

// 带查询参数，结果是 /register?plan=private
router.push({ path: '/register', query: { plan: 'private' } })

// 替换当前位置 (不会有历史记录)
router.push({ path: '/home', replace: true })
// 相当于
router.replace({ path: '/home' })

router.back()
router.forward()
router.go(-1) // router.go(-1) => router.back()
router.go(1) // router.go(1) => router.forward()
</script>
```
> 前端路由切换的本质是什么？hash 和 history 有什么区别？
> - 前端路由切换的本质: 监听 URL 的改变, URL 和内容进行映射

|                                            hash                                            |                                          history                                           |
|:------------------------------------------------------------------------------------------:|:------------------------------------------------------------------------------------------:|
|                                          有 # 号                                           |                                         没有 # 号                                          |
|                                      能够兼容到 IE 8                                       |                                      只能兼容到 IE 10                                      |
| 实际的 url 之前使用哈希字符，这部分 url 不会发送到服务器，不需要在服务器层面上进行任何处理 | 每访问一个页面都需要服务器进行路由匹配生成 html 文件再发送响应给浏览器，消耗服务器大量资源 |
|                                   刷新不会存在 404 问题                                    |                         浏览器直接访问嵌套路由时，会报 404 问题。                          |
|                                    不需要服务器任何配置                                    |                                需要在服务器配置一个回调路由                                |

## 5.5 路由的嵌套
* 1. 在一层路由中添加 children 属性:
  * `{ path: "recommend", component: () => import ("@/views/homerecomend.vue") }`
* 2. 在 Home 组件中添加 `<router-view>`
* 3. 路径跳转 `<router-link>`
```js
routes: [
    {
      path: "/",
      // 重定向,将根路径重定向到/home路径,默认跳到首页.
      redirect: "/home",
    },
    {
      name: "home",
      path: "/home",
      // 懒加载
      component: () => import(/* webpackChunkName:"home-chunk"*/"../Views/Home.vue"),
      children: [
        {
          //如果path:" ",需要写name
          path: "/home",
          // 重定向
          redirect: "/home/rank"
        },
        {
          path: "rank",//相当于/home/rank
          component: () => import("../Views/HomeRanking.vue")
        },
        {
          path: "recommend",// 相当于/home/recommend
          component: () => import("../Views/HomeRecomend.vue")
        }
      ]
    },
]
```

## 5.6 路由守卫 
- vue-router 提供的导航首位主要用来通过**跳转或取消的方式守卫导航**
- 全局的前置守卫 beforeEach 是在导航触发时会被回调
	- 两参数
		- `to` 即将进入的路由的 Route 对象
		- `form` 即将离开的路由 Route 对象
	- 返回值
		- `false` 取消当前导航
		- `无返 or undefined` 进入默认导航
		- `路由地址`
			- string 类型的 path 
			- object 类型的对象, 可以携带 path、query、params 等信息
```js
// 路由守卫(进行任何路由跳转之前,传入beforeEach中的函数都会呗回调)
router.beforeEach((to, from) => {
  if (to.path !== '/login') {
    if (!isLogged.value) {
      ElMessage({ type: 'warning', message: '请先登录!' })
      return '/login'
    }
  }
})
```

![[导航的整个流程解析.png]]

## 5.7 动态路由
不同的人就会有不同的权限, 那么不同的人对应能访问的页面能做的操作也就不同 

通过创建出来的路由实例上的 addRoutes 方法添加

```js
router.addRoute("<name>",{
  path: "",
  name: "",
  component: ()=>import("./...")
})
```
# 六、状态管理
## 6.1 Vuex
- 每一个 vuex 对应的核心就是 store (仓库)
	- store 是一个基本的容器保存着应用中包含的大部分的状态 (state) 
- vuex 中保存的数据是响应式的
	- 当组件从 store 中读取状态的时候如果数据发生变化则相应的组件也会重新渲染
- 改变 state 唯一的方式
	- 通过提交 (commit) mutation 的方式
- Mutation 中只能有同步处理
- 对应的异步处理需要 actions  通过组件中 dispatch 的操作

### Vuex 的基本使用步骤
* 单一状态树:
	- Vuex 中使用单一状态树
	- 通过一个对象就包含了全部的应用层级的状态
	- 然后通过模块的形式区分不同模块中的状态的定义

* Vuex 的基本使用步骤
	- 安装 `npm install vuex@next --save`
	- 通过 createStore 创建一个 store 实例
		```js
		import {createStore} from "vuex"
		import home from "./home.js"
		const store = createStore({
		  // modules 因为vuex是一个单一状态树 
		  // 所以对应的根状态的下一个层级就是在modules中定义 方便之后的状态的管理和维护
		  modules: {
	        home,
	        about
		  },
		  // store() 返回一个包含定义状态的对象 
		  state () {
		    return {
		      name: "wmm",
		      age: 18,
		      height: "1.88"
		    }
		  },
		  // mutations 定义改变state的方法(同步) 组件中通过commit调用
		  mutations: {
		    increment(state,payload = 1) {
		      state.age+= payload
		    }
		  },
		  // actions定义这异步请求的函数 组件中通过 dispatch方法调用
		  actions: {
		    getInfo() {
		      ...
		    }
		  },
		  // getters 定义这一些对state中的数据封装后的值 相当于组件中的 computed
		  getters: {
		    getShopTotalPrice: (state) => {
		      return state.shopList.reduce((total, next) => {
		        return total + next.price * next.num
		      }, 0)
		    }
		  }
		})
		export default store
		```
- 在 app 中通过插件安装
	```js
	// main.js
	import store from "./store"
	createApp(App).use(store).mount("#root")
	```
- 在组件中使用
	```html
	<!--home.Vue-->
	<template> {{name}} </template>
	
	<script setup>
	import { useStore } from "vuex"
	const store = useStore()
	const name = store.State.name
	</script>
	```

### Vuex state-映射到组件中  
```js
state: () => ({
    count: 10,
    name: 'fishx',
    type: '管理员',
    shopList: [
      { id: 1, name: '内墙漆', price: 41.5, num: 1, img: 'neiqiang.jpeg' },
      { id: 2, name: '多功能排插', price: 52.0, num: 4, img: 'paichafanglei.jpg' },
      { id: 3, name: '万用表', price: 32.5, num: 2, img: 'wyb.jpeg' },
      { id: 4, name: '金杯电线', price: 75.5, num: 3, img: 'xinlan.jpeg' }
    ]
})
```

- Options api
	```html
	<script>
	import { mapState } from 'vuex'
	
	export default {
	  computed: {
	    fullname() {
	      return 'xxx'
	    },
	    ...mapState(['name', 'level', 'avatarURL']),
	    ...mapState({
	      sName: state => state.name,
	      sLevel: state => state.level
	    })
	  }
	}
	</script>
	```
- Composition api
	```html
	<script setup>
	import { mapState, useStore } from 'vuex'
	import { computed } from 'vue'
	
	const { name, level } = mapState(["name", "level"])
	const store = useStore()
	const cName = computed(name.bind({ $store: store }))
	const cLevel = computed(level.bind({ $store: store }))
	</script>
	```

	```html
	<script setup>
	import { toRefs } from 'vue'
	import { useStore } from 'vuex'
	
	const state = useStore()
	const { name, type } = toRefs(state.state)
	</script>
	```


### Vuex getters-映射到组件中
```js
getters: {
    getShopTotalPrice: (state) => {
      return state.shopList.reduce(function (total, next) {
        return total + next.price * next.num
      }, 0)
    },
    getShopById: (state) => {
      return (id: number) => {
        return state.shopList.filter((item) => item.id === id)
      }
    }
}
```

- Options api
	![[Pasted image 20230303010123.png]]
- Composition api
	```html
	<script setup>
	import { toRefs } from 'vue'
	import { useStore } from 'vuex'
	
	const state = useStore()
	const { getShopTotalPrice, getShopById } = toRefs(state.getters)
	</script>

	<template>
	   <p>
	      gettes映射: <br />
	      &nbsp; gettes(返回一个值): {{ getShopTotalPrice }}<br />
	      &nbsp; gettes(返回一个函数): {{ getShopById(1) }}
	   </p>
	</template>
	```

### Vuex mutations
```js
// 重要原则: 不要在mutation方法中执行异步操作
mutations: {
    increase: (state) => {
      state.count++
    },
    modify: (state, payload) => {
      state.shopList[0].name = payload
    },
    modifyObj: (state, payload: { id: number; name: string }) => {
      state.shopList[payload.id - 1].name = payload.name
    }
}
```

```html
<script setup lang="ts">
import { ref } from 'vue'
import store from '@/store'
const state = useStore()

const str = ref('')
const id = ref(1)
const increase = () => {
  store.commit('increase')
}
const modifyName = () => {
  store.commit('modify', str)
}
const modifyObj = () => {
  store.commit('modifyObj', { id: id.value, name: str })
}
</script>

<template>
	<button @click="increase">count++</button> &nbsp;
	<input type="text" v-model="id" />&nbsp;
	<input type="text" v-model="str" />&nbsp;
	<button @click="modifyName">修改第1个shop-name</button>&nbsp;
	<button @click="modifyObj">以对象形式修改</button>
</template>
```

- 映射
	- Options api
		```html
		<script>
		import { mapMutations } from 'vuex'
		export default {
		  computed: {},
		  methods: {
		    ...mapMutations(["changeName", "incrementLevel"])
		  }
		}
		</script>
		
		<template>
		    <button @click="changeName('王小波')">修改name</button>
		    <button @click="incrementLevel">递增level</button>
		</template>
		```
	- Composition api
		```html
		<script setup>
		import { mapMutations, useStore } from 'vuex'
		const store = useStore()
		
		// 手动的映射和绑定
		const mutations = mapMutations(['changeName', 'incrementLevel'])
		const newMutations = {}
		Object.keys(mutations).forEach(key => {
		  newMutations[key] = mutations[key].bind({ $store: store })
		})
		const { changeName, incrementLevel } = newMutations
		</script>
		```

### Vuex actions
```js
mutations: {
    increment(state) {
      state.counter++
    },
    changeName(state, payload) {
      state.name = payload
    }
},
actions: {
    incrementAction(context) {
      // console.log(context.commit) // 用于提交mutation
      // console.log(context.getters) // getters
      // console.log(context.state) // state
      context.commit("increment")
    },
    changeNameAction(context, payload) {
      context.commit("changeName", payload)
    } 
}
```
Options api
```html
<script>
import { mapActions } from 'vuex'

export default {
  methods: {
    counterBtnClick() {
      this.$store.dispatch('incrementAction')
    },
    nameBtnClick() {
      this.$store.dispatch('changeNameAction', 'aaa')
    },
    // 辅助函数,映射
    ...mapActions(['incrementAction', 'changeNameAction'])
  }
}
</script>

<template>
  <div class="home">
    <h2>当前计数: {{ $store.state.counter }}</h2>
    <button @click="counterBtnClick">发起action修改counter</button>
    <h2>name: {{ $store.state.name }}</h2>
    <button @click="nameBtnClick">发起action修改name</button>
  </div>
</template>
```

Composition api
```html
<script setup>
import { useStore, mapActions } from 'vuex'

const store = useStore()

// 1.在setup中使用mapActions辅助函数
const actions = mapActions(['incrementAction', 'changeNameAction'])
const newActions = {}
Object.keys(actions).forEach(key => {
  newActions[key] = actions[key].bind({ $store: store })
})
const { incrementAction, changeNameAction } = newActions

// 2.使用默认的做法
function increment() {
  store.dispatch('incrementAction')
}
</script>

<template>
  <div class="home">
    <h2>当前计数: {{ $store.state.counter }}</h2>
    <button @click="incrementAction">发起action修改counter</button>
    <button @click="increment">递增counter</button>
    <h2>name: {{ $store.state.name }}</h2>
    <button @click="changeNameAction('bbbb')">发起action修改name</button>
  </div>
</template>
```

Actions 异步获取数据
```js
state: () => ({
    banners: [],
	recommends: []
}),
mutations: {
	changeBanners(state, banners) {
      state.banners = banners
    },
    changeRecommends(state, recommends) {
      state.recommends = recommends
    }
},
actions: {
    fetchHomeList: async () => {
      const res = await fetch('http://123.207.32.32:8000/home/multidata')
      const data = await res.json()
      // 保存至state
      context.commit('changeBanners', data.banner.list)
      context.commit('changeRecommends',data.recommend.list)
    }
}
```

```html
<script setup>
import { useStore } from 'vuex'

// 告诉Vuex发起网络请求
const store = useStore()
store.dispatch('fetchHomeList').then(res => {
  console.log('home中的then被回调:', res)
})
</script>

<template>
  <div class="banner">
	<div v-for="(item, index) in $store.state.banners" :key="index">
		<p>{{ item.title }}</p>
	    <img :src="item.image" alt="" width="260" />
	</div>
  </div>
</template>
```

### Vuex modules
```js
// src/store/index.js
import { createStore } from 'vuex'
import home from './modules/home'

const store = createStore({
  modules: {
    home
  }
})

export default store
```

```js
// src/store/modules/home.js
export default {
  state: () => ({
    banners: [],
    recommends: []
  }),
  getters: {},
  mutations: {
    changeBanners(state: { banners: any }, banners: any) {
      state.banners = banners
    },
    changeRecommends(state: { recommends: any }, recommends: any) {
      state.recommends = recommends
    }
  },
  actions: {
    fetchHomeList: async (context: { commit: (arg0: string, arg1: any) => void }) => {
      const res = await fetch('http://123.207.32.32:8000/home/multidata')
      const { data } = await res.json()

      // 保存至state
      context.commit('changeBanners', data.banner.list)
      context.commit('changeRecommends', data.recommend.list)
    }
  }
}
```

```html
<script setup lang="ts">
import store from '@/store/index'
import { toRefs } from 'vue'
import { useStore } from 'vuex'

const state = useStore()
const { banners } = toRefs(state.state.home)
store.dispatch('fetchHomeList')
</script>

<template>
    <div class="banner">
      <div v-for="(item, index) in banners" :key="index">
        <p>{{ item.title }}</p>
        <img :src="item.image" alt="" width="265" />
      </div>
	</div>
</template>
```
## 6.2 Pinia 
- 特点
	- 完整的 ts 的支持；
	- 足够轻量，压缩后的体积只有 1 kb 左右;
	- 去除 mutations，只有 state，getters，actions；
	- Actions 支持同步和异步；
	- 代码扁平化没有模块嵌套，只有 store 的概念，store 之间可以自由使用，每一个 store 都是独立的
	- 无需手动添加 store，store 一旦创建便会自动添加；
	- 支持 Vue 3 和 Vue2
- 安装
	- `npm install pinia`
- `main.ts` 注册
	```ts
	import { createApp } from 'vue'
	import App from './App.vue'
	import { createPinia } from 'pinia'
	const store = createPinia()
	createApp(App).use(store).mount('#app')
	```

![[学习Vue3#3.定义仓库Store]]
### Pinia state
- State 是允许直接修改值的, 例如 `current++` 
	```html
	<script setup lang="ts">
	import useCounter from '@/pinia/counter'
	const counter = useCounter()
	const add = () => counter.count++
	</script>
	
	<template>
	  <div id="pinia">
	    <h1>状态管理: Pinia 🍍</h1>
	    <hr />
	    <p>
	      counter: {{ counter.count }} &nbsp;
	      <button @click="add">修改count+1</button>
	    </p>
	  </div>
	</template>
	```
- `$reset`
	- 重置 store 到他的初始状态
	- 调用$reset () `-->` 将会把 state 所有值重置回原始状态
	```html
	<script setup lang="ts">
	import useCounter from '@/pinia/counter'
	const counter = useCounter()
	const reset = () => counter.$reset()
	</script>
	
	<button @click="reset">change</button>
	```

[[学习Vue3#学习Pinia 第三章（State）]]

```js
import useUser from '@/stores/user'
import { storeToRefs } from 'pinia'

const userStore = useUser()
const { name, age, level } = storeToRefs(userStore)

function changeState() {
  // 1.一个个修改状态
  userStore.name = 'kobe'
  userStore.age = 20
  userStore.level = 200

  // 2.一次性修改多个状态
  userStore.$patch({
    name: 'james',
    age: 35
  })

  // 3.替换state为新的对象
  const oldState = userStore.$state
  userStore.$state = {
    name: 'curry',
    level: 200
  }
  console.log(oldState === userStore.$state)
}

function resetState() {
  userStore.$reset()
}
```

### Pinia getters
[[学习Vue3#2.getters]]

```js
// user_store
import { defineStore } from 'pinia'

const useCounter = defineStore('counter', {
  state: () => ({
    count: 1
  }),
  getters: {
    // 1. 基本使用
    doubleCount: (state) => {
      return state.count * 2
    },
    // 2. getter依赖另一个getter结果
    doubleCountAddOne(): number {
      // this是store实例
      return this.doubleCount + 1
    },
    // 3. 返回一个函数(传参)
    addCount: (state) => {
      return (num: number) => {
        return state.count + num
      }
    },
    // 4. 使用其他store中的数据
    showMessage: (): string => {
      // (1).获取user信息
      const user_store = useUser()
      // (2).获取自己的信息

      // (3).拼接信息
      return `{name:${user_store.name},type:${user_store.type}}`
    }
  }
})

export default useCounter
```

```html
<template>
    <p>
      getters: 
      doubleCount-{{ counter.doubleCount }} ||
      doubleCountAddOne-{{ counter.doubleCountAddOne }} ||
      addCount(11)-{{ counter.addCount(11) }} ||
      useInfo: {{ counter.showMessage }}
    </p>
</template>
```

### Pinia actions
[[学习Vue3#1. Actions（支持同步、异步）]]

```ts
// store
actions: {
    modify(num: number) {
      this.count = num
    },
    async fetchHomeList() {
      const res = await fetch('http://123.207.32.32:8000/home/multidata')
      const { data } = await res.json()
      this.banners = data.banner.list
      this.recommends = data.recommend.list
    }
}
```

```html
<script setup lang="ts">
import useCounter from '@/pinia/counter'
const counter = useCounter()
counter.fetchHomeList()
</script>

<template>
    <div class="recommends">
        <div v-for="(item, index) in counter.recommends" :key="index">
          <img :src="item.image" alt="" width="160"/>
          {{ item.title }}
        </div>
    </div>
</template>
```

### Pinia 总结
- Pinia 中有哪些核心概念
	- State
	  - State 是一个选项，这个选项的值需要是一个函数，函数返回一个对象，对象中存储数据
	  - 在组件中拿到当前的 store 直接使用即可，`store.Xxx`
	- Getters
	  - Getters 也是一个选项，这个选项的值是一个对象，对象中存储着一个个函数，每个函数可以有一个参数 state，通过 state 可以获取到当前 store 的 state
	  - 除此之外每个函数还可以拿到一个 this，这个 this 就是当前的整个 store 实例
	  - 通过这个 this，可以想用谁就用谁
	  - 在组件中使用也是拿到 store 直接 `store.Xxx` 即可
	- Actions
	  - 在 actions 中，主要存放一个个函数，每个函数最主要的工作发送异步请求，获取到数据后直接修改 state
	  - 每个 action 函数并不像 getter 函数一样，第一个参数是 state，它可以没有参数
	  - 需要通过 this 拿到 state 然后再修改 state 中的值
	  - 在组件中拿到 store 后直接调用即可，`store.Xxx()`
	    - 如果你在此时传递参数，那么就可以在 action 中拿到参数
	- 没有模块的概念
	  - 在 vuex 中会有 modules 的概念
	  - 但是在 pinia 中没有，想要有相同的概念，只需要多创建结构 store 即可
	  - Store 与 store 之前没有什么必然的联系
	  - 如果你想在某个 store 中使用另一个 store 中的内容，只需要拿到那个 store 使用里面的内容即可，灵活
# 七、网络请求库 axios

安装 `npm install axios`

## 7.1 axios 发送请求 
```js
import axios from 'axios'

// 1.发送request请求
axios.request({
  url: "http://123.207.32.32:8000/home/multidata",
  method: "get"
}).then(res => {
  console.log("res:", res.data)
})

// 2.发送get请求
axios.get(`http://123.207.32.32:9001/lyric?id=500665346`).then(res => {
  console.log("res:", res.data.lrc)
})
axios.get("http://123.207.32.32:9001/lyric", {
  params: {
    id: 500665346
  }
}).then(res => {
  console.log("res:", res.data.lrc)
})

// 3.发送post请求
axios.post("http://123.207.32.32:1888/02_param/postjson", {
  name: "coderwhy",
  password: 123456
}).then(res => {
  console.log("res", res.data)
})

axios.post("http://123.207.32.32:1888/02_param/postjson", {
  data: {
    name: "coderwhy",
    password: 123456
  }
}).then(res => {
  console.log("res", res.data)
})
```

- 常见的配置选项
	- 请求地址
		- url: '/user'	
	- 请求类型 
		- method: 'get'
	- 请求根路径 
		- baseURL: `http://www.mt.com/api`
	- 请求前的数据处理 
		- transformRequest: `function(data){}` 
	- 请求后的数据处理 
		- transformResponse: `function (data){}`
	- 自定义的请求头 
		- `headers:{'x-Requested-With': 'XMLHttpRequest'}`
	- URL 查询对象 
		- `params:{ id: 12 }`
	- 查询对象序列化函数
		- paramsSerializer: `function (params){}` 
	- request body 
		- data: { key: 'aa'}
	- 超时设置
		- timeout: 1000

## 7.2 axios 额外补充
* axios.defaults.baseURL
* axios.defaults.timeout/headers
* axios.all -> Promise.all
```js
import axios from 'axios'

// baseURL
const baseURL = "http://123.207.32.32:8000"

// 给axios实例配置公共的基础配置
axios.defaults.baseURL = baseURL
axios.defaults.timeout = 10000
axios.defaults.headers = {}

// axios发送多个请求
axios.all([
  axios.get("/home/multidata"),
  axios.get("http://123.207.32.32:9001/lyric?id=500665346")
]).then(res => {
  console.log("res:", res)
})
```

## 7.4 axios 创建实例
```js
import axios from 'axios'

// axios默认库提供给我们的实例对象
axios.get("http://123.207.32.32:9001/lyric?id=500665346")

// 创建其他的实例发送网络请求
const instance1 = axios.create({
  baseURL: "http://123.207.32.32:9001",
  timeout: 6000,
  headers: {}
})

instance1.get("/lyric", {
  params: {
    id: 500665346
  }
}).then(res => {
  console.log("res:", res.data)
})

const instance2 = axios.create({
  baseURL: "http://123.207.32.32:8000",
  timeout: 10000,
  headers: {}
})
```

## 7.5 axios 的拦截器
* `axios.interceptors.request.use(请求成功拦截, 请求失败拦截)`
* `axios.interceptors.response.use(响应成功拦截, 响应失败拦截)`
```js
import axios from 'axios'

// 请求拦截
axios.interceptors.request.use((config) => {
  console.log("请求成功的拦截")
  // 1.开始loading的动画

  // 2.对原来的配置进行一些修改
  // 2.1. header
  // 2.2. 认证登录: token/cookie
  // 2.3. 请求参数进行某些转化

  return config
}, (err) => {
  console.log("请求失败的拦截")
  return err
})

// 响应拦截
axios.interceptors.response.use((res) => {
  console.log("响应成功的拦截")

  // 1.结束loading的动画

  // 2.对数据进行转化, 再返回数据
  return res.data
}, (err) => {
  console.log("响应失败的拦截:", err)
  return err
})

axios.get("http://123.207.32.32:9001/lyric?id=500665346").then(res => {
  console.log("res:", res)
}).catch(err => {
  console.log("err:", err)
})
```

## 7.6 axios 的简洁封装
- `src/server/index.ts`
	```ts
	import axios from 'axios'
	import type { AxiosInstance, AxiosRequestConfig } from 'axios'
	
	class Request {
	  // axios 实例
	  instance: AxiosInstance
	
	  constructor(baseURL: string, timeout = 6000) {
	    this.instance = axios.create({
	      baseURL,
	      timeout
	    })
	  }
	
	  requset(config: AxiosRequestConfig) {
	    return new Promise((resolve, reject) => {
	      this.instance
	        .request(config)
	        .then((res) => {
	          resolve(res.data)
	        })
	        .catch((err) => {
	          reject(err)
	        })
	    })
	  }
	
	  get(config: AxiosRequestConfig) {
	    return this.requset({ ...config, method: 'get' })
	  }
	
	  post(config: AxiosRequestConfig) {
	    return this.requset({ ...config, method: 'post' })
	  }
	}
	
	export default new Request('http://123.207.32.32:9001')
	```
- 使用
	```ts
	import request from '@/server'
	
	request
	  .requset({ url: '/lyric', params: { id: 500665346 } })
	  .then((res: any) => {
	    console.log('res: ', res)
	  })
	  .catch((err) => {
	    console.log('err: ', err)
	  })
	```

# 八、vue 自定义指令
>[!note]- Vue 中重用代码的方式：组件和组合式函数
>- 组件是主要的构建模块，而组合式函数则侧重于有状态的逻辑
>- 自定义指令主要是为了重用涉及普通元素的底层 DOM 访问的逻辑

## 8.1 局部/全局指令 
局部指令
```html
<script setup>
// 在模板中启用 v-focus
const vFocus = {
  mounted: (el) => el.focus()
}
</script>

<template>
  <input v-focus />
</template>
```
全局指令
```js
const app = createApp(App)
app.directive('focus', {
  mounted: (el) => el.focus()
})

app.mount('#app')
```

## 8.2 指令-生命周期 
![[Pasted image 20230313221008.png]]
```js
const myDirective = {
  // 在绑定元素的 attribute 前
  // 或事件监听器应用前调用
  created(el, binding, vnode, prevVnode) {
    // 下面会介绍各个参数的细节
  },
  
  // 在元素被插入到 DOM 前调用
  beforeMount(el, binding, vnode, prevVnode) {},
  
  // 在绑定元素的父组件
  // 及他自己的所有子节点都挂载完成后调用
  mounted(el, binding, vnode, prevVnode) {},
  
  // 绑定元素的父组件更新前调用
  beforeUpdate(el, binding, vnode, prevVnode) {},
  
  // 在绑定元素的父组件
  // 及他自己的所有子节点都更新后调用
  updated(el, binding, vnode, prevVnode) {},
  
  // 绑定元素的父组件卸载前调用
  beforeUnmount(el, binding, vnode, prevVnode) {},
  
  // 绑定元素的父组件卸载后调用
  unmounted(el, binding, vnode, prevVnode) {}
}
```

## 8.3 指令-钩子参数 
[钩子参数](https://cn.vuejs.org/guide/reusability/custom-directives.html#hook-arguments)

- 指令的钩子会传递以下几种参数：
	- `el`：指令绑定到的元素。这可以用于直接操作 DOM。
	- `binding`：一个对象，包含以下属性。
	    - `value`：传递给指令的值。例如在 `v-my-directive="1 + 1"` 中，值是 `2`。
	    - `oldValue`：之前的值，仅在 `beforeUpdate` 和 `updated` 中可用。无论值是否更改，它都可用。
	    - `arg`：传递给指令的参数 (如果有的话)。例如在 `v-my-directive:foo` 中，参数是 `"foo"`。
	    - `modifiers`：一个包含修饰符的对象 (如果有的话)。例如在 `v-my-directive.foo.bar` 中，修饰符对象是 `{ foo: true, bar: true }`。
	    - `instance`：使用该指令的组件实例。
	    - `dir`：指令的定义对象。
	- `vnode`：代表绑定元素的底层 VNode。
	- `prevNode`：之前的渲染中代表指令所绑定元素的 VNode。仅在 `beforeUpdate` 和 `updated` 钩子中可用。

```html
<div v-example:foo.bar="baz">
```
`binding` 参数会是一个这样的对象：
```js
{
  arg: 'foo',
  modifiers: { bar: true },
  value: /* `baz` 的值 */,
  oldValue: /* 上一次更新时 `baz` 的值 */
}
```