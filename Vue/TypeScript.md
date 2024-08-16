# 一、邂逅 TypeScript 语法
## 1.1 TypeScript 的编译环境

>[!info]- 说明
> - 一般 vite、webpack 等模块打包器内置了对应的编译环境
> - 以下配置仅限于无打包工具环境下手动配置

- 安装 ts-node
	```shell
	npm install ts-node -g
	```
- 另外 ts-node 需要依赖 tslib 和 @types/node 两个包：
	```shell
	npm install tslib @types/node -g
	```
- 现在可以直接通过 ts-node 来运行 TypeScript 的代码：
	```shell
	ts-node math.ts
	```

## 1.2 变量的声明
在 TypeScript 中定义变量需要指定标识符[^1]的类型

声明了类型后 TypeScript 就会进行类型检测，<u>声明的数据类型</u>可以称之为**类型注解（Type Annotation）**
```js
var/let/const 标识符: 数据类型 = 赋值;
```

## 1.3 变量的类型推导/推断
声明类型之后 TypeScript 就会进行类型检测、类型推断出对应的数据类型
![[Pasted image 20230315202118.png]]
一个变量第一次赋值时，会根据后面的赋值内容的类型，来推断出变量的类型

## 1.4 Js 和 Ts 的数据类型
TypeScript 是 JavaScript 的一个超集：
![[Pasted image 20230315202326.png]]

### Js 类型 —— number
TypeScript 和 JavaScript 一样，不区分整数类型（int）和浮点型（double），统一为 number 类型。
同时支持二进制、八进制、十六进制的表示
![[Pasted image 20230315202512.png]]

### Js 类型 —— boolean
boolean 类型只有两个取值：true 和 false
![[Pasted image 20230315202546.png]]

### Js 类型 —— string
- string 类型是字符串类型，可以使用单引号或者双引号表示
	![[Pasted image 20230315202637.png]]
- 同时也支持 ES6的模板字符串来拼接变量和字符串：
	![[Pasted image 20230315202647.png]]
* string vs String
	 * string 是基本数据类型，String 是 String 类型的对象
	 * string 是 TypeScript 中的关键字，而 String 是 JavaScript 中的关键字
### Js 类型 —— Array
 数组类型的定义有两种方式
 ```ts
 const array_1:string[] = ['a','b','c']
 // Array<string>事实上是一种泛型的写法
 const array_2: Array<string> = ['a','b','c']
 ```

### Js 类型 —— Object
object 对象类型可以用于描述一个对象：
![[Pasted image 20230315212051.png]]
一个空对象, 无法读取
实际需要使用 `interface`、`type` 定义类型, 或者默认类型推断

### Ts 类型 —— unknown
- unknown 是 TypeScript 中比较特殊的一种类型，它用于描述类型不确定的变量
	- 和 any 类型有点类似，但是 unknown 类型的值上做任何事情都是不合法的
```ts
let foo: unknown = 'aaa'
foo = 123

// unknown 类型默认情况下在上面进行任意的操作都是非法的
// 要求必须进行类型的校验(缩小),才能根据缩小之后的类型,进行对应的操作
if(typeof foo === 'string') { // 类型缩小/限定
  console.log(foo.length)
}
```

### Js 类型 —— Symbol
- 在 ES5中，如果我们是不可以在对象中添加相同的属性名称的
	![[Pasted image 20230315213119.png]]
- 通常我们的做法是定义两个不同的属性名字：比如 identity1和 identity2。
- 但是我们也可以通过 symbol 来定义相同的名称，因为 Symbol 函数返回的是不同的值
	![[Pasted image 20230315213127.png]]

### Js 类型 —— null、undefined
- 在 JavaScript 中，undefined 和 null 是两个基本数据类型。
- 在 TypeScript 中，它们各自的类型也是 undefined 和 null，也就意味着它们既是实际的值，也是自己的类型
![[Pasted image 20230315213304.png]]

### 函数的类型
- 具名函数
	- 函数是 JavaScript 非常重要的组成部分，TypeScript 允许我们指定函数的参数和返回值的类型。
	- 参数的类型注解
		- 声明函数时，可以在每个参数后添加类型注解，以声明函数接受的参数类型：
			![[Pasted image 20230315213537.png]]
	- 函数的返回值类型
		- 也可以添加返回值的类型注解，这个注解出现在函数列表的后面：
			![[Pasted image 20230315214012.png]]
		- 和变量的类型注解一样，我们通常情况下不需要返回类型注解
			- 因为 TypeScript 会根据 return 返回值推断函数的返回类型
			- 某些第三方库处于方便理解，会明确指定返回类型，看个人喜好
- 匿名函数
	- 当一个函数出现在 TypeScript 可以确定该函数会被如何调用的地方时；
	- 该函数的参数会自动指定类型:
		![[Pasted image 20230315214259.png]]
	- 并没有指定 item 的类型，但是 item 是一个 string 类型：
		- 这是因为 TypeScript 会根据 forEach 函数的类型以及数组的类型推断出 item 的类型
		- 这个过程称之为**上下文类型（contextual typing）**，因为函数执行的上下文可以帮助确定参数和返回值的类型


* 对象类型
	* 希望限定一个函数接受的参数是一个对象
		* 可以使用对象类型
			![[Pasted image 20230315214940.png]]
		* 在对象我们可以添加属性，并且告知 TypeScript 该属性需要是什么类型
		* 属性之间可以使用 , 或者 ; 来分割，最后一个分隔符是可选的
		* 每个属性的类型部分也是可选的，如果不指定，那么就是 any 类型
		* `属性名?:数据类型` 是可选类型, 非必须

### Ts 类型 —— any 
- 在某些情况下，我们确实无法**确定一个变量的类型，并且可能它会发生一些变化**，这个时候我们**可以使用 any 类型**（类似于 Dart 语言中的 dynamic 类型）
	* 可以对 any 类型的变量*进行任何的操作*，包括获取不存在的属性、方法；
	* 给一个 any 类型的变量*赋值任何的值*，比如数字、字符串的值
### Ts 类型 —— void 
- void 通常用来指定一个函数是没有返回值的，那么它的返回值就是 void 类型：
	![[Pasted image 20230315220642.png]]
- 这个函数我们没有写任何类型，那么它默认返回值的类型就是 void 的，我们也可以显示的来指定返回值是 void
	![[Pasted image 20230315220702.png]]
- 当基于上下文的类型推导（Contextual Typing）推导出返回类型为 void 的时候，并不会强制函数一定不能返回内容
	![[Pasted image 20230315221259.png]]

### Ts 类型 —— never
- never 表示永远不会发生值的类型，比如一个函数
	- 如果一个函数中是一个死循环或者抛出一个异常，就使用 never 
		![[Pasted image 20230315221504.png]]

### Ts 类型 —— tuple
- tuple 是元组类型，很多语言中也有这种数据类型
	![[Pasted image 20230315221613.png]]
- tuple 和数组有什么区别?
	- 首先，数组中通常建议存放相同类型的元素，不同类型的元素是不推荐放在数组中。（可以放在对象或者元组中）
	- 其次，元组中每个元素都有自己特性的类型，根据索引值获取到的值可以确定对应的类型
![[Pasted image 20230315221758.png]]

- Tuple 的应用场景
	- tuple 通常可以作为返回的值
		![[Pasted image 20230315221840.png]]

# 二、TypeScript 语法细节
## 2.1 联合类型和交叉类型
- 联合类型: **表示多个类型中一个即可**
	- 联合类型是由两个或者多个其他类型组成的类型
	- 表示可以是这些类型中的任何一个值
	- 联合类型中的每一个类型被称之为联合成员（union's members）
	- TypeScript 的类型系统允许我们使用多种运算符，从现有类型中构建新类型
	```ts
	// 联合类型
	function printID(id: number | string) {
	  console.log(`Your ID is: ${id}`)
	  // 类型缩小
	  if (typeof id === 'string') {
	    console.log(id.length)
	  } else {
	    console.log(id)
	  }
	}
	```
- 交叉类型: **表示需要满足多个类型的条件**
	- 类型别名不能被 extends 和 implements
		- 但可以使用交叉类型实现
		```ts
		interface Point {
		  x: number
		  y: number
		}
		// 需要同时满足 Point 和 {z: number} 
		type Point3D = Point & {
		  z: number
		}
		```
	- 交叉类型常应用于对象类型进行交叉的
		```ts
		interface Colorful {
		  color: string
		}
		interface Circle {
		  radius: number
		}
		type newType = Colorful & Circle
		const circle: newType = {
		  color: 'red',
		  radius: 2
		}
		```

## 2.2 type 和 interface 使用
- 通过 type 可以用来声明一个对象类型：
	```ts
	type Ponit = {
	  x: number
	  y: number
	}
	```
- 对象的另外一种声明方式就是通过接口来声明
	```ts
	interface Point {
	  x: number
	  y: number
	}
	```
>[!note]- interface 和 type 区别
> - 区别一: 类型别名可以直接赋值, type 类型使用范围更广, 接口类型只能用来声明对象
> 	```ts
> 		// 1. 类型别名可以声明基本类型
> 		Type Name = string
> 		Const str: Name = 'zhangsan'
> 		// 2. 类型别名, 可以用于声明一个函数, 还可以声明联合类型 
> 		type NameResolver = () => string
> 		Type NameOrResolver = string | NameResolver
> 	```
> - 区别二: 在声明对象时, interface 可以多次声明, 并合并
> 	```ts
> 	// 接口可以被多次声明合并
> 	interface Point {
> 	  x: number,
> 	  y: number
> 	}
> 	interface Point {
> 	  z?: number
> 	}
> 	const point: Point = {
> 	  x: 1, y: 2, z:3
> 	}
> 	```
> - 区别三: interface 支持实现、继承
> 	```ts
> 	// 类型别名不能被 extends 和 implements
> 	interface Point {
> 	  x: number,
> 	  y: number
> 	}
> 	interface Ponit3D extends Point {
> 	  z: number
> 	}
> 	const point3D: Ponit3D = {
> 	  x: 1, y: 2, z:3
> 	}
> 	```

>[!abstract]- interface 和 type 的抉择
>- 如果是非对象类的定义 => type
>- 如果时对象类型的定义/声明 => interface

>[!abstract]- interface 和 class 的继承/实现
>- 接口可以继承接口 (, 隔开可多继承), 而类是单继承
>- 类可以实现接口 (, 隔开可多实现)
## 2.3 类型断言和非空断言

> 类型断言 as

```ts
const imgEle = document.querySelector('.img')
imgEle.src = './1.jpg'
imgEle.alt = '图片'
```
出现报错: `imgEle` 元素的自动类型推导为 Element 和 null 的联合类型
![[Pasted image 20230316181507.png]]
那能不能使用 `类型限定` 解决 => <span style="color: #F1616A">依旧报错</span>

使用 as 断言 => <span style="color: #2DD081">问题解决</span>
![[Pasted image 20230316182307.png]]

>[!warning]- 注意
>1. 断言只能够「欺骗」TypeScript 编译器
>	- 无法避免运行时的错误，滥用断言可能会掩盖真实的类型错误
>2. 断言只能断言成更加具体的类型
>	- 编译器检测允许断言成范围更大、不太具体的类型 (any、unknown)
>	- 但从语法角度来看, 这是不合理的
>3. as 断言的使用, 应当是缩小类型范围、使得类型更加安全

> 非空断言 !

```ts
interface Person {
  name: string;
  age: number;
  // ? 表示可选属性
  children?: {
    name: string;
  }
}

const person: Person = {
  name: 'zhangsan',
  age: 18,
}

// 属性访问:
// ?. 可选链只能用于可选属性
console.log(person.children?.name);

// 属性赋值:
// person.children?.name = 'lisi';赋值表达式的左侧不能是可选属性访问

// 解决方案1: 使用类型限定
if (person.children) person.children.name = 'lisi';

// 解决方案2: 使用类型断言
(person.children as { name: string }).name = 'lisi';

// 解决方案3: 使用非空类型断言
person.children!.name = 'lisi';
```
>[!warning]- 非空类型断言是个危险操作!
>- 非空类型断言规则:
>	1. 只能用于可选属性访问
>	2. 只能用于赋值表达式的左侧
>	3. 只能用于函数调用的参数
## 2.4 字面量类型和类型缩小/限定

><span style="font-size:24px">字符串字面量类型</span>: 用来约束取值只能是某几个字符串中的一个

```ts
// 字面量类型的基本使用
const name: 'fishx' = 'fishx'
let age: 19 = 19

// 为了防止全局变量冲突，使用export {}将其封装起来
export {}
```

多个字面量类型联合起来 `|` 
```ts
// 字面量类型的联合类型
type Direction = 'LEFT' | 'RIGHT' | 'UP' | 'DOWN'
// Direction可以是四个方位,其中一个string类型
let toWhere: Direction = 'LEFT'
// toWhere = 'LEFT1' // 报错
```

例子: 封装请求方法
```ts
type MethodType = "get" | "post"

function request(url: string, method: MethodType) {}

request("http://fishx.com/api","post")
```
Ts 细节
```ts
const info = {
  url: 'http://fishx.com/api',
  method: 'get'
}

// 类型“string”的参数不能赋给类型“MethodType”的参数
request(info.url, info.method) 
```

```ts
// 解决方案一: 类型断言
request(info.url, info.method as MethodType)
```

`as const` 保证 info 的值不会被修改
```ts
// 解决方案二: `as const` 保证info的值不会被修改
const info = {
  url: 'http://fishx.com/api',
  method: 'get'
} as const
```
![[Pasted image 20230316202609.png]]

>[!note]- 字面量类型和通用类型的区别
>  - 字面量类型: 
> 	 - const 声明的变量, 字面量类型是**可以**添加属性的
> 	 - 但是不能修改/删除属性、值、属性的类型、值的类型的值,
> - 通用类型: 
> 	- let 声明的变量, 通用类型是**不可以**添加属性的
> 	- 也不能修改/删除属性、值、属性的类型、值的类型的值


><span style="font-size:24px">类型缩小</span>: 通过类似于 `typeof num === 'number'` 的判断语句, 来改变 Ts 的执行路径
- 常见的类型保护
	- typeof
	- 平等缩小 `===` `!==`
	- `instanceof`
	- `in`
	- ...
- 在 TypeScript 中，检查返回的值 typeof 是一种类型保护：
	```ts
	// 1. typeof
	function printID(id: number | string) {
	  if (typeof id === 'string') {
	    console.log(id.length, id.split(''))
	  } else {
	    console.log(id)
	  }
	}
	```
- 平等缩小: 可以使用 Switch 或者相等的一些运算符来表达相等性（比如`===, !==, ==, !=` ）
	```ts
	// 2. 平等缩小
	type Direction = 'LEFT' | 'RIGHT' | 'UP' | 'DOWN'
	function switchDirection(direction: Direction) {
	  if (direction === 'LEFT') {
	    console.log('左:', '角色向左移动')
	  } else if (direction === 'RIGHT') {
	    console.log('右:', '角色向右移动')
	  } else if (direction === 'UP') {
	    console.log('上:', '角色向上移动')
	  } else {
	    console.log('下:', '角色向下移动')
	  }
	}
	```
- JavaScript 有一个运算符 `instanceof` 来检查一个值是否是另一个值的“实例” 
	```ts
	// 3. instanceof
	function printDate(date: string | Date) {
	  if (date instanceof Date) {
	    console.log(date.toLocaleDateString())
	  }
	  if (typeof date === 'string') {
	    console.log(date)
	  }
	}
	```
- Javascript 有一个运算符，用于确定对象是否具有带名称的属性：in 运算符
	- 如果指定的属性在指定的对象或其原型链中，则 in 运算符返回 true；
	```ts
	// 4. in
	interface Fish {
	  swim: () => void
	}
	interface Bird {
	  fly: () => void
	}
	function move(animal: Fish | Bird) {
	  if ('swim' in animal) {
	    animal.swim()
	  } else {
	    animal.fly()
	  }
	}
	```

## 2.5 函数的类型和函数签名
> 函数的类型: [格式: (参数列表) => 返回值]
- 在 JavaScript 开发中，函数是重要的组成部分，并且函数可以**作为一等公民**
	- 可以作为参数，也可以作为返回值进行传递
- 函数类型的表达式（Function Type Expressions），来表示函数类型
	```ts
	// 格式: (参数列表) => 返回值
	type CalcFunc = (a: number, b: number) => number
	
	function calc(fn: CalcFunc) {
	  return fn(1, 2)
	}
	```
- `(a: number, b: number) => number`，代表的就是一个函数类型
	- 接收两个参数的函数：a、b，都是 number 类型且返回 number 类型

> 调用签名（Call Signatures）: [格式: (参数列表): 返回类型]

在 JavaScript 中，函数除了可以被调用，自己也是可以有属性值的
```ts
// 函数调用签名(从对象角度看待这函数,也可以有其他的属性)
interface Bar {
  name: string
  age: number
  // 函数可以调用签名
  (a: number, b: number): number
}

/* 
  使用let声明就会报错,而使用const声明就不会报错
  报错:类型 "(a: number, b: number) => number" 中
  缺少属性 "age"但类型 "Bar" 中需要该属性
  let bar: Bar = (a: number, b: number) => {
    return a + b
  }
 */

/** 为什么使用const不会报错呢?
 * 因为const声明的变量是字面量类型,字面量类型是可以添加属性的
 * 但是let声明的变量是通用类型,通用类型是不可以添加属性的
*/
const bar: Bar = (a: number, b: number) => {
  return a + b
}
bar.name = 'bar'
bar.age = 18
bar(1, 2)
```

>[!abstract]- 函数的类型和函数签名, 在开发中如何选择?
>1. 如果只是描述函数类型本身[函数可以调用], 使用函数类型表达式 (Function Type Expressions)  
>2. 如果在描述函数作为对象可以被调用, 同时也有其他属性时, 使用函数调用签名 (Call Signatures)

> 参数的可选类型
- 可以指定某个参数是可选的：
	![[Pasted image 20230316232703.png]]
- 这个时候这个参数 y 依然是有类型的 number | undefined
- 另外可选类型需要在必传参数的后面：
	![[Pasted image 20230316232828.png]]

> 默认参数
- 从 ES6开始，JavaScript 是支持默认参数的，TypeScript 也是支持默认参数的：
	![[Pasted image 20230316232909.png]]
- 这个时候 y 的类型其实是 undefined 和 number 类型的联合。

> 剩余参数
- 从 ES6开始，JavaScript 也支持剩余参数，剩余参数语法允许我们将一个不定数量的参数放到一个数组中
	![[Pasted image 20230316233024.png]]
## 2.6 函数的重载和 this 类型
> 函数的重载
- 在 TypeScript 中，如果我们编写了一个 add 函数，希望可以对字符串和数字类型进行相加
	![[Pasted image 20230316233257.png]]
- 可以去编写不同的重载签名（overload signatures）来表示函数可以以不同的方式进行调用
	- 一般是编写两个或者以上的重载签名，再去编写一个通用的函数以及实现
```ts
function add(num1: string, num2: string): string
function add(num1: number, num2: number): number
function add(num1: any, num2: any) {
  return num1 + num2
}

add(1, 2)
add('1', '2')
```

> 联合类型和重载
- 有一个需求：定义一个函数，可以传入字符串或者数组，获取它们的长度： 
	- 方案一：使用联合类型来实现； 
		![[Pasted image 20230317001410.png]]
	- 方案二：实现函数重载来实现；
		![[Pasted image 20230317001427.png]]
- 在可能的情况下，尽量选择使用联合类型来实现

> 可推导的 [this](https://mp.weixin.qq.com/s/hYm0JgBI25grNG_2sCRlTA) 类型
- 在没有指定 this 的情况，this 默认情况下是 any 类型的
	![[Pasted image 20230317002356.png]]
- 配置 Ts
	- 初始化配置文件 `tsc --init`
	- 打开 `tsconfig.json` 文件, 取消 `"noImplicitThis": true,` 的注释
	![[Pasted image 20230317002909.png]]
- 指定 this 的类型
	- 在开启 noImplicitThis 的情况下，我们必须指定 this 的类型。
	- 如何指定呢？函数的第一个参数类型：
		- 函数的第一个参数我们可以根据该函数之后被调用的情况，用于声明 this 的类型（名词必须叫 this）
		- 在后续调用函数传入参数时，从第二个参数开始传递的，this 参数会在编译后被抹除
		![[Pasted image 20230317003203.png]]
- this 相关的内置工具[^2]
	- ThisParameterType
		- 用于提取一个函数类型 Type 的 this (opens new window)参数类型；
		- 如果这个函数类型没有 this 参数返回 unknown；
		![[Pasted image 20230317003516.png]]
	- OmitThisParameter
		- 用于移除一个函数类型 Type 的 this 参数类型, 并且返回当前的函数类型
		![[Pasted image 20230317003540.png]]
	- ThisType
		- 这个类型不返回一个转换过的类型，它被用作标记一个上下文的 this 类型
		![[Pasted image 20230317004214.png]]

# 三、TypeScript 面向对象
## 3.1 TypeScript 类的使用
### 类的定义
- 声明类的属性：在类的内部声明类的属性以及对应的类型
	- 如果类型没有声明，那么它们默认是 any 的
	- 在默认的 strictPropertyInitialization 模式下
		- 属性是必须初始化的,如果没有初始化，那么编译时就会报错
		- 不希望给属性初始化, 可以使用 name!: string 语法
- 构造函数不需要返回任何值，默认返回当前创建出来的实例
- 类中可以有自己的函数，定义的函数称之为方法	
```ts
class Person {
  // 成员属性: 用来描述对象的特征
  name!: string // 非空断言,name必有值
  // 可以给属性设置初始值
  age = 18

  // 构造函数, 用来初始化对象的成员属性赋值
  constructor(name: string, age: number) {
    this.name = name
    this.age = age
  }

  // 成员方法: 用来描述对象的行为
  sayHi() {
    console.log(`大家好, 我是${this.name}`)
  }
}

// 实例对象: instance
const p = new Person('孙悟空', 18)
const p2 = new Person('猪八戒', 28)

console.log(p.name, p.age)
export {}
```

### 类的继承
继承不仅仅可以减少我们的代码量，也是多态的使用前提
使用 extends 关键字来实现继承，子类中使用 super 来访问父类
```ts
class Student extends Person {
  sno: number
  constructor(name: string, age: number, sno: number) {
    super(name, age)
    this.sno = sno
  }
  sayHi(): void {
    super.sayHi()
    console.log(`我是${this.name}, 我的学号是${this.sno}`)
  }
}

const student = new Student('沙和尚', 38, 1001)
student.sayHi()
```

### 只读属性 
- 如果有一个属性我们不希望外界可以任意的修改，只希望确定值后直接使用，那么可以使用 readonly
![[Pasted image 20230317101234.png]]

### 参数属性
- 参数属性（parameter properties）
	- TypeScript 提供了特殊的语法，可以把一个构造函数参数转成一个同名同值的类属性
- 通过在构造函数参数前
	- 添加一个可见性修饰符 public private protected 
	- 或者 readonly 来创建参数属性
	- 最后这属性字段也会得到这些修饰符
```ts
class Man {
  // private name: string === private name 
  constructor(private name: string) {
    this.name = name
  }
  setName(name: string) {
    this.name = name
  }
  getName() {
    return this.name
  }
}
```

### 类的封装
- 在 TypeScript 中，类的属性和方法支持三种修饰符： public、private、protected
	- Public 修饰的是在任何地方可见、公有的属性或方法，默认编写的属性就是 public 的； 
	- Private 修饰的是仅在同一类中可见、私有的属性或方法；
	- Protected 修饰的是仅在类自身及子类中可见、受保护的属性或方法
- 类的属性设为 `private`
- 对外提供 `Getter、Setter` 方法
```ts
// [语法糖]类的参数使用修饰符, 可以直接定义成员属性
class Woman {
  constructor(private name: string, private age: number) {}
  setName(name: string) {
    this.name = name
  }
  getName() {
    return this.name
  }
  setAge(age: number) {
    this.age = age
  }
  getAge() {
    return this.age
  }
}

let woman: Woman = new Woman('小红', 18)
```
### 类的类型
类本身也是可以作为一种数据类型的
![[Pasted image 20230317144309.png]]

### Ts抽象类 
- 在 TypeScript 中没有具体实现的方法 (没有方法体)，就是抽象方法
	- 抽象方法，必须存在于抽象类中
	- 抽象类是使用 abstract 声明的类
- 抽象类特点
	- 抽象类是不能被实例的话（也就是不能通过 new 创建）
	- 抽象方法必须被子类实现，否则该类必须是一个抽象类
```ts
abstract class Shape {
  abstract getArea(): number;
}
```

```ts
class Circle extends Shape {
  constructor(private radius: number) {
    super();
    this.radius = radius;
  }
  getArea() {
    return 3.14 * this.radius ** 2;
  }
}

class Rectangle extends Shape {
  constructor(private width: number, private height: number) {
    super();
    this.width = width;
    this.height = height;
  }
  getArea() {
    return this.width * this.height;
  }
}
```

```ts
const calcArea = (shape: Shape) => {
  console.log(shape.getArea());
}
// 父类的引用指向子类的实例对象 => 多态向上转型
calcArea(new Circle(10));
calcArea(new Rectangle(10, 20));
```
>[!note]- TS 类型检测机制——鸭子类型
> - 什么是鸭子类型的 TS 类型检测机制: 
> 	- 如果一个对象的形状和接口的形状一致, 那么就可以赋值给这个接口

>[!cite] 什么是鸭子类型: 如果它走起路来像鸭子, 叫起来也像鸭子, 那么它就是鸭子

>[!cite] 什么是TS类型检测机制: TS 会在编译阶段检测代码的类型是否正确

## 3.2 TypeScript 对象类型
### 属性修饰符
- 对象类型中的每个属性可以说明它的类型、属性是否可选、属性是否只读
	- 可选属性（Optional Properties）
		- 属性名后面加一个 ? 标记表示这个属性是可选的
	- 只读属性（Readonly Properties）
		- 属性可以被标记为 readonly, 之后不允许修改

![[Pasted image 20230317144818.png]]
### 索引签名
* 索引签名 (index signature) 用来描述可能的值的类型
	 * 1. 索引签名的作用
		 - 作用: 用来描述对象中可以存在哪些属性, 以及属性的类型
	 * 2. 索引签名的语法
		- 语法: [属性名: 属性类型]: 属性值类型
	 * 3. 索引签名的限制
		 - 限制: 索引签名的属性名必须是字符串类型, 属性值的类型可以是任意类型


## 3.3 特殊: 严格字面量检测
![[Pasted image 20230317153803.png]]
- 每个对象字面量最初都被认为是“新鲜的（fresh）”。
- 当一个新的对象字面量分配给一个变量或传递给一个非空目标类型的参数时，对象字面量指定目标类型中不存在的属性是错误的。
- 当类型断言或对象字面量的类型扩大时，新鲜度会消失
## 3.4 TypeScript 枚举类型
- 枚举类型是为数不多的 TypeScript 特性有的特性之一： 
	- 枚举其实就是将一组可能出现的值，一个个列举出来，定义在一个类型中，这个类型就是枚举类型
	- 枚举允许开发者定义一组命名常量，常量可以是数字、字符串类型
```ts
enum Direction { UP, DOWN, LEFT, RIGHT }

const direction: Direction = Direction.UP

function turnDirection(direction: Direction) {
  switch (direction) {
    case Direction.UP:
      console.log('向上')
      break
    case Direction.DOWN:
      console.log('向下')
      break
    case Direction.LEFT:
      console.log('向左')
      break
    case Direction.RIGHT:
      console.log('向右')
      break
    default:
      break
  }
}
```

- 枚举类型的值
	- 枚举类型默认是有值的, 默认 0 且递增
		![[Pasted image 20230317162527.png]]
	- 也可以进行赋值 
		- 赋值类型为 `number` 会从第一项开始自增
		- 赋值类型为 `string` 不会递增, 要求每一项都有值
		![[Pasted image 20230317162710.png]]


# 四、TypeScript 泛型编程
Ts 类型的参数化: 通过泛型来实现类型的参数化
```ts
/* 普通函数写法: 
function bar<Type>(arg: Type): Type {
  return arg
} */

// 箭头函数写法:
const bar = <T>(arg: T): T => {
  return arg
}
type Person = {
  name: string
  age: number
}
// 使用: 完整写法
const res1 = bar<number>(123)
const res2 = bar<string>('123')
const res3 = bar<Person>({ name: 'zs', age: 23 })
// 使用: 类型推断
const res_1 = bar(123) // const 推断为字面量
let res_2 = bar('123') // let 推断为字符串(通用类型)
const res_3 = bar({ name: 'zs', age: 23 })
```
- 一些常用的名称：
	- T：Type 的缩写，类型 
	- K、V：key 和 value 的缩写，键值对 
	- E：Element 的缩写，元素 
	- O：Object 的缩写，对象
## 4.1 泛型接口、类的使用
> 泛型接口
```ts
// IDou<T = string> 默认为string
interface IDou<T> {
  name: T
  age: number
  slogan: T
}

const ikun: IDou<number> = {
  name: 123,
  age: 18,
  slogan: 456
}

const ikun1: IDou<string> = {
  name: 'ikun',
  age: 18,
  slogan: 'Hello World'
}
```
> 泛型类
```ts
class Point<T> {
  constructor(public x: T, public y: T) {
    this.x = x
    this.y = y
  }
}

const p1 = new Point(1, 2)
const p2 = new Point('1', '2')
```
## 4.2 泛型约束和类型条件
> 泛型约束 `extends` 
```ts
interface ILength {
  length: number
}

function getLength<T extends ILength>(arg: T): number {
  console.log(arg, arg.length)
  return arg.length
}

// getLength(1) // 报错
getLength('123') // 3 (String.length: number)
getLength([1, 2, 3]) // 3 (Array<number>.length: number)
getLength({ length: 100, name: 'fishx' }) // arg.length = 100
```

> 类型条件 (`keyof x`: 取出对象 x 中所有的 key)
```ts
// 传入的key类型,是obj的key类型的子集
// K extends keyof O(K是O的key的子集)
function getObjectProperty<O, K extends keyof O>(obj: O, key: K) {
  return obj[key]
}

const obj = {
  name: 'fishx',
  age: 18,
  gender: 'male'
}

const name = getObjectProperty(obj, 'name')
// const name = getObjectProperty(obj, 'fishx') // 报错
```

## 4.3 TypeScript 映射类型
> 映射类型（Mapped Types）
- 映射类型建立在索引签名的语法上：
```ts
// 映射类型: 通过已有的类型生成新的类型 —— 函数
type MapPerson<T> = {
  // 当 T = Person时, Key = 'name' | 'age'
  // [Key in 'name' | 'age']: string
  [Key in keyof T]: T[Key]
  // 索引类型以此进行使用
  // name: string
  // age: number
}

interface Person {
  name: string
  age: number
}
// 拷贝一份Person
type NewPerson = MapPerson<Person>
```

> - 映射修饰符（Mapping Modifiers）
> 	- 一个是 readonly，用于设置属性只读；
> 	- 一个是 ? ，用于设置属性可选

可以通过前缀 - 或者 + 删除或者添加这些修饰符，如果没有写前缀，相当于使用了 + 前缀
![[Pasted image 20230318003227.png]]

## 4.4 TypeScript 条件类型
- 需要**基于输入的值来决定输出的值**，同样我们**也需要基于输入的值的类型来决定输出的值的类型**
- 条件类型（Conditional types）就是用来帮助我们描述**输入类型和输出类型之间**的关系
	- `SomeType extends OtherType ? TrueType : FalseType;`
		- 有点类似三元判断

## 4.5 类型工具和类型体操
[类型体操](https://github.com/type-challenges/type-challenges)
[体操解答](https://ghaiklor.github.io/type-challenges-solutions/en/)

### 条件类型使用
- 条件类型的写法有点类似于 JavaScript 中的条件表达式（condition ? TrueExpression : falseExpression ）
	- 比如:  SomeType extends OtherType ? TrueType : FalseType;
```ts
type IDType = number | string

// 判断number是否是extends IDType
// const res = 2 > 3? true: false
type ResType = boolean extends IDType? true: false

function sum<T extends number | string>(num1: T, num2: T): T extends number? number:string
function sum(num1, num2) {
  return num1 + num2
}

const res = sum(20, 30)
const res2 = sum("abc", "cba")

export {}

```

### Inter 关键字

- 它用来在条件类型中推断
- 可以从正在比较的类型中推断类型，然后在 true 分支里引用该推断结果

```ts
type CalcFnType = (num1: number, num2: string) => number

function foo() {
  return "abc"
}
// 总结类型体操题目: MyReturnType
type MyReturnType<T extends (...args: any[]) => any> = T extends (...args: any[]) => infer R? R: never
type MyParameterType<T extends (...args: any[]) => any> = T extends (...args: infer A) => any? A: never

// 获取一个函数的返回值类型: 内置工具
type CalcReturnType = MyReturnType<CalcFnType>
type FooReturnType = MyReturnType<typeof foo>

type CalcParameterType = MyParameterType<CalcFnType>

export {}
```

### Distributive 关键字

- 用作分发条件类型
- 当在泛型中使用条件类型的时候，如果传入一个联合类型，就会变成分发的（distributive）

```ts
type toArray<T> = T extends any? T[]: never

type NumArray = toArray<number>

// number[]|string[] 而不是 (number|string)[]
type NumAndStrArray = toArray<number|string>
```



### 常见的 TypeScript 内置类型工具

内置类型工具

- `Partial<Type>` 
	- 用于构造一个 Type 下面的所有属性都设置为可选的类型
- `Required<Type>` 
	- 用于构造一个 Type 下面的所有属性全都设置为必填的类型，这个工具类型跟 Partial 相反。
- `Readonly<Type>` 
	- 用于构造一个 Type 下面的所有属性全都设置为只读的类型，意味着这个类型的所有的属性全都不可以重新赋值。
- `Record<Keys,Type>` 
	- 用于构造一个对象类型，它所有的 key (键)都是 Keys 类型，它所有的 value (值)都是 Type 类型。
- `Pick<Type,Keys>`
	- 用于构造一个类型，它是从 Type 类型里面挑了一些属性 Keys
- `Omit<Type,Keys>` 
	- 用于构造一个类型，它是从 Type 类型里面过滤了一些属性 Keys
- `Exclude<UnionType,ExcludedMembers>` 
	- 用于构造一个类型，它是从 UnionType 联合类型里面排除了所有可以赋给 ExcludedMembers 的类型
- `Extract<Type,Union>` 
	- 用于构造一个类型，它是从 Type 类型里面提取了所有可以赋给 Union 的类型。
- `NonNullable<Type>`
	- 用于构造一个类型，这个类型从 Type 中排除了所有的 null、undefined 的类型。
- `InstanceType<Type>`
	- 用于构造一个由所有 Type 的构造函数的实例类型组成的类型。







# 五、TypeScript 知识扩展
## 5.1 TypeScript 模块使用
TypeScript 中最主要使用的模块化方案就是 ES Module
```ts
// /utils/math.ts
export function add(a: number, b: number) {
  return a + b;
}

// /utils/type.ts
export interface Person {
  name: string
  age: number
}
// export type PersonKeys = export type PersonKeys = "name" | "age"
export type PersonKeys = keyof Person 
```
JavaScript 规范声明<u>任何没有 export 的 JavaScript 文件都应该被认为是一个脚本，而非一个模块</u>

在一个脚本文件中，<u>变量和类型会被声明在共享的全局作用域</u>，将多个输入文件合并成一个输出文件，或者在 HTML 使用多个 `<script>` 标签加载这些文件

没有任何 import 或者 export, 但希望它是模块 `export {}`,这会把文件改成一个没有导出任何内容的模块

使用 type 前缀，表明被导入的是一个类型, 可以让一个非 TypeScript 编译器, 知道什么样的导入可以被安全移除
```ts
import { add } from './utils/math'
// 内置类型导入（Inline type imports）
import type { Person, PersonKeys } from './utils/type'

console.log(add(1, 2))
const person: Person = {
  name: 'zhangsan',
  age: 18
}
const keys: PersonKeys = 'name'
```

- TypeScript 模块化和 JavaScript 模块化的区别
	- JavaScript 有一个很长的处理模块化代码的历史
		- 比如: 以前会将一个模块封装到一个立即执行函数中
	- 随着时间流逝，社区和 JavaScript 规范已经使用为名为 ESModule 的格式
		- 这也就是我们所知的 import/export 语法
	- JavaScript 规范声明任何没有 export 的 JavaScript 文件都应该被认为是一个脚本，而非一个模块
	- 在 TypeScript 中最主要使用的模块化方案就是 ESModule
		- 在 TS 中如果想要让一个 JS 文件编程模块化可以添加一个 export{} 语法

## 5.2 TypeScript 命名空间
- TypeScript 有它自己的模块格式，名为 namespaces ，它在 ES 模块标准之前出现
	- 命名空间也称为内部模块
		- 目的: 将一个模块内部再进行作用域的划分，防止一些命名冲突的问题
- 命名空间没有废除, 但更建议使用 Es Modules, 这样才能与 JavaScript 的（发展）方向保持一致。
```ts
// utils/formate.ts
export namespace Time {
  export function formart(time: string) {
    return time.replace('T', ' ')
  }
}

export namespace Price {
  export function formart(price: number) {
    return `¥${price.toFixed(2)}`
  }
}

// index.ts
Time.formart('2020-01-01T12:00:00')
Price.formart(100)
```

## 5.3 内置声明文件的使用
> 类型的查找

![[Pasted image 20230318095125.png]]
HTMLImageElement 类型来自 TypeScript 对类型的管理和查找规则
- `.ts` ——编译-> `.js` 
-  `.d.ts` 文件是用来做类型的声明 (declare)
	- 称之为类型声明（Type Declaration）或者类型定义（Type Definition）文件
	- 它仅仅用来做类型检测，告知 typescript 我们有哪些类型
- typescript 查找类型声明在
	- 内置类型声明；
	- 外部定义类型声明；
	- 自定义类型声明；

> 内置类型声明
- 内置类型声明是 typescript 自带的、帮助我们内置了 JavaScript 运行时的一些标准化 API 的声明文件
	- 包括比如 Function、String、Math、Date 等内置类型；
	- 也包括运行环境中的 DOM API，比如 Window、Document 等
- TypeScript 使用模式命名这些声明文件 `lib.[something].d.ts`
	![[Pasted image 20230318095757.png]]


## 5.4 第三方库声明的文件
- 第三方库库通常有两种类型声明方式：
	- 方式一：在自己库中进行类型声明（编写.d.ts 文件），比如 axios
	- 方式二：通过社区的一个[公有库 DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped/) 存放类型声明文件
## 5.5 编写自定义声明文件
- 编写自定义声明文件的情况
	- 情况一：我们使用的第三方库是一个纯的 JavaScript 库，没有对应的声明文件；比如 lodash 
	- 情况二：我们给自己的代码中声明一些类型，方便在其他地方直接进行使用
	![[Pasted image 20230318110321.png]]

> declare 声明文件
- 在某些情况下，我们也可以声明文件：
	- 比如在开发 vue 的过程中，默认是不识别我们的 `.vue` 文件的，那么我们就需要对其进行文件的声明；
	- 比如在开发中我们使用了 jpg 这类图片文件，默认 typescript 也是不支持的，也需要对其进行声明；
	![[Pasted image 20230318110753.png]]

> declare 命名空间
- 在 `index.html` 中直接引入了 `jQuery` 
- 可以进行命名空间的声明
	![[Pasted image 20230318110907.png]]
- 在 `main.ts` 中就可以使用了 
	![[Pasted image 20230318110913.png]]
## 5.6 tsconfig 配置文件解析
> 认识 `tsconfig.json` 文件
- tsconfig. json 文件有两个作用：
	- 作用一（主要的作用）：
		- 让 TypeScript Compiler 在编译的时候，知道如何去编译 TypeScript 代码和进行类型检测；
			- ✓ 比如是否允许不明确的 this 选项，是否允许隐式的 any 类型；
			- ✓ 将 TypeScript 代码编译成什么版本的 JavaScript 代码；
	- 作用二：让编辑器（比如 VSCode）可以按照正确的方式识别 TypeScript 代码；
		- ✓ 对于哪些语法进行提示、类型错误检测等等；

> `tsconfig.json` 配置  
- `tsconfig.json` 在编译时如何被使用
	- 在调用 tsc 命令并且没有其它输入文件参数时，编译器将由当前目录开始向父级目录寻找包含 tsconfig 文件的目录。
	- 调用 tsc 命令并且没有其他输入文件参数，可以使用 --project （或者只是 -p）的命令行选项来指定包含了 tsconfig. json 的目录；
	- 当命令行中指定了输入文件参数， tsconfig. json 文件会被忽略；
	- webpack 中使用 ts-loader 进行打包时，也会自动读取 tsconfig 文件，根据配置编译 TypeScript 代码
- `tsconfig.json` 顶层选项
	![[Pasted image 20230318111921.png]]
- `tsconfig.json` 文件
	- `tsconfig.json` 是用于配置 TypeScript 编译时的[配置选项](https://www.typescriptlang.org/tsconfig)
	![[Pasted image 20230318112135.png]]
	![[Pasted image 20230318112200.png]]

# 六、axios+Ts 封装 
## 6.1 文件结构
```zsh
server
├── config  ## 存放配置文件api、timeout...
│   └── index.ts 
├── index.ts ## 统一请求入口
├── module ## 按项目分模块请求
│   ├── entire.ts
│   └── home.ts
└── request 
    ├── index.ts ## 封装axios请求
    └── type.ts ## 类型接口
```
## 6.2 封装 axios 请求类
`server/config/index.ts`
```ts
import type {
  AxiosRequestConfig,
  AxiosResponse,
  InternalAxiosRequestConfig
} from 'axios'

export interface interceptors<T = AxiosResponse> {
  requestSuccessFn?: (config: InternalAxiosRequestConfig) => InternalAxiosRequestConfig
  requestFailureFn?: (err: any) => any
  responseSuccessFn?: (res: T) => T
  responseFailureFn?: (err: any) => any
}

export interface RequestConfig<T = AxiosResponse> extends AxiosRequestConfig {
  interceptors?: interceptors<T>
}
```
`server/request/index.ts`
```ts
import axios from 'axios'
import type { AxiosInstance } from 'axios'
import type { RequestConfig } from './type'

class Request {
  instance: AxiosInstance
  
  constructor(config: RequestConfig) {
    // 创建并初始化instance实例
    this.instance = axios.create(config)
    // 类拦截器-请求拦截
    this.instance.interceptors.request.use(
      (config) => {
        // 添加loading/token
        return config
      },
      (err) => err
    )
	// 接口拦截器-针对特定的Request实例添加拦截器
	this.instance.interceptors.request.use(
      config.interceptors?.requestSuccessFn,
      config.interceptors?.requestFailureFn
	)
	this.instance.interceptors.response.use(
	  config.interceptors?.responseSuccessFn,
      config.interceptors?.responseFailureFn
	)
	// 类拦截器-响应拦截
	this.instance.interceptors.response.use(
	  (res) => {
	    // 移除loading加载动画
	    return res.data
	  },
	  (err) => err
	)
  }
  // 封装网络请求方法
  request<T = any>(config: RequestConfig<T>){
    // 实例拦截器-请求拦截
    if (config.interceptors?.requestSuccessFn) {
      config = config.interceptors.requestSuccessFn(config as any)
    }
    return new Promise<T>((resolve,reject) => {
      this.instance.request<any,T>(config)
        .then((res) => {
          // 实例拦截器-响应拦截
          if (config.interceptors?.responseSuccessFn) {
            res = config.interceptors.responseSuccessFn(res)
          }
          resolve(res)
        })
        .catch((res) => reject(err))
    })
  }
  get<T = any>(config: RequestConfig<T>) {
    return this.request<T>({ ...config, method: 'GET' })
  }
  post<T = any>(config: RequestConfig<T>) {
    return this.request({ ...config, method: 'POST' })
  }
}
```
## 6.3 按项目分模块请求
`server/module/config.ts`
```ts
export const BASE_URL = "http://codercba.com:8000"
export const AIR_URL = 'http://codercba.com:1888/airbnb/api'
export const TIME_OUT = 5000
```
`server/module/home.ts`
```ts
import { BASE_URL, TIME_OUT } from '../config'
import Request from '../request'

const request = new Request({ baseURL: BASE_URL, timeout: TIME_OUT })

export function getHomeList() {
  return request.get<Multidata>({ url: '/home/multidata' })
}
```
`server/module/entire.ts`
```ts
import { AIR_URL, TIME_OUT } from '../config'
import Request from '../request'

const request = new Request({
  baseURL: AIR_URL,
  timeout: TIME_OUT,
  interceptors: {
    requestSuccessFn(config) {
      console.log('爱彼赢-接口拦截-请求拦截-成功')
      return config
    },
    requestFailureFn(err) {
      console.log('爱彼赢-接口拦截-请求拦截-失败')
      return err
    },
    responseSuccessFn(res) {
      console.log('爱彼赢-接口拦截-响应拦截-成功')
      return res
    },
    responseFailureFn(err) {
      console.log('爱彼赢-接口拦截-响应拦截-失败')
      return err
    }
  }
})

export function getAirList() {
  return request.get({ url: '/entire/list', params: { offset: 0, size: 20 } })
}

export function getHightScore() {
  return request.get({
    url: '/home/highscore',
    interceptors: {
      requestSuccessFn: (config) => {
        console.log('highscore-实例拦截-请求拦截-成功')
        return config
      },
      responseSuccessFn: (res) => {
        console.log('highscore-实例拦截-响应拦截-成功')
        return res
      }
    }
  })
}
```
## 6.4 整合请求、统一请求入口
```ts
export * from './module/entire'
export * from './module/home'
```


[^1]: 标识符是用来给变量、类、方法以及包进行命名的. 而 var、let、const 是用来声明变量的关键字
[^2]: 内置工具: Typescript 提供了一些工具类型来辅助进行常见的类型转换，这些类型全局可用