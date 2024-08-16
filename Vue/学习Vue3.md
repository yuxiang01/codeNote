## 学习Vue3
- 写 vue2 ==> 关闭 vetur, 打开 volar
- 写 vue3 ==> 打开 volar, 关闭 vetur 
### var、let、const声明变量的区别
* var与let的区别：
    * 1.var声明变量可以重复声明，重复声明后之前变量值被覆盖;而let不可以重复声明，重复声明会报错。
    * 2.var声明的变量不受限于块级作用域，即var声明的变量是全局变量，不受当前(块级)作用域;let声明的变量当前(块级)作用域限制，只在作用域内有效。
    * 3.let不存在变量提升：var声明变量的代码上面可以访问变量，而let不可以，在let声明的上面访问变量会报错，这就我们说的暂存死区。
    * 4、var会与window相映射(会挂一个属性)，而let不与window相映射
* const声明变量的特点
    * const和let一样不会与window相映射、支持块级作用域、在声明的上面访问变量会报错
    * const声明之后必须赋值，否则会报错
    * const定义不可变的量，改变了就会报错
### 学习Vue3 第一章
#### 1.重写双向绑定

- vue2

  - 基于Object.defineProperty()实现

- vue3

  - 基于Proxy与Object.defineProperty(obj, prop, desc)方式,优势:
    ```text
    1. 丢掉麻烦的备份数据
    2. 省去for in 循环
    3. 可以监听数组变化
    4. 代码更简化
    5. 可以监听动态新增的属性；
    6. 可以监听删除的属性 ；
    7. 可以监听数组的索引和 length 属性；
    ```
##### 代码实例

```js
let proxyObj = new Proxy(obj,{
	get : function(target,prop) {
		return prop in target ? target[prop] : 0
	},
	set : function (target,prop,value) {
		target[prop] = 888;
	}
})
```

#### 2.Vue3 优化Vdom([查看静态标记](https://vue-next-template-explorer.netlify.app/))

- Vue2中
  - ***每次更新diff,都是全量对比***

- Vue3

  - ***只对比带有标记的*** , 减少了非动态内容的对比消耗 

- patch flag 优化静态树

   ```html
    <span>Hello world!</span>
    <span>Hello world!</span>
    <span>Hello world!</span>
    <span>Hello world!</span>
    <span>{{msg}}</span>
    <span>Hello world!</span>
    <span>Hello world! </span>
    ```

  - Vue3 编译后的 Vdom 是这个样子的

    ```js
    export function render(_ctx，_cache,$props,$setup，$data，$options){return (_openBlock(),_createBlock(_Fragment,null,[
    _createvNode( "span", null,"Hello world ! "),
    _createvNode( "span", null,"Hello world! "),
    _createvNode( "span", null,"Hello world! "),
    _createvNode( "span", null,"Hello world! "),
    _createVNode("span", null,_toDisplaystring(_ctx.msg),1/* TEXT */),
    _createvNode( "span", null,"Hello world! "),
    _createvNode( "span", null,"Hello world! ")],64/*STABLE_FRAGMENT */))
    ```

  - 新增了 patch flag 标记

    ```json
    TEXT = 1 // 动态文本节点
    CLASS=1<<1,1 // 2//动态class
    STYLE=1<<2，// 4 //动态style
    PROPS=1<<3,// 8 //动态属性，但不包含类名和样式
    FULLPR0PS=1<<4,// 16 //具有动态key属性，当key改变时，需要进行完整的diff比较。
    HYDRATE_ EVENTS = 1 << 5，// 32 //带有监听事件的节点
    STABLE FRAGMENT = 1 << 6, // 64 //一个不会改变子节点顺序的fragment
    KEYED_ FRAGMENT = 1 << 7, // 128 //带有key属性的fragment 或部分子字节有key
    UNKEYED FRAGMENT = 1<< 8, // 256 //子节点没有key 的fragment
    NEED PATCH = 1 << 9, // 512 //一个节点只会进行非props比较
    DYNAMIC_SLOTS = 1 << 10 // 1024 // 动态slot
    HOISTED = -1 // 静态节点
    BALL = -2
    ```

  - Vue3 Fragment
  	- 允许我们支持多个根节点
  	- 支持render JSX 写法
  	- 同时新增了Suspense 和 多 v-model 用法
  	
  - Vue3 Tree shaking
    - 简单来讲，就是在保持代码运行结果不变的前提下，去除无用的代码
  
  - Vue 3 Composition Api
    - Setup 函数式编程 也叫vue Hook

### 学习Vue3 第二章(配置环境)

- #### 1.建议安装14版本

- #### 2.vite项目优势

  - 冷服务 
    - 默认的构建目标浏览器是能 [在 script 标签上支持原生 ESM](https://caniuse.com/es6-module) 和 [原生 ESM 动态导入](httccxxps://caniuse.com/es6-module-dynamic-import)
  -  HMR 
        - 速度快到惊人的 [模块热更新（HMR）](https://vitejs.cn/guide/features.html#hot-module-replacement)
  - Rollup打包 
    - 它使用 [Rollup](https://rollupjs.org/) 打包你的代码，并且它是预配置的 并且支持大部分rollup插件

- #### 3.构建vite项目([开始](https://cn.vitejs.dev/guide/))

  - npm
   
    ```shell
    npm init vite@latest
    ```
    
    ![](16460487624094.jpg)
  
- #### 4.package json 命令解析
```json
{
  "scripts": {
    "dev": "vite", // 启动开发服务器，别名：`vite dev`，`vite serve`
    "build": "vite build", // 为生产环境构建产物
    "preview": "vite preview" // 本地预览生产构建产物
  }
} 
```  
- #### 5.安装Vue cli脚手架
```shell
npm install @vue/cli -g
```
### 学习Vue3 第三章（Vite目录 & Vue单文件组件）
- #### Vite目录
    - public ==> **不会被编译**, 存放静态资源
    - assets ==> **存放**可编译**静态资源** 
    - components ==> 自定义组件
    - App.vue ==> 全局组件
    - main.ts ==> 全局ts文件
    - index.html ==> 入口文件 

- #### SFC 语法规范<br>
    *.vue 件都由三种类型的顶层语法块所组成
- ##### 标签 template 
    - 每个 *.vue 文件最多可同时包含一个顶层 < template >块。
    - 其中的内容会被提取出来并传递给 @vue/compiler-dom，预编译为 JavaScript 的渲染函数，并附属到导出的组件上作为其 render 选项。
- ##### 标签 script setup 
    - 每个 *.vue 文件最多可同时包含一个< script setup >块 (不包括常规的< script >)
    - 该脚本会被预处理并作为组件的 setup() 函数使用，也就是说它会在每个组件实例中执行.< script setup > 的顶层绑定会自动暴露给模板。更多详情请查看<[script setup](https://v3.cn.vuejs.org/api/sfc-script-setup.html)> 文档。
- ##### 标签 style --> SFC [样式特性](https://v3.cn.vuejs.org/api/sfc-style)
    - 一个 *.vue 文件可以包含多个 < style > 标签。
    - < style > 标签可以通过 scoped 或 module attribute将样式封装在当前组件内。多个不同封装模式的 < style > 标签可以在同一个组件中混
### 学习Vue3 第四章（模板语法 & vue指令）
-  #### 1. 模板插值语法
    - 在script 声明一个变量可以直接在template 使用用法为`{{变量名称}}`. 
    ```html
    <!--1.one.Base-->
    <template>
        <div>{{ message }}</div>
    </template>
    
    <script setup lang= "ts">
        const message = "小满哥."
    </script>
    
    <style>
    </style>
    ```
    
    ```html
    <!--2.模板语法是可以编写条件运算的-->
    <template>
        <div>{{ message == 0 ? '是小满哥' : '是fishx'}}</div>
    </template>
    
    <script setup lang= "ts">
        const message:number = 1
    </script>
    
    <style>
    </style>
    ```
    
    ```html
   <!-- 3.支持运算-->
    <template>
        <div>{{ message + 1}}</div>
    </template>
 
    <script setup lang="ts">
        const message:number = 1
    </script>

    <style>
    </style>
    ```
    
    ```html
    <!--4.支持操作Api-->
    <template>
        <div>{{ message.split(',') }}</div>
    </template>
 
    <script setup lang="ts">
        const message:string = "我,是,小,满"
    </script>
 
    <style>
    </style>
    ```
-  #### 2.指令(v- 开头都是vue 的指令)
    -  ##### v-text
        - 显示文本
    - ##### v-html
        - 展示富文本
    - ##### v-if
        - 控制元素的显示/隐藏 => `注释` => 频繁切换浪费性能
        - v-if能使得组件重新渲染和销毁 
    - ##### v-else-if
        - v-if 的“else if 块”。可以链式调用
    - ##### v-else 
        - v-if条件收尾语句
    - ##### v-show
        - 控制元素的显示和隐藏 =>` display: none`block css切换
    - ##### v-on(`简写@`)
        - 给元素添加事件
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
    - ##### v-bind(`简写:`)
        - 绑定元素的属性
        ```html
         v-bind --- 绑定class案例One
            -------------------------------------------------
            <template>
                <div :class = "[flag ? 'one' : 'two', 'three']"></div>
            </template>
            
            <script setup lang="ts">
                const flag: boolean = false;
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
         
         ```html
         v-bind --- 绑定class案例Two
            -------------------------------------------------
            <template>
                <div :class = "flag"></div>
            </template>
            
            <script setup lang="ts">
                type Cls = {
                    one: boolean,
                    two: boolean
                }
                const flag: Cls = {
                    one: false,
                    two: true
                }
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
            -------------------------------------------------
            除了能绑定类名之外,也能直接绑定style,然后直接写样式
         ```
    - ##### v-model
        - 双向绑定
         ```html
         v-model --- 双向数据绑定案例
            -------------------------------------------------
            <template>
                <input v-model="message" type="text"/>
                <div>{{ message }}</div>
            </template>
           
            <script setup lang="ts">
                 import { ref } from 'vue'
                 const message = ref("v-model")
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

    - ##### v-for
        - 用来遍历元素
        ```html
        <template>
            <div v-for="(item,index) in list">{{item.name}}, 今年{{item.age}}了</div>
        </template>
        
        <script setup lang="ts">
            type List = {
              age: number
              name: string
            }
            const list: List[] = [
              { name: '刘莺莺', age: 19 },
              { name: '王冰冰', age: 22 },
              { name: '范冰冰', age: 30 },
              { name: '刘亦菲', age: 18 }
            ]
        </script>
        
        <style></style>
        ```
### 学习Vue3 第五章（Vue核心虚拟Dom和 diff 算法）
#### 1. 虚拟DOM
- 虚拟DOM就是通过JS来生成一个AST节点树[Vue Template Explorer](https://vue-next-template-explorer.netlify.app/#eyJzcmMiOiI8ZGl2PlxyXG4gICAgPGRpdj4gXHJcbiAgICAgICAgIDxzZWN0aW9uPnRlc3Q8L3NlY3Rpb24+XHJcbiAgICAgIDwvZGl2PiAgXHJcbjwvZGl2PiIsIm9wdGlvbnMiOnt9fQ==)
#### 2. 虚拟DOM的必要性
- 1.原生jsDOM操作浪费性能 `不可取`
- 2.而js又少不了操作Dom,因此要尽可能少的操作Dom
#### 3.Diff算法[(vue3源码)](https://github.com/vuejs/core)
```html
<template>
    <div>
        <div :key="item" v-for="item in Arr">{{item}}</div>
    </div>
</template>

<script setup lang="ts">
    const Arr: Array<string> = ['A','B','C','D']
    Arr.splice(2,0,'DDD') //splice接头(索引,删除数量,新增内容)
</script>

<style></style>
```

| 参数    | 描述 |
|:--------:|:----------------------:|
| index    | 必需.整数,索引,使用负号则从数组的末尾开始 |
| howmany  | 可选.要删除的项目数.0就是不删除任何项目  |
| item1... | 可选.要添加到数组中的新项目         |

![diff](diff.png)

### 学习Vue3 第六章（认识Ref全家桶）
#### 1. ref
- 接受一个内部值并返回一个 ***响应式*** 且可变的 ref 对象。 
- ref 对象仅有一个 .value property，指向该内部值。
```html
<!--One.引入Ref解决-->
<template>
    <div>
      <button @click="changeMsg">change</button>
      <div>{{ message }}</div>
    </div>
</template>

<script setup lang="ts">
import {ref,Ref}
    let message: Ref<string> = ref("点我试试?")
    const changeMsg = () => {
      message.value = "试试就试试"
    }
</script>

<style></style>

```

```html
<!--Two.通过泛型解决-->
<template>
  <div>
    <button @click="changeMsg">change</button>
    <div>{{ message }}</div>
  </div>
</template>

<script setup lang="ts">
    import {ref} from 'vue'
    let message = ref<string | number>("点我试试?")
    
    const changeMsg = () => {
      message.value = "试试就试试,是吧?啊,说的就是你"
    }
</script>

<style></style>

```
#### 2. isRef 
- 判断是否为一个ref对象
```ts
<script setup lang="ts">
  import {ref,Ref,isRef} from 'vue'
  let message: Ref<string | number> = ref("点我试试?")
  let notRef:number = 123
  const changeMsg = () => {
    message.value = "试试就试试"
    console.log(isRef(message))
    console.log(isRef(notRef))
  }  
<script>
```
#### 3. shallowRef 
- 创建一个跟踪自身 .value 变化的 ref，但***非响应式***
```html
One.不能监听到的修改value
-------------------------------------------------
<template>
  <div>
    <button @click="changeMsg">change</button>
    <div>{{ message.name }}</div>
  </div>
</template>

<script setup lang="ts">
import { Ref, shallowRef } from 'vue'
type Obj = {
  name: string
}
let message: Ref<Obj> = shallowRef({
  name: "小满"
})
 
const changeMsg = () => {
  message.value.name = '大满'
}
</script>

<style></style>
```
```html
Two.能过监听到的修改value
-------------------------------------------------
<template>
  <div>
    <button @click="changeMsg">change</button>
    <div>{{ message.name }}</div>
  </div>
</template>

<script setup lang="ts">
import { Ref, shallowRef } from 'vue'
type Obj = {
  name: string
}
let message: Ref<Obj> = shallowRef({
  name: "fishx"
})
 
const changeMsg = () => {
  message.value = {name:'大满'}
}
</script>

<style></style>

```
#### 3.triggerRef
- 强制更新页面DOM
```html
<template>
  <div>
    <button @click="changeMsg">change</button>
    <div>{{ message.name }}</div>
  </div>
</template>

<script setup lang="ts">
import { Ref, shallowRef } from 'vue'
type Obj = {
  name: string
}
let message: Ref<Obj> = shallowRef({
  name: "fishx"
})
 
const changeMsg = () => {
  message.value.name = '大满'
  triggerRef(message)
}
</script>

<style></style>

```
#### 4.customRef
- 自定义ref(使用场景:调用接口后更新数据) 
    - customRef 是个工厂函数要求我们返回一个对象 并且实现 get 和 set
```html
<template>
  <div>
    <button @click="changeMsg">change</button>
    <div>{{ message }}</div>
  </div>
</template>

<script setup lang="ts">
import { customRef } from 'vue'
function MyRef<T>(value: T) { // 形参
  // 将自定义ref工厂函数return出去,该自定义的ref里接收工厂函数
  return customRef((trank, trigger) => {
    return {  
      // 工程函数要求返回一个对象,对象里要返回一个get()和set()
      get() { // 获取
        trank() // 接收trank:收集依赖
        return value 
      },
      set(newVal: T) { // 更新
        console.log('set')
        value = newVal
        trigger() // 与此同时要触发更新页面
      }
    }
  })
}
let message = MyRef<string>('fishx')
const changeMsg = () => {
  message.value = '小满'
}
</script>

<style></style>
```
### 学习Vue3 第七章（认识Reactive全家桶）
#### 1. reactive
  - 用来绑定复杂的数据类型 例如:对象、数组
  - reactive [源码](https://github.com/vuejs/core)约束了我们的类型
  - 因此**不绑定数据类型**是**不允许**的,会报错!
  - 绑定复杂数据类型`reactive`不需要写`.value`、基本类型`ref`需要写
```js
// 1.reactive基础用法
import { reactive } from 'vue'
let person = reactive({
   name: "fishx"
})
person.name = '小满'
```
```js
// 2.数组异步赋值
import { reactive } from 'vue'
let msg = reative<number[]>([])
setTimeout(()=> {
   let arr = [1,2,3] //one
   msg = arr
   console.log(msg)
},1000)
————————————————————————————————————————————
one:这种直接赋值会破坏msg的响应式
```
```js
<!--解决方案one.采用push-->
let msg = reactive<number[]>([])
setTimeout(() => {
  let arr = [1, 2, 3]
  msg.push(...arr)
  console.log(msg)
}, 1000)
```
```js
<!--解决方案two.包裹一层对象-->
let msg = reactive<number[]>([])
type O = {
  list: number[]
}
let msg = reactive<O>({ list: [] })
setTimeout(() => {
  msg.list = [1, 2, 3, 4, 5]
}, 1000)
```
#### 2.readonly
- 拷贝一份proxy对象将其设置为只读
```js
let person = reactive({ count: 1 })
person.count++
let copy = readonly(person)
copy.count++ // x 报错:target is readonly
```
#### 3.shallowReactive 
- 只能对浅层的数据 如果是深层的数据只会改变值 不会改变视图
- 在**Dom挂载前**无论是浅层还是深层次都会生效,**挂载之后**的操作深层次数据是不生效的!
```html
<div>
  <p>{{ message }}</p>
  <button @click="change1">浅响应</button>
  <button @click="change2">深响应</button>
</div>
```
```js
let message = shallowReactive({
  test: "I'm fishx",
  nav: {
    name: "I'm xiaoZ"
  }
})
const change1 = () => {
  message.test = '我是fishx'
}
const change2 = () => {
  message.nav.name = '我是小Z'
  console.log(message)
}
```
### 学习Vue3 第八章（认识to系列全家桶）
#### 1.toRef
- 如果原始对象是非响应式的就不会更新视图 数据是会变的
```html
<!--基础版-->
<template>
  <div>
    <div><button @click="change">改变</button></div>
    <div>{{ state }}</div>
  </div>
</template>

<script setup lang="ts">
 import { toRef } from 'vue'
 const obj = {
   foo: 1,
   bar: 1
 }
const state = toRef(obj, 'bar') // 将obj对象中属性转化为响应式
const change = () => {
  state.value++
  console.log('原始对象--->', obj)
  console.log('引用之后--->', state)
}
<!--——————————————————————————————————————————-->
1.因为转化为ref,就需要使用.value
2.引用了toRef,并不会改变view层,但会改变原始数据
</script>
```
- 如果原始对象是响应式的是会更新视图并且改变数据的
```js
<!--被proxy代理的对象,view层会发生改变(包括原数据)-->
const obj = reactive({
  foo: 1,
  bar: 1
})
```
#### 2.toRefs
- 可以帮我们批量创建ref对象主要是方便我们解构使用
- `toRefs`底层依旧是调用`toRef`,将每一个属性都转为`ref`
```html
<template>
  <div>
    <div>foo--->{{ foo }}</div>
    <div>bar--->{{ bar }}</div>
    <button @click="change">+1</button>
  </div>
</template>

<script setup lang="ts">
import { reactive, toRefs } from 'vue'
const obj = reactive({
  foo: 1,
  bar: 1
})
let { foo, bar } = toRefs(obj)
const change = () => {
  foo.value++
  bar.value++
}
</script>
```
#### 3.toRaw
- 将响应式对象转化为普通对象(便于减少内存)
```js
import { reactive, toRaw } from 'vue'
const obj = reactive({
  foo: 1,
  bar: 1
})
const raw = toRaw(obj)
console.log('-----> 响应式的: ', obj)
console.log('-----> 非响应式的: ', raw)
```
### 学习Vue3 第九章（认识computed计算属性）
- 计算属性就是当依赖的属性的值发生变化的时候，才会触发他的更改，如果依赖的值，不发生变化的时候，使用的是缓存中的属性值。
#### 1.函数形式
```html
<template>
  <input v-model="firstName" type="text" /> <br>
  <input v-model="lastName" type="text" />
  <div>{{ name }}</div>
</template>

<script setup lang="ts">
import { computed, ref } from 'vue'
let firstName = ref('')
let lastName = ref('')

const name = computed(() => {
  return firstName.value + ' ————————  ' + lastName.value
})
</script>
```
#### 2.对象形式
```js
const name = computed({
  get() {
    return firstName.value + lastName.value
  },
  set() {
    firstName.value + lastName.value
  }
})
```
#### 3.购物车案例
```html
<template>
  <div>
    <table border style="width: 800px">
      <thead>
        <tr>
          <th>名称</th>
          <th>数量</th>
          <th>单价</th>
          <th>操作</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in data" :key="index">
          <td align="center">{{ item.name }}</td>
          <td align="center">
            <button @click="AddandSub(item, false)">-</button>
            {{ item.num }}
            <button @click="AddandSub(item, true)">+</button>
          </td>
          <td align="center">{{ item.num * item.price }}</td>
          <td align="center">
            <button @click="del(index)">删除</button>
          </td>
        </tr>
      </tbody>
      <tfoot>
        <td colspan="3">总价</td>
        <td>{{ $total }}</td>
      </tfoot>
    </table>
  </div>
</template>

<script setup lang="ts">
import { computed, reactive, ref } from 'vue'
type Shop = {
  name: string
  num: number
  price: number
}
let $total = ref(0)
const data = reactive<Shop[]>([
  {
    name: '香蕉',
    num: 1,
    price: 25
  },
  {
    name: '汉堡包',
    num: 1,
    price: 10
  },
  {
    name: '苹果',
    num: 1,
    price: 18
  }
])
const AddandSub = (item: Shop, type: boolean): void => {
  if (item.num > 1 && !type) {
    item.num--
  }
  if (item.num < 99 && type) {
    item.num++
  }
}
const del = (index: number) => {
  data.splice(index, 1)
}
$total = computed<number>(() => {
  // 当data数组(被reactive所包裹是响应式的)发生改变,
  // 就触发computed计算属性的回调函数
  return data.reduce((prev, next) => {
    return prev + next.num * next.price
  }, 0)
})
</script>
```
### 学习Vue3 第十章（认识watch侦听器）
- watch 需要侦听特定的数据源，并在单独的回调函数中执行副作用
    - `watch(参数1,参数2,参数3) => {},{options}`
        - 参数1: 监听源
        - 参数2: 回调函数callback（newVal,oldVal）
        - 参数3: options配置项,是一个对象
            - immediate:true //是否立即调用一次
            - deep:true //是否开启深度监听
#### 1.监听Ref 案例
- 监听单个ref
```html
<template>
  <div>
    <input type="text" v-model="msg" />
  </div>
</template>

<script setup lang="ts">
import { watch, ref } from 'vue'
let msg = ref<string>('')
watch(msg, (newVal, oldVal) => {
  console.log('新的', newVal)
  console.log('旧的', oldVal)
})
</script>
```
- 监听多个ref <span style="color: red">注意变成数组!</span>
```html
<template>
  <div>
    <input type="text" v-model="msg1" />
    <input type="text" v-model="msg2" />
  </div>
</template>

<script setup lang="ts">
import { watch, ref } from 'vue'
let msg1 = ref<string>('')
let msg2 = ref<string>('')
watch([msg1,msg2], (newVal, oldVal) => {
  console.log('新的', newVal)
  console.log('旧的', oldVal)
})
</script>
```
- 监听深层ref
```html
<template>
  <div>
    <input type="text" v-model="msg.nav.bar.name" />
  </div>
</template>

<script setup lang="ts">
import { watch, ref } from 'vue'
let msg = ref({
  nav: {
    bar: {
      name: 'fishx'
    }
  }
})
watch(msg,(newVal, oldVal) => {
    console.log('新的', newVal)
    console.log('旧的', oldVal)
  },{
    deep: true
  })
</script>
``` 
#### 2.监听Reactive
- 使用reactive监听深层对象,开启和不`开启deep效果一样`
```html
<template>
  <div>
    <input type="text" v-model="msg.nav.bar.name" />
  </div>
</template>

<script setup lang="ts">
import { watch, ref } from 'vue'
let msg = reative({
  nav: {
    bar: {
      name: 'fishx'
    }
  }
})
watch(msg,(newVal, oldVal) => {
    console.log('新的', newVal)
    console.log('旧的', oldVal)
  }
</script>
```
- 监听reactive 单一值
```html
<template>
  <div>
    <input type="text" v-model="msg.name" />
    <input type="text" v-model="msg.name2" />
  </div>
</template>

<script setup lang="ts">
import { watch, ref, reactive } from 'vue'
let msg = reactive({
  name: 'fishx',
  name2: 'fishc'
})
watch(() => msg.name, (newVal, oldVal) => {
  console.log('新的', newVal)
  console.log('旧的', oldVal)
})
</script>
```
### 学习Vue3 第十一章（认识watchEffect高级侦听器）
#### 1.watchEffect
  - 立即执行传入的一个函数，同时响应式追踪其依赖，并在其依赖变更时重新运行该函数。
  - 如果用到message 就只会监听message 就是用到几个监听几个 而且是非惰性 会`默认调用一次`
```html
<template>
  <div>
    <input v-model="meg" type="text" />
    <input v-model="msg" type="text" />
  </div>
</template>

<script setup lang="ts">
import { watchEffect, ref } from 'vue'
let meg = ref<String>('fishx')
let msg = ref<String>('fishc')
watchEffect(() => {
  console.log('meg', meg.value)
  console.log('msg', msg.value)
})
</script>
```  
#### 2.清除副作用
- 就是在触发监听之前会调用一个函数可以处理你的逻辑,例如防抖
```js
import { watchEffect, ref } from 'vue'
let meg = ref<String>('fishx')
let msg = ref<String>('fishc')
watchEffect((oninvalidate) => {
  console.log('meg', meg.value)
  console.log('msg', msg.value)
  oninvalidate(() => {
    console.log('before')
  })
})
```
#### 3.停止跟踪 watchEffect 返回一个函数 调用之后将停止更新 
```js
import { watchEffect, ref } from 'vue'
let meg = ref<String>('fishx')
let msg = ref<String>('fishc')
const stop = watchEffect((oninvalidate) => {
  console.log('meg', meg.value)
  console.log('msg', msg.value)
  oninvalidate(() => {
    console.log('before')
  })
})
const stopWatch = () => stop()
```
#### 4.更多的配置项

| 配置项  | 更新时    |
|:----:|:----------:|
| pre  | 组件更新前执行    |
| sync | 强制效果始终同步触发 |
| post | 组件更新后执行    |
```ts
import { watchEffect, ref } from 'vue'
let meg = ref<String>('fishx')
let msg = ref<String>('fishc')
const stop = watchEffect(
  (oninvalidate) => {
    let ipt: HTMLInputElement = document.querySelector('#ipt') as HTMLInputElement
    console.log(ipt,'ellllll');
    oninvalidate(() => {
      console.log('before')
    })
  },
  {
    flush: 'post'
  }
)
const stopWatch = () => stop()
```
#### 5.onTrigger调试watchEffect
```ts
  {
    flush: 'post',
    onTrigger(e) {
      debugger
    }
  }
```
### 学习Vue3 第十二章(认识组件和生命周期)
#### 1.组件基础
- 每一个.vue 文件呢都可以充当组件来使用
- 每一个组件都可以复用
![组件基础](https://gitee.com/zhijianggg/pic/raw/master/img/components.png)
#### 2.组件的生命周期
![](https://gitee.com/zhijianggg/pic/raw/master/img/lifecycle.png)
##### 1.onBeforeMount()
- 在组件DOM实际渲染安装之前调用。在这一步中，根元素还不存在。
##### 2.onMounted()
- 在组件的第一次渲染后调用，该元素现在可用，允许直接DOM访问
##### 3.onBeforeUpdate()
- 数据更新时调用，发生在虚拟 DOM 打补丁之前。
##### 4.updated()
- DOM更新后，updated的方法即会调用
##### 5.onBeforeUnmounted()
- 在卸载组件实例之前调用。在这个阶段，实例仍然是完全正常的。
##### 6.onUnmounted()
- 卸载组件实例后调用。调用此钩子时，组件实例的所有指令都被解除绑定，所有事件侦听器都被移除，所有子组件实例被卸载。
```html
<template>
  <div id="hello">我是hello 组件</div>
  <div>{{ count }}</div>
  <button @click="count++">+1</button>
</template>

<script setup lang="ts">
import { onBeforeMount,onMounted,onBeforeUpdate,onUpdated,onBeforeUnmount,onUnmounted,ref } from 'vue'
console.log('setup');
const count = ref(0)
onBeforeMount(()=> {
  let div = document.querySelector('#hello')
  console.log('创建之前 ===> onBeforeMount',div);
})
onMounted(()=>{
  let div = document.querySelector("#hello")
  console.log('创建完成 ===> onMounted', div);
})
onBeforeUpdate(()=> {
  let div = document.querySelector('#hello')
  console.log('更新之前 ===> onBeforeUpdate',div);
})
onUpdated(()=> {
  let div = document.querySelector('#hello')
  console.log('更新完毕 ==> onUpdated',div);
})
onBeforeUnmount(()=> {
  console.log('卸载之前 ==> onBeforeUnmount');
})
onUnmounted(()=>{
  console.log('卸载完毕 ==> onUnmounted' );
})
</script>

<style></style>
```
##### 7.Hook function
| 选项式 API         | 钩子内部设置      |  翻译 |
|:---------------:|:-----------------:|:-----:|
| beforeCreate    | x                 |之前创建|
| created         | x                 | 创建 |
| beforeMount     | onBeforeMount     |安装前|
| mounted         | onMounted         |安装|
| beforeUpdate    | onBeforeUpdate    |更新前|
| updated         | onUpdated         |更新|
| beforeUnmount   | onBeforeUnmount   |卸载前|
| unmounted       | onUnmounted       |卸载|
| errorCaptured   | onErrorCaptured   |错误捕获|
| renderTracked   | onRenderTracked   |渲染跟踪|
| renderTriggered | onRenderTriggered |渲染触发|
| activated       | onActivated       |激活|
| deactivated     | onDeactivated     |停用|

### 学习Vue3 第十三章（实操组件和认识less 和 scoped）
#### 1.vite 安装 less
```shell
npm install less less-loader -D
```
#### 2.什么是scoped
- 实现组件的私有化, 当前style属性只属于当前模块.
- 在DOM结构中可以发现,vue通过在DOM结构以及css样式上加了`唯一标记`,达到样式私有化,`不污染全局`的作用
#### 3.样式穿透 `/deep/`
- 修改第三方组件样式

### 学习Vue3 第十四章（父子组件传参)
#### 1.父组件传值
- 父组件通过`v-bind`绑定一个数据，然后子组件通过`defineProps接受`传过来的值
  ##### (1) 传递字符串类型不需要加v-bind 
```html
<template>
  <div class="layout">
    <Menu title="首页"></Menu>
  </div>
</template>

<script setup lang="ts">
import Menu from './Menu/index.vue'
const list = reactive<number[]>([1, 2, 3])
</script>
```
  ##### (2) 传递非字符串类型需要加v-bind (`简写:`)
```html
<template>
  <div class="layout">
    <Menu :data="list" ></Menu>
  </div>
</template>

<script setup lang="ts">
import Menu from './Menu/index.vue'
import { reactive } from 'vue' 
const list = reactive<number[]>([1, 2, 3])
</script>
```
#### 2.子组件接受值
- 通过defineProps 来接受 **defineProps是无须引入的直接使用即可**
- 如果我们使用的`TypeScript`,可以使用传递字面量类型的纯类型语法做为参数`这是TS特有的`
  ##### (1) 使用的是TS
  - TS 特有的默认值方式
    - withDefaults是个函数也是无须引入开箱即用接受一个props函数第二个参数是一个对象设置默认值 
```html
<!--父组件传的是string-->
<template>
  <div class="menu"> 菜单区域
    <div>
      {{ title }}
    </div>
  </div>
</template>

<script setup lang="ts">
type Prop = {
  title: String
}
defineProps<Prop>()
</script>
``` 
```html
<!--父组件传的非string类型-->
<template>
  <div class="menu">菜单区域
    <div>{{ data }}</div>
  </div>
</template>

<script setup lang="ts">
import { reactive } from 'vue'
type Prop = {
  title: String
  data: number[]
}
</script>
```
  ##### (2) 没使用TS
```js
defineProps({
    title:{
        default:"",
        type:string
    },
    data:Array
})
``` 
#### 3.子组件给父组件传参
- 是通过defineEmits派发一个事件
```html
<!--子组件传值-->
<template>
  <div class="menu">菜单区域
    <div>
      <button @click="clickTap">派发</button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { reactive } from 'vue'
const list = reactive<Number[]>([11, 22, 33, 44, 55, 66, 77, 88, 99])
const emit = defineEmits(['on-click'])
const clickTap = () => {
  emit('on-click', list)
} 
defineProps<Prop>()
</script>
```
- 绑定了一个点击事件 => 通过`defineEmits` 注册 => 自定义事件
- 事件流程: 点击click => 触发 emit 调用 => 自定义事件 => 然后传递参数
```html
<!--父组件接收-->
<template>
  <div class="layout">
    <Menu @on-click="getlist" :data="list" title="首页"></Menu>
  </div>
</template>

<script setup lang="ts">
import Menu from './Menu/index.vue'
import { reactive } from 'vue'
const list = reactive<number[]>([1, 2, 3])
const getlist = (list: Number[]) => {
  console.log(list)
}
</script>
```
#### 4.子组件暴露给父组件内部属性
```html
<!--父组件-->
 <Menu ref="menus"></Menu>
 <script setup lang="ts">
  const menus = ref(null)
  console.log(menus.value)
</script>
```
此时,看不到任何,因为vue3对子组件进行保护了.
```ts
<!--子组件-->
const list = reactive<number[]>([4, 5, 6])
defineExpose({
    list
})
```
### 学习 vue3 第十五章(全局组件、局部组件、递归组件)
#### 1.全局组件
- 配置全局组件
1. `在components中新建Cart.vue`
```html
<template>
  <div class="card">
    <div class="card_header">
      <div>主标题</div>
      <div>副标题</div>
    </div>
    <div class="card_content" v-if="content">{{ content }}</div>
  </div>
</template>

<script setup lang="ts">
type Prop = {
  content?: string
}
defineProps<Prop>()
</script>

<style lang="less" scoped>
@border: #ccc;
.card {
  border: 1px solid @border;
  &:hover {
    box-shadow: 0 0 10px @border;
  }
  &_header {
    display: flex;
    justify-content: space-between;
    padding: 10px;
    border-bottom: 1px solid @border;
  }
  &_content {
    padding: 10px;
  }
}
</style>
```
2. `在main.ts中进行引入和挂载`
```ts
import Scard from './components/shopcart.vue'
createApp(App).component('Card', Card).mount('#app')
```
3. 使用
```html
<div>
  <Cart :content="`我是第${item}个`"></Cart>
</div>
```
#### 2.局部组件
- 配置局部组件
```html
<template>
  <Menu></Menu>
</template>

<script setup lang="ts">
import Menu from './Menu/index.vue'
</script>
```

#### 3.递归组件
- 配置递归组件
1. 新建`Tree/Tree.vue`文件
2. 父组件配置数据结构(数组对象格式) 传给子组件
```html
<template>
  <div class="menu">
    菜单区域
    <Tree :data="data"></Tree>
  </div>
</template>
<script>
import {reactive} from 'vue'
type TreeList = {
  name:string,
  icon?:string,
  children?:TreeList[] | []
}
const data = reactive<TreeList[]>([
  {
    name: 'no.1',
    children:[{
      name:'no.1的儿子',
      children:[{
        name:'no.1的孙子'
      }]
    }]
  },
  {
    name: 'no.2',
    children:[{
      name:'no.2的儿子'
    }]
  },
  {
    name: 'no.3',
    children:[{
      name:'no.3的儿子'
    }]
  }
])
</script>
```
3. 子组件接收值
```html
<template>
  <div style="margin-left: 10px">
    <div v-for="(item, index) in data" :key="index">
      {{ item.name }}
      <TreeItem v-if="item?.children?.length" :data="item.children"></TreeItem>
    </div>
  </div>
</template>

<script setup lang="ts">
type TreeList = {
  name:string,
  icon?:string,
  children?:TreeList[] | []
}
type Prop = {
  data?:TreeList[]
}
defineProps<Prop>()
</script>

<script lang="ts">
export default {
  name: 'TreeItem'
}
</script>
``` 
### 学习Vue3 第十六章（动态组件）
#### 1.概念
- 让多个组件使用同一个挂载点，并动态切换，这就是动态组件。
#### 2.使用
- 在挂载点使用component标签，然后使用`v-bind:is="组件"`
- 在`setup`语法糖里,需要`:`;不使用`setup`用`export default`可以不用`v-bind`
1. 新建三个组件`A.vue`、`B.vue`、`C.vue`
```html
<template>
  <div class="a">AAAAAAA</div>
</template>

<style lang="less" scoped>
.a {
  margin: 10px;
  height: 300px;
  line-height: 300px;
  text-align: center;
  background: #a2c1de;
  border: 1px solid #ccc;
}
</style>
```
2. 在`index.vue`中引入这三组件
3. 定义数据类型和定义数据 
```ts
type Tabs = {
  name:string,
  comName:any
}
const data = reactive<Tabs[]>([
   {
     name: '我是A组件',
     comName: markRaw(A)
   },
   {
     name: '我是B组件',
     comName: markRaw(B)
   },
   {
     name: '我是C组件',
     comName: markRaw(C)
   }
])
// 使用了reactive就会让porxy代理浪费性能,使用markRaw跳过代理
```
4. 再到`<template>`,对数据进行循环遍历渲染,以及组件内渲染
```html
<div class="tab">
   <div class="_div" v-for="item in data" :key= "item.name">
        {{item.name}}
   </div>
</div>
<component :is="current.comName"></component>
```
5. 定义类型,实现切换组件
```ts 
type Com = Pick<Tabs,'comName'> // 摘出Tabs类型里的comName
let current = reactive<Com>({
   ComName: data[0].comName
})
```
6. 点击切换
```html
<div class="_div" v-for="item in data" :key= "item.name" @click="SwitchCom(item)">
    {{item.name}}
</div>
<script setup lang="ts">
    const SwitchCom = (item: Tabs) => {
      current.comName = item.comName
    }
</script>
```
#### 3.使用场景
- tab切换 居多
#### 4.注意事项 
- 1.在Vue2 的时候is 是通过组件名称切换的 在Vue3 setup 是通过组件实例切换的
- 2.如果你把组件实例放到Reactive Vue会给你一个警告
    - 这是因为reactive 会进行proxy 代理 而我们组件代理之后毫无用处 节省性能开销 推荐我们使用shallowRef 或者  markRaw 跳过proxy 代理

### 学习Vue3 第十七章（插槽slot）
- 概念
    - 插槽就是子组件中的提供给父组件使用的一个占位符，用`<slot></slot>`表示
    - 父组件可以在这个占位符中填充任何模板代码，如 HTML、组件等，填充的内容会替换子组件的`<slot></slot>标签`
#### 1.匿名插槽
```html
<!--子组件-->
<div class="hearder">
    <slot></slot>
</div>
<!--父组件-->
<Dialog>
  <template>
    <div>我被插入了---上面</div>
  </template>
</Dialog>
```
#### 2.具名插槽
- 具名插槽其实就是给插槽取个名字。
- 一个子组件可以放多个插槽，而且可以放在不同的地方，而父组件填充内容时，可以根据这个名字把内容填充到对应插槽中
```html
<!--子组件-->
<div class="hearder">
    <slot name="hearder"></slot>
</div>
<!--父组件-->
<Dialog>
  <template v-slot:hearder>
    <div>我被插入了---上面</div>
  </template>
</Dialog>
```
- 简写
```html
<Dialog>
  <template #hearder>
    <div>我被插入了---上面</div>
  </template>
</Dialog>
```
#### 3.作用域插槽
- 在子组件动态绑定参数 派发给父组件的slot去使用
```html
<!--子组件定义数据-->
<template>
   <main class="main">
      <div v-for="item in data" :key="item.name">
        <slot :data="item"></slot>
      </div>
   </main>
</template>

<script setup lang="ts">
type names = {
  name: string,
  age: number
}
const data = reactive<names[]>([
   {
     name: fishx,
     age: 20
   },
   ···
])
</script>
```
```html
<!--父组件--> 
<template #default="{data}">
    <div>姓名: {{data.name}} 今年 {{data.age}}了</div>
</template>
```
#### 4.动态插槽
```html
<!--父组件-->
<Dialog>
   <template #[name]>
      <div>我在哪?</div>
   </template>
</Dialog>
```

```ts
let name = ref('footer')
```
### 学习Vue3 第十八章（异步组件&代码分包&suspense）

### 学习Vue3 第十九章（Teleport传送组件）
#### 1.概念
- Teleport 是一种能够将我们的模板渲染至指定DOM节点，不受父级style、v-show等属性影响，但data、prop数据依旧能够共用的技术；类似于 React 的 Portal。
- 主要解决的问题 
    - 因为Teleport节点挂载在其他指定的DOM节点下，完全不受父级style样式影响
#### 2.使用方法
  - 通过to 属性 插入指定元素位置 to="body" 便可以将Teleport 内容传送到指定位置
```html
<Teleport to="body">
    <Loading></Loading>
</Teleport>
```
- 可以自定义传送位置 支持`class`、`id`等 选择器
```html
<!--传送组件-->
<div id="app"></div>
<div class="modal"></div>
```
```html
<!--指定传送位置-->
<Teleport to=".modal">
   <Loading></Loading>
</Teleport>
```
### 学习Vue3 第二十章（keep-alive缓存组件）
- 不希望组件被重新渲染影响使用体验；
- 处于性能考虑，避免多次重复渲染降低性能
- 缓存组件,维持当前的状态。这时候就需要用到keep-alive组件。
```html
<template>
  <div class="content">
    <button @click="switchCom">切换</button>
    <keep-alive>
      <Login v-if="flag"></Login>
      <Register v-else></Register>
    </keep-alive>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import Login from '../../components/login/index.vue'
import Register from '../../components/register/index.vue'

const flag = ref(true)
const switchCom = () => {
  flag.value = !flag.value
}
</script>
```
#### 1.开启keep-alive 生命周期的变化
- 初次进入时： onMounted> onActivated
- 退出后触发 deactivated
- 再次进入：
    - 只会触发 onActivated
- 事件挂载的方法等，只执行一次的放在 onMounted中；组件每次进去执行的方法放在 onActivated中
#### 2. include、exclude 和 Max 
- include 和 exclude prop 允许组件有条件地缓存
	- 二者都可以用逗号分隔字符串、正则表达式或一个数组来表示
```html
<keep-alive :include="" :exclude="" :max=""></keep-alive>
```
### 学习Vue3 第二十一章（transition动画组件）
#### 1.在下列情形中，可以给任何元素和组件添加进入/离开过渡:
- 条件渲染 (使用 v-if)
- 条件展示 (使用 v-show)
- 动态组件
- 组件根节点
#### 2.过渡 class
- v-enter-from：定义进入过渡的开始状态。在元素被插入之前生效，在元素被插入之后的下一帧移除。 
- v-enter-active：定义进入过渡生效时的状态。在整个进入过渡的阶段中应用，在元素被插入之前生效，在过渡/动画完成之后移除。这个类可以被用来定义进入过渡的过程时间，延迟和曲线函数。
- v-enter-to：定义进入过渡的结束状态。在元素被插入之后下一帧生效 (与此同时 v-enter-from 被移除)，在过渡/动画完成之后移除。 
- v-leave-from：定义离开过渡的开始状态。在离开过渡被触发时立刻生效，下一帧被移除。 
- v-leave-active：定义离开过渡生效时的状态。在整个离开过渡的阶段中应用，在离开过渡被触发时立刻生效，在过渡/动画完成之后移除。这个类可以被用来定义离开过渡的过程时间，延迟和曲线函数。 
- v-leave-to：离开过渡的结束状态。在离开过渡被触发之后下一帧生效 (与此同时 v-leave-from 被移除)，在过渡/动画完成之后移除。
```html
  <button @click='flag = !flag'>切换</button>
  <transition name='fade'>
    <div v-if='flag' class="box"></div>
  </transition>
```
```css
//开始过度
.fade-enter-from{
   background:red;
   width:0px;
   height:0px;
   transform:rotate(360deg)
}
//开始过度了
.fade-enter-active{
  transition: all 2.5s linear;
}
//过度完成
.fade-enter-to{
   background:yellow;
   width:200px;
   height:200px;
}
//离开的过度
.fade-leave-from{
  width:200px;
  height:200px;
  transform:rotate(360deg)
}
//离开中过度
.fade-leave-active{
  transition: all 1s linear;
}
//离开完成
.fade-leave-to{
  width:0px;
   height:0px;
}
```
#### 3.自定义过渡 class 类名

trasnsition props

*   `enter-from-class`
*   `enter-active-class`
*   `enter-to-class`
*   `leave-from-class`
*   `leave-active-class`
*   `leave-to-class`
#### 4.自定义过度时间(单位毫秒)
```html
<transition :duration="1000">...</transition>
<transition :duration="{ enter: 500, leave: 800 }">...</transition>
```
#### 5.通过自定义class 结合css动画库animate.css[(官方文档)](https://animate.style/)
* 安装库`npm install animate.css`
* 引入 `import 'animate.css'`
* transition 生命周期8个
    - 当只用 JavaScript 过渡的时候，**在 enter 和 leave 钩子中必须使用 done 进行回调**
```css
  @before-enter="beforeEnter" //对应enter-from
  @enter="enter"//对应enter-active
  @after-enter="afterEnter"//对应enter-to
  @enter-cancelled="enterCancelled"//显示过度打断
  @before-leave="beforeLeave"//对应leave-from
  @leave="leave"//对应enter-active
  @after-leave="afterLeave"//对应leave-to
  @leave-cancelled="leaveCancelled"//离开过度打断
```
```html
    <transition
      @before-enter="EnterFrom"
      @enter="EnterActive"
      @after-enter="EnterTo"
      @enter-cancelled="EnterCancel"
      @before-leave="LeaveFrom"
      @leave="Leave"
      @after-leave="LeaveTo"
      @leave-cancelled="LeaveCancel"
    >
      <div v-if="flag" class="box"></div>
    </transition>
```
```ts
<!--el代表当前元素,:定义元素类型-->
const EnterFrom = (el: Element) => {
  console.log('进入之前')
}
const EnterActive = (el: Element, done: Function) => {
  console.log('过度曲线')
  setTimeout(() => {
    done()
  }, 5000)
}
const EnterTo = (el: Element) => {
  console.log('过度完成')
}
const EnterCancel = (el: Element) => {
  console.log('过渡效果被打断了!')
}
const LeaveFrom = () => {
  console.log('离开之前!')
}
const Leave = (el: Element, done: Function) => {
  console.log('离开过渡的曲线')
  setTimeout(() => {
    done()
  },1000)
}
const LeaveTo = () => {
  console.log('离开之前')
}
const LeaveCancel = () => {
  console.log('离开过渡的曲线被打断了')
}

```
#### 6.结合gsap 动画库使用 GreenSock[(官网)](https://greensock.com/)
1. 下载 `npm i gsap`
2. 导入 `import gsap from 'gsap'`
3. 使用
```html
<transition @before-enter="EnterFrom" @enter="EnterActive" @leave="Leave">
  <div v-if="flag" class="box"></div>
</transition>
```
```ts
const EnterFrom = (el: Element) => {
  gsap.set(el, {
    width: 0,
    height: 0
  })
}
const EnterActive = (el: Element, done: gsap.Callback) => {
  gsap.to(el, {
    width: 200,
    height: 200,
    onComplete: done
  })
}
const Leave = (el: Element, done: gsap.Callback) => {
  gsap.to(el, {
    width: 0,
    height: 0,
    onComplete: done
  })
}
```
#### 7.页面加载完成就开始动画(对应三个状态)
* 首次加载页面完毕就执行动画
```html
<transition
      appear
      appear-from-class="from"
      appear-active-class="active"
      apper-to-class="to"
>
    <div v-if="flag" class="box"></div>
</transition>
```
```css
.box {
  width: 200px;
  height: 200px;
  background-color: #f00;
}
.from {
  width: 0;
  height: 0;
}
.active {
  transition: all 2s ease;
}
```
### 学习Vue3 第二十二章（transition-group过度列表）
#### 1.过渡动画列表
* 默认情况下，它不会渲染一个包裹元素，但是你可以通过 tag attribute 指定渲染一个元素。
* 过渡模式不可用，因为我们不再相互切换特有的元素。
* 内部元素总是需要提供唯一的 key attribute 值。
* CSS 过渡的类将会应用在内部的元素中，而不是这个组/容器本身。
```html
<template>
  <div class="content">
    <button @click="add">ADD</button><button @click="sub">Pop</button>
    <div class="wraps">
      <transition-group
        leave-active-class="animate__animated animate__backOutUp"
        enter-active-class="animate__animated animate__backInUp"
      >
        <div class="item" :key="item" v-for="item in list">{{ item }}</div>
      </transition-group>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { ref, reactive } from 'vue'
import 'animate.css'
const list = reactive<number[]>([1, 2, 3, 4, 5, 6])
const add = () => {
  list.push(list.length + 1)
}
const sub = () => {
  list.pop()
}
</script>

<style scoped lang="less">
.wraps {
  display: flex;
  flex-wrap: wrap;
  word-break: break-all;
  border: 1px solid #ccc;
  .item {
    margin: 10px;
  }
}
</style>
```
#### 2.列表的移动过渡 
* <transition-group> 组件还有一个特殊之处。除了进入和离开，它还可以为**定位的改变添加动画**
* 案例:随机排列数盘
    * 1.下载随机排列的js库
        * `npm i lodash -S`
    * 2.下载ts说明文件
        * `npm install @types/lodash -D` 
```html
<template>
  <div>
    <transition-group move-class="move" tag="div" class="wraps">
      <div class="item" v-for="item in list" :key="item.id">{{item.number}}</div>
    </transition-group>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import _ from 'lodash'
// 占位并生成数字
let list = ref(
  Array.apply(null, { length: 81 } as number[]).map((_, index) => {
    return {
      id: index,
      number: (index % 9) + 1
    }
  })
)
// 将数字打乱
const random = () => {
  list.value = _.shuffle(list.value)
}
setInterval(()=>{
  random()
},2000)
</script>

<style lang="less" scoped>
.wraps{
  display: flex;
  flex-wrap: wrap;
  margin: 200px auto;
  width: calc(25px * 10 + 9px);
  text-align: center;
  .item{
    width: 25px;
    height: 25px;
    border: 1px solid #ccc;
  }
}
.move{
  transition: all 1s;
}
</style>
```
#### 3.状态过渡
* Vue 也同样可以给数字 Svg 背景颜色等添加过渡动画
```html
<template>
  <div>
    <input v-model="num.current" type="number" step="20" />
    <div>
      {{ num.tweenedNumber }}
    </div>
  </div>
</template>

<script lang="ts" setup>
import { watch, reactive } from 'vue'
import gsap from 'gsap'

const num = reactive({
  current: 0, // 当前为0
  tweenedNumber: 0 // 状态也为0
})

watch(
  () => num.current,
  (newVal, oldVal) => {
    gsap.to(num, {
      duration: 1, // 过渡时间
      tweenedNumber: newVal // 状态变化后新值
    })
  }
)
</script>

<style scoped lang="less"></style>
```
### 学习Vue3 第二十三章（依赖注入Provide / Inject）
#### 1.使用实例
* 父组件传递数据
```html
<template>
  <div class="App">
    <h1>我是根组件</h1>
    <A></A>
  </div>
</template>

<script setup lang="ts">
import { provide, ref, reactive } from 'vue'
import A from './layout/Content/A.vue'
provide('flag', ref(true))
</script>
```
* 子组件接受
```html
<template>
  <div class="B">
    <h1>我是BBB组件</h1>
    <button @click="changeFlag">改变</button>
    <div>{{ data }}</div>
  </div>
</template>

<script setup lang="ts">
import { inject,Ref,ref } from 'vue'

let data = inject<Ref<boolean>>('flag',ref(false))

const changeFlag = () => {
  data.value = !data.value
}
</script>

<style scoped lang="less">
.B {
  width: 200px;
  height: 200px;
  background-color: rgb(0, 157, 255);
  color: #000;
}
</style>
```
#### 2.使用场景
* 当父组件有很多数据需要分发给其子代组件的时候， 就可以使用provide和inject。
### 学习Vue3 第二十四章（兄弟组件传参和Bus）
#### 1.借助父组件传参
`A => 父组件 => B`
```html
<!--A组件-->
<template>
  <div class="A">
    <button @click="emitB">派发一个事件</button>
  </div>
</template>

<script setup lang="ts">
const emit = defineEmits(['on-click'])
let flag = false
const emitB = () => {
  flag = !flag
  emit('on-click',flag)
}
</script>

<style lang="less" scoped>
.A {
  width: 200px;
  height: 200px;
  background-color: aliceblue;
  color: #000;
}
</style>
```
```html
<!--父组件-->
<template>
  <div>
    <A @on-click="getflag"></A>
    <B :flag="Flag"></B>
  </div>
</template>

<script setup lang="ts">
import A from './components/A.vue'
import B from './components/B.vue'
import { ref } from 'vue'
let Flag = ref(false)
const getflag = (params: boolean) => {
  Flag.value = params
}
</script>

<style lang="less" scoped></style>
```
```html
<!--B组件-->
```html
<template>
  <div class="B">
    <h1>B组件</h1>
    {{ flag }}
  </div>
</template>

<script setup lang="ts">
type Props = {
  flag: boolean
}
defineProps<Props>()
</script>

<style lang="less" scoped>
.B {
  width: 200px;
  height: 200px;
  background-color: skyblue;
  color: #000;
}
</style>
```
#### 2.Event Bus
* `Bus.ts` 
* 原理: 运用了JS设计模式之发布订阅模式
```ts
type Busclass = {
  emit: (name: string) => void
  on: (name: string, callback: Function) => void
}

type ParmsKey = string | number | symbol

type List = {
  [key: ParmsKey]: Array<Function>
}
class Bus implements Busclass {
  list: List
  constructor() {
    this.list = {}
  }

  emit(name: string, ...args: Array<any>) {
    let eventName: Array<Function> = this.list[name]
    eventName.forEach((fn) => {
      fn.apply(this, args)
    })
  }

  on(name: string, callback: Function) {
    let fn: Array<Function> = this.list[name] || []
    fn.push(callback)
    this.list[name] = fn
  }
}
export default new Bus()
```
* `A => B`
```html
<template>
  <div class="A">
    <button @click="emitB">派发一个事件</button>
  </div>
</template>

<script setup lang="ts">
import Bus from '../Bus'
let flag = false
const emitB = () => {
  flag = !flag
  Bus.emit('on-click', flag)
}
</script>

<style lang="less" scoped>
.A {
  width: 200px;
  height: 200px;
  background-color: aliceblue;
  color: #000;
}
</style>
```
```html
<!--B组件-->
<template>
  <div class="B">
    <h1>B组件</h1>
    {{ Flag }}
  </div>
</template>

<script setup lang="ts">
import Bus from '../Bus'
import { ref } from 'vue'
let Flag = ref(false)
Bus.on('on-click', (flag: boolean) => {
  Flag.value = !flag
})
</script>

<style lang="less" scoped>
.B {
  width: 200px;
  height: 200px;
  background-color: skyblue;
  color: #000;
}
</style>
```
### 学习Vue3 第二十五章（TSX）
#### 1.安装插件
* `npm install @vitejs/plugin-vue-jsx -D`
* vite.config.ts 配置
```ts
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import vueJsx from '@vitejs/plugin-vue-jsx';
// https://vitejs.dev/config/
export default defineConfig({
  plugins: [vue(),vueJsx()]
})
```
#### 2.修改tsconfig.json 配置文件
```json
"jsx": "preserve",
"jsxFactory": "h",
"jsxFragmentFactory": "Fragment",
```
#### 3.使用TSX
* tsx不会自动解包使用ref加.vlaue
##### 1.TSX支持`v-model`
```ts
import { ref } from 'vue'
let v = ref<string>('')
const renderDom = () => {
  return (
    <>
      <div>Hello,tsx!</div>
      <input v-model={v.value} type="text" />
      <div>{v.value}</div>
    </>
  )
}
export default renderDom
```
##### 2.TSX支持`v-show`
```ts
import { ref } from 'vue'
let flag = ref(false)
const renderDom = () => {
  return (
    <>
      <div v-show={flag.value}>惊天</div>
      <div v-show={!flag.value}>动地</div>
    </>
  )
}
export default renderDom
```
##### 3.TSX不支持`v-if`(需要转换风格,用三元解决)
```ts
import { ref } from 'vue'
let v = ref(false)
const renderDom = () => {
  return <>{v.value ? <div>惊天</div> : <div>动地</div>}</>
}
export default renderDom
```
##### 4.v-for也是不支持的(需要使用Map)
```ts
let arr = [1, 2, 3, 4, 5]
const renderDom = () => {
  return (
    <>
      {arr.map((v) => {
        return <div>${v}</div>
      })}
    </>
  )
}
export default renderDom
```
##### 5.v-bind使用(直接赋值使用)
`return <div data-arr={v}>${v}</div>`

##### 6.v-on绑定事件(须按照react风格)
* 所有事件有on开头
* 所有事件名称首字母大写
```ts
let arr = [1, 2, 3, 4, 5]

const renderDom = () => {
  return (
    <>
      {arr.map((v) => {
        return (
          <div onClick={clickTap} data-arr={v}>
            ${v}
          </div>
        )
      })}
    </>
  )
}

const clickTap = () => {
  console.log('我被点了')
}
export default renderDom
```
```ts
let arr = [1, 2, 3, 4, 5]

const renderDom = () => {
  return (
    <>
      {arr.map((v) => {
        return (
          <!--clickTap通过bind传参(第一个参数是this指向,第二个是需传递的参数)-->
          <div onClick={clickTap.bind(this, v)} data-arr={v}>
            ${v}
          </div>
        )
      })}
    </>
  )
}

const clickTap = (v: number) => {
  console.log('我被点了' + v)
}
export default renderDom
```
##### 7.Props 接受值
```ts
type Props = {
  title: string

}
const renderDom = (props: Props) => {
  return (
    <div>
      <div>{props.title}</div>
    </div>
  )
}
export default renderDom
```
```html
<template>
  <div>
    <renderDom title="我是标题"></renderDom>
  </div>
</template>
```
##### 8.Emit派发
```ts
let Arr = [1, 2, 3, 4, 5]

type Props = {
  title: string
}
const renderDom = (props: Props, ctx: any) => {
  return (
    <div>
      <div>{props.title}</div>
      {Arr.map((v) => {
        return <div onClick={clickTap.bind(this, ctx)} data-index={v}>*{v}</div>
      })}
    </div>
  )
}

const clickTap = (ctx:any) => {
  ctx.emit('on-click',123)
}
export default renderDom
```
```html
<template>
  <div>
    <renderDom @on-click="getNum" title="我是标题"></renderDom>
  </div>
</template>

<script setup lang="ts">
import renderDom from './App'
const getNum = (num: number) => {
  console.log(num,'接收到咯')
}
</script>

<style lang="less" scoped></style>
```
### 学习Vue3 第二十六章（深入v-model）
#### 1.Vue3自动引入插件
* 安装
    * `npm i -D unplugin-auto-import/vite` 
* vite配置
```js
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import VueJsx from '@vitejs/plugin-vue-jsx'
import AutoImport from 'unplugin-auto-import/vite'
// https://vitejs.dev/config/
export default defineConfig({
  plugins: [vue(),VueJsx(),AutoImport({
    imports:['vue'],
    dts:"src/auto-import.d.ts"
  })]
})
````
#### 2.v-model(是props 和 emit的组合)
##### 1.默认值的改变
*   prop：`value` -> `modelValue`；
*   事件：`input` -> `update:modelValue`；
*   `v-bind` 的 `.sync` 修饰符和组件的 `model` 选项已移除
*   新增 支持多个v-model
*   新增 支持自定义 修饰符
##### 2.使用
```html
<!--子组件-->
<template>
  <div v-if="modelValue" class="dialog">
    <div class="dialog_header">
      <div>标题---{{title}}</div>
      <div @click="close" style="cursor: pointer">x</div>
    </div>
    <div class="dialog_content">内容</div>
  </div>
</template>

<script setup lang="ts">
type Props = {
  modelValue: boolean
  title: string
}

const emit = defineEmits(['update:modelValue','update:title'])
defineProps<Props>()

const close = () => {
  emit('update:modelValue', false)
  emit('update:title', "我不是🐶")
}
</script>

<style lang="less" scoped>
.dialog {
  width: 300px;
  height: 300px;
  border: 1px solid #ccc;
  position: fixed;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  &_header {
    border-bottom: 1px solid #ccc;
    display: flex;
    justify-content: space-between;
    padding: 10px;
  }
  &_content {
    padding: 10px;
  }
}
</style>
```
```html
<template>
  <div>
    <button @click="flag = !flag">改变{{ flag }}</button>
    <div>标题 {{ title }}</div>
    <Dialog v-model:title="title" v-model="flag"></Dialog>
  </div>
</template>

<script setup lang="ts">
import Dialog from './components/Dialog/Dialog.vue'
import { ref } from 'vue'
let flag = ref<boolean>(true)
let title = ref<string>('我是个帅哥')
</script>

<style lang="less" scoped></style>
```
##### 3.自定义修饰符
* 添加到组件 v-model 的修饰符将通过 modelModifiers prop 提供给组件。
```ts
type Props = {
    modelValue: boolean,
    title?: string,
    modelModifiers?: {
        default: () => {}
    }
    titleModifiers?: {
        default: () => {}
    }
 
}
 
const propData = defineProps<Props>()
 
const emit = defineEmits(['update:modelValue', 'update:title'])
 
const close = () => {
    console.log(propData.modelModifiers);
 
    emit('update:modelValue', false)
    emit('update:title', '我要改变')
}
```
### 学习Vue3 第二十七章（自定义指令directive）
* directive-自定义指令（属于破坏性更新）
#### 1.Vue3指令的钩子函数
* created 元素初始化的时候 
* beforeMount 指令绑定到元素后调用 只调用一次 
* mounted 元素插入父级dom调用 
* beforeUpdate 元素被更新之前调用 
* update 这个周期方法被移除 改用updated 
* beforeUnmount 在元素被移除前调用 
* unmounted 指令被移除后调用 只调用一次
#### 2.生命周期钩子参数详解
* 第一个 el 当前绑定的DOM 元素 
* 第二个 binding instance：使用指令的组件实例。 
    * value：传递给指令的值。例如，在 v-my-directive="1 + 1" 中，该值为 2。 
    * oldValue：先前的值，仅在 beforeUpdate 和 updated 中可用。无论值是否有更改都可用。 
    * arg：传递给指令的参数(如果有的话)。例如在 v-my-directive:foo 中，arg 为 "foo"。 
    * modifiers：包含修饰符(如果有的话) 的对象。例如在 v-my-directive.foo.bar 中，修饰符对象为 {foo: true，bar: true}。 
    * dir：一个对象，在注册指令时作为参数传递 
```html
<template>
  <div>
    <button @click="flag = !flag">off</button>
    <B v-if="flag" v-move:fishx="{ background: 'aliceblue' }"></B>
  </div>
</template>

<script setup lang="ts">
import { ref, Directive, DirectiveBinding } from 'vue'
import B from './layout/Content/B.vue'
let flag = ref<boolean>(true)
type Dir = {
  background: string
}
const vMove: Directive = {
  created() {
    console.log('=====> created')
  },
  beforeMount() {
    console.log('====> beforeMount')
  },
  mounted(el: HTMLElement, dir: DirectiveBinding<Dir>) {
    console.log('===> mounted')
    el.style.background = dir.value.background
  },
  beforeUpdate() {
    console.log('===> beforeUpdate')
  },
  updated() {
    console.log('===> updated')
  },
  beforeUnmount() {
    console.log('===> 在元素被移除前调用')
  },
  unmounted() {
    console.log('===> unmounted')
  }
}
</script>

<style lang="less" scoped></style>
```
```html
<template>
  <div class="B">
    <h1>我是BBB组件</h1>
    <button @click="changeFlag">改变</button>
    <div>{{ data }}</div>
  </div>
</template>

<script setup lang="ts">
import { inject, Ref, ref } from 'vue'

let data = inject<Ref<boolean>>('flag', ref(false))

const changeFlag = () => {
  data.value = !data.value
}
</script>

<style scoped lang="less">
.B {
  width: 200px;
  height: 200px;
  background-color: rgb(0, 157, 255);
  color: #000;
}
</style>
```
#### 3.在setup内定义局部指令
* 必须以 vNameOfDirective 的形式来命名本地自定义指令，以使得它们可以直接在模板中使用。
```html
<template>
  <button @click="show = !show">开关{{show}} ----- {{title}}</button>
  <Dialog  v-move-directive="{background:'green',flag:show}"></Dialog>
</template>
```
```ts
const vMoveDirective: Directive = {
  created: () => {
    console.log("初始化====>");
  },
  beforeMount(...args: Array<any>) {
    // 在元素上做些操作
    console.log("初始化一次=======>");
  },
  mounted(el: any, dir: DirectiveBinding<Value>) {
    el.style.background = dir.value.background;
    console.log("初始化========>");
  },
  beforeUpdate() {
    console.log("更新之前");
  },
  updated() {
    console.log("更新结束");
  },
  beforeUnmount(...args: Array<any>) {
    console.log(args);
    console.log("======>卸载之前");
  },
  unmounted(...args: Array<any>) {
    console.log(args);
    console.log("======>卸载完成");
  },
}
```
#### 4.函数简写(mounted 和 updated 时触发相同行为)
```html
<template>
  <div>
    <input type="text" v-model="value" />
    <B v-move="{ background: value }"></B>
  </div>
</template>

<script setup lang="ts">
import { ref, Directive, DirectiveBinding } from 'vue'
import B from './layout/Content/B.vue'

let value = ref<string>('')
type Dir = {
  background: string
}
const vMove: Directive = (el: HTMLElement, background: DirectiveBinding<Dir>) => {
  el.style.background = background.value.background
}
</script>

<style lang="less" scoped></style>
```
```html
<template>
  <div class="B">
    <h1>我是BBB组件</h1>
  </div>
</template>

<script setup lang="ts"></script>

<style scoped lang="less">
.B {
  width: 200px;
  height: 200px;
  background-color: rgb(0, 157, 255);
  color: #000;
}
</style>
```
#### 5.案例: 自定义拖拽指令
* 拖拽有三步:
    * 鼠标按下 => 鼠标拖拽 => 鼠标抬起,清除事件
```html
<template>
  <div v-move class="box">
    <div class="header"></div>
    <div>内容</div>
  </div>
</template>

<script setup lang="ts">
import { Directive, DirectiveBinding } from 'vue'

const vMove: Directive<any, void> = (el: HTMLElement) => {
  let moveElement: HTMLDivElement = el.firstElementChild as HTMLDivElement
  const moveDown = (e: MouseEvent) => {
    let x = e.clientX - el.offsetLeft
    let y = e.clientY - el.offsetTop
    const move = (e: MouseEvent) => {
      console.log(e)
      el.style.left = e.clientX - x + 'px'
      el.style.top = e.clientY - y + 'px'
    }
    document.addEventListener('mousemove', move)
    document.addEventListener('mouseup', () => {
      document.removeEventListener('mousemove', move)
    })
  }
  moveElement.addEventListener('mousedown', moveDown)
}
</script>

<style lang="less" scoped>
.box {
  position: fixed;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  width: 200px;
  height: 200px;
  border: 1px solid black;
  .header {
    height: 20px;
    background: black;
    cursor: move;
  }
}
</style>
```
### 学习Vue3 第二十八章（自定义Hooks）
* 自定义Hook
    * 主要用来处理复用代码逻辑的一些封装
#### 1.自定义Hook(vue3) `vs` mixins(vue2)
* vue2的mixins
    * mixins就是将多个相同的逻辑抽离出来，各个组件只需要引入mixins，就能实现一次写代码，多组件受益的效果。
    * 弊端
        * 涉及到覆盖的问题
            * 组件的data、methods、filters会覆盖mixins里的同名data、methods、filters。
            * 变量来源不明确（隐式传入），不利于阅读，使代码变得难以维护。
* vue3的自定义Hook[(hook库)](https://vueuse.org/guide/)
    * Vue3 的 hook函数 相当于 vue2 的 mixin, 不同在与 hooks 是函数
    * Vue3 的 hook函数 可以帮助我们提高代码的复用性
#### 2.案例: 将图片转码(=> base64)
```html
<template>
  <div>
    <img id="img" width="410" height="190" src="./assets/垂钓.jpg" />
  </div>
</template>

<script setup lang="ts">
import useBase64 from './Hook'

useBase64({ el: '#img' }).then((res) => {
  console.log(res.baseUrl);
})
</script>

<style scoped></style>
```

```ts
// 转图片为base64具体实现代码
import { onMounted } from "vue"

type Options = {
  el: string
}

export default function (options: Options): Promise<{ baseUrl: string }> {
  return new Promise((resolve) => {
    onMounted(() => {
      let img: HTMLImageElement = document.querySelector(
        options.el
      ) as HTMLImageElement
      console.log(img, '=====>')
      img.onload = () => {
        resolve({
          baseUrl: base64(img)
        })
      }
    })

    const base64 = (el: HTMLImageElement) => {
      const canvs = document.createElement('canvas')
      const ctx = canvs.getContext('2d')
      canvs.width = el.width
      canvs.height = el.height
      ctx?.drawImage(el, 0, 0, canvs.width, canvs.height)
      return canvs.toDataURL('image/png')
    }
  })

}
function resolve(arg0: { baseUrl: string }) {
  throw new Error("Function not implemented.")
}
```
### 学习Vue3 第二十九章（Vue3定义全局函数和变量）
#### 1.globalProperties
```ts
// 之前 (Vue 2.x)
Vue.prototype.$http = () => {}
```
```ts
// 之后 (Vue 3.x)
const app = createApp({})
app.config.globalProperties.$http = () => {}
```
#### 2.过滤器
* 4/10 推倒不通
### 学习Vue3 第三十章（编写Vue3插件）
#### 1.概念
* 插件
    * 自包含的代码，通常向 Vue 添加全局级功能
#### 2.使用
* 在使用 createApp() 初始化 Vue 应用程序后，你可以通过调用 use() 方法将插件添加到你的应用程序中。
#### 3.案例(Loading)
* Loading.Vue
```html
<template>
    <div v-if="isShow" class="loading">
        <div class="loading-content">Loading...</div>
    </div>
</template>
    
<script setup lang='ts'>
import { ref } from 'vue';
const isShow = ref(false)//定位loading 的开关
 
const show = () => {
    isShow.value = true
}
const hide = () => {
    isShow.value = false
}
//对外暴露 当前组件的属性和方法
defineExpose({
    isShow,
    show,
    hide
})
</script>

<style scoped lang="less">
.loading {
    position: fixed;
    inset: 0;
    background: rgba(0, 0, 0, 0.8);
    display: flex;
    justify-content: center;
    align-items: center;
    &-content {
        font-size: 30px;
        color: #fff;
    }
}
</style>
```
* Loading.ts
```ts
import {  createVNode, render, VNode, App } from 'vue';
import Loading from './index.vue'
 
export default {
    install(app: App) {
        //createVNode vue提供的底层方法 可以给我们组件创建一个虚拟DOM 也就是Vnode
        const vnode: VNode = createVNode(Loading)
        //render 把我们的Vnode 生成真实DOM 并且挂载到指定节点
        render(vnode, document.body)
        // Vue 提供的全局配置 可以自定义
        app.config.globalProperties.$loading = {
            show: () => vnode.component?.exposed?.show(),
            hide: () => vnode.component?.exposed?.hide()
        }
    }
}
```
* Main.ts
```ts
import Loading from './components/loading'
let app = createApp(App)
app.use(Loading)
type Lod = {
    show: () => void,
    hide: () => void
}
//编写ts loading 声明文件放置报错 和 智能提示
declare module '@vue/runtime-core' {
    export interface ComponentCustomProperties {
        $loading: Lod
    }
}
app.mount('#app')
```
### 学习Vue3 第三十一章（了解UI库ElementUI，AntDesigin等）
* ElementUI
#### 1.安装
- `npm install element-plus --save`
#### 2.引入
* 在`main.ts`中进行相应引入
```ts
import { createApp } from 'vue'
import ElementPlus from 'element-plus'
import 'element-plus/dist/index.css'
import App from './App.vue'
const app = createApp(App)
app.use(ElementPlus)
app.mount('#app')
```
#### 3.volar插件支持
* 在`tsconfig.json`中添加配置
```json
{
  "compilerOptions": {
    // ...
    "types": ["element-plus/global"]
  }
}
```
* AntDesigin
- 安装 `npm install ant-design-vue@next --save`
- 使用
```ts
import { createApp } from 'vue';
import Antd from 'ant-design-vue';
import App from './App';
import 'ant-design-vue/dist/antd.css';
const app = createApp(App);
app.use(Antd).mount('#app');
```
#### 4.样式穿透(自定义样式)
* ~~/deep/~~ or deep()

## 学习Router
因为Vue是单面应用没有那么多`Html`去跳转,所以要使用路由做页面的跳转

Vue路由允许我们通过不同的URL去访问不同的内容.

通过Vue可以实现多视图的单页Web应用
### 学习Router第一章安装和使用

## 学习菠萝🍍 ---> 全局状态管理工具
### 学习Pinia 第一章（介绍Pinia)
#### 1.特点
- 完整的 ts 的支持；
- 足够轻量，压缩后的体积只有1kb左右;
- 去除 mutations，只有 state，getters，actions；
- actions 支持同步和异步；
- 代码扁平化没有模块嵌套，只有 store 的概念，store 之间可以自由使用，每一个store都是独立的
- 无需手动添加 store，store 一旦创建便会自动添加；
- 支持Vue3 和 Vue2
#### 2.安装
```shell
npm install pinia
```
#### 3.引入注册Vue3
```ts
import { createApp } from 'vue'
import App from './App.vue'
import {createPinia} from 'pinia'
const store = createPinia()
let app = createApp(App)
app.use(store)
app.mount('#app')
```
#### 4.vue2使用pinia
```js
import { createPinia, PiniaVuePlugin } from 'pinia'
 
Vue.use(PiniaVuePlugin)
const pinia = createPinia()
 
new Vue({
  el: '#app',
  // other options...
  // ...
  // note the same `pinia` instance can be used across multiple Vue apps on
  // the same page
  pinia,
})
```
### 学习Pinia 第二章（初始化仓库Store）
#### 1.新建一个文件夹Store
* `src/store`
#### 2.新建文件\[name\].ts
* `src/store/index.ts`
#### 3.定义仓库Store
* 存储是使用定义的defineStore()，并且它需要一个`唯一的名称`，作为第一个参数传递
    * State 箭头函数 返回一个对象 在对象里面定义值
```ts
import { defineStore } from 'pinia'
import { Name } from './store-name'
export const Test = defineStore(Name.TEST, {
  state: () => {
    return {
      current: 1,
      name: 'fishx'
    }
  },
  // 类似于computed 修饰一些值
  getters: {},
  // 类似于methods 可以操作异步 和 同步提交state
  actions: {}
})
```
* 此处将name名称抽离了`src/store/store-name`
```ts
export const enum Name {
  TEST = 'TEST'
}
```
* name名称，也称为id，是必要的
    * Pania 使用它来将商店连接到 devtools
#### 4.使用
```html
<template>
  Pinia: {{ Test.current }} ===> {{ Test.name }}
</template>

<script setup lang="ts">
import { useTestStore } from './store'
const Test = useTestStore()
</script>
```
### 学习Pinia 第三章（State）
#### 1.State 是允许直接修改值的 例如current++
```html
<template>
  Pinia: {{ Test.current }} ===> {{ Test.name }}
  <button @click="change">+1</button>
</template>

<script setup lang="ts">
import { useTestStore } from './store'
const Test = useTestStore()
const change = () => {
  Test.current++
}
</script>
```
#### 2.批量修改State的值($patch方法)
```html
<template>
  Pinia: {{ Test.current }} ===> {{ Test.name }}
  <button @click="change">改变</button>
</template>

<script setup lang="ts">
import { useTestStore } from './store'
const Test = useTestStore()
const change = () => {
  Test.$patch({
    current: 1000,
    name: 'fishc'
  })
}
</script>
```
#### 3.批量修改函数形式
* $patch ——> 推荐使用函数形式 可以自定义修改逻辑
```ts
const change = () => {
  Test.$patch((state) => {
    state.current = 999,
    state.name = '范冰冰'
  })
}
```
#### 4.通过原始对象($state)修改整个实例(弊端:必须修改整个对象)
```ts
const change = () => {
  Test.$state = {
    current: 111,
    name: 'admin123'
  }
}
```
#### 5.通过actions修改

* 1.`src/store/index.ts`
```ts
actions: {
 setCurrent () {
    this.current = 567
 }
}
```

* 2.调用
```ts
const change = () => {
  Test.setCurrent()
}
```

* 支持传参
```ts
actions: {
 setCurrent (num:number) {
    this.current = num
 }
}
// 分割线
const change = () => {
  Test.setCurrent(520)
}
```

### 学习Pinia 第四章（解构store）

#### 1.解构
```ts
const Test = useTestStore()
const { current, name } = Test
```
* 在Pinia是不允许直接解构的,因为这会失去响应性
```html
<!--失去了响应-->
<template>
  <div>原始值: {{ Test.current }}</div>
  Pinia: {{ current }} ===> {{ name }}
  <button @click="change">改变</button>
</template>

<script setup lang="ts">
import { useTestStore } from './store'
const Test = useTestStore()
const { current, name } = Test
const change = () => {
  Test.current++
}
</script>
```
#### 2.解决(使用 storeToRefs)
```ts
import { storeToRefs } from 'pinia';
import { useTestStore } from './store'
const Test = useTestStore()
const { current, name } = storeToRefs(Test)
const change = () => {
  Test.current++
}
```
#### 3.storeToRefs解决原理
* 与toRefs 一样的给里面的数据包裹一层toref
* 源码 => toRaw => store 变回原始数据 => 防止重复代理
* 循环 store 通过 isRef isReactive 判断如果是响应式对象直接拷贝一份给 refs 对象将其原始对象包裹 toRef 使其变为响应式对象

### 学习Pinia 第五章（Actions，getters）
#### 1. Actions（支持同步、异步）
##### 1.同步 ——> 直接调用即可
* `src/index.ts`
```ts
import { defineStore } from 'pinia'
import { Name } from './store-name'

type User = {
  name: string
  age: number
}

let result:User = {
   name: 'fishx',
   age: 123
}
export const useTestStore = defineStore(Name.TEST, {
  state: () => {
    return {
      user: <User>{},
      name: ' '
    }
  },
  actions: {
    setUser () {
      this.user = result
    }
  }
})
```
##### 2.异步 ——> 可以结合(async、await)修饰
```ts
import { defineStore } from 'pinia'
import { Name } from './store-name'

type User = {
  name: string
  age: number
}

const Login = () => {
  return new Promise<User>((resolve) => {
    setTimeout(() => {
      resolve({ name: 'fishx', age: 20 })
    }, 2000)
  })
}
export const useTestStore = defineStore(Name.TEST, {
  state: () => {
    return {
      user: <User>{},
      name: 'fishc'
    }
  },
  
  // 类似于methods 可以去做同步和异步操作,提交state
  actions: {
    async setUser() {
      const result = await Login()
      this.user = result
  }
})
```
##### 3.多个action互相调用getLoginInfo、setName
```ts
import { defineStore } from 'pinia'
import { Name } from './store-name'

type User = {
  name: string
  age: number
}

const Login = () => {
  return new Promise<User>((resolve, reject) => {
    setTimeout(() => {
      resolve({ name: 'fishx', age: 20 })
    }, 2000)
  })
}
export const useTestStore = defineStore(Name.TEST, {
  state: () => {
    return {
      user: <User>{},
      name: 'fishc'
    }
  },
  // 类似于methods 可以去做同步和异步操作,提交state
  actions: {
    async setUser() {
      const result = await Login()
      this.user = result
      this.setName('fishx')
    },
    setName(name: string) {
      this.name = name
    }
  }
})
```
* `App.vue`
```html
<template>
  <div>
    <p>actions-user: {{ Test.user }}</p>
    <hr />
    <button @click="change">change</button>
  </div>
</template>

<script setup lang="ts">
import { useTestStore } from './store'

const Test = useTestStore()

const change = () => {
  Test.setUser()
}
</script>
```
#### 2.getters
* 使用箭头函数不能使用 this, this 指向已经改变指向 undefined 修改值请用 state 
##### 1.使用箭头函数不能使用 this
> this 指向已经改变指向 undefined 修改值请用 state

```ts
getters:{
    newPrice:(state)=>  `$${state.user.price}`
}
```
##### 2.普通函数形式可以使用 this 
```ts
getters: {
  newName():string{
    return `$-${this.name}`
  }
},
```
##### 3.getters 互相调用
```ts
getters: {
  newName(): string {
    return `$-${this.name}-${this.getUserAge}`
  },
  getUserAge(): number {
    return this.user.age
  }
}
```
### 学习Pinia 第六章（API）
#### 1.$reset
* 重置store到他的初始状态
* 调用$reset() `-->`将会把state所有值 重置回 原始状态
```html
<button @click="change">change</button>
```

```ts
const reset = () => {
  Test.$reset()
}
```
#### 2.订阅state的改变
* 类似于Vuex 的abscribe  只要有state 的变化就会走这个函数
```ts
Test.$subscribe((args, state) => {
  console.log('======>', args)
  console.log('======>', state )
})
```
* 第二个参数
    - 如果你的组件卸载之后还想继续调用请设置第二个参数
```ts
Test.$subscribe((args,state)=>{
   console.log(args,state);
},{
  detached:true
})
```
#### 3.订阅Actions的调用
* 只要有actions被调用就会走这个函数
```ts
Test.$onAction((args)=>{
   console.log(args);
})
```
### 学习Pinia 第七章（pinia插件）

