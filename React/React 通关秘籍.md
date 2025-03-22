## 一、 一网打尽组件常用 Hook
### 1. useState
用于在函数组件中添加状态,***状态是变化的数据***
```tsx
import {useState} from "react";

function App() {
  // 这个函数只能写一些同步的计算逻辑，不支持异步
  const [num, setNum] = useState(() => {
    const num1 = 1 + 2;
    const num2 = 2 + 3;
    return num1 + num2
  })

  return (
      <button onClick={() => setNum((prevNum) => prevNum + 1)}>{num}</button>
  );
}
```
### 2. useEffect
用于在组件渲染完成后执行副作用操作（如数据获取、订阅、手动操作 DOM）
```tsx
import {useEffect, useState} from "react";

function App() {
  const [num, setNum] = useState(0)

  useEffect(() => {
    console.log("effect...")
    const timer = setInterval(() => {
      console.log(num)
    }, 1000)

    return () => {
      console.log('clean...')
      clearInterval(timer)
    }
  }, [num]);

  return (
      <div className="flex justify-center m-5">
        <button className="w-10 border" onClick={() => setNum(prevState => prevState + 1)}>+ 1</button>
        <input className="w-16 border text-center" type="text" value={num} readOnly={true}/>
        <button className="w-10 border" onClick={() => setNum(prevState => prevState - 1)}>- 1</button>
      </div>
  )
}

export default App;
```
![[Pasted image 20240424172011.png | 800]]
### 3. useLayoutEffect
![[Pasted image 20240424172257.png | 800]]
绝大多数情况下，用 useEffect，它能避免因为 effect 逻辑执行时间长导致页面卡顿（掉帧）。但如果你遇到闪动的问题比较严重，那可以用 useLayoutEffect，但要注意 effect 逻辑不要执行时间太长。
### 4. useReducer
用于在函数组件中执行复杂的状态逻辑, **当修改 state 的逻辑比较复杂，用 useReducer**
```tsx
import {Reducer, useReducer} from "react";

interface Data {
  result: number
}

interface Action {
  type: 'add' | 'minus',
  num: number
}

function reducer(state: Data, action: Action) {
  switch (action.type) {
    case 'add':
      return {
        result: state.result + action.num
      }
    case 'minus':
      return {
        result: state.result - action.num
      }
    default: return state;
  }
}

function App() {
  const [res, dispatch] = useReducer<Reducer<Data, Action>>(reducer, {result: 0})
  return (
      <div className="flex justify-center items-center text-center mt-16">
        <div className="border px-6 py-3 cursor-pointer no-select"
             onClick={() => dispatch({type: 'add', num: 2})}>加</div>
        <div className="w-10 ml-3 mr-3">{res.result}</div>
        <div className="border px-6 py-3 cursor-pointer no-select"
             onClick={() => dispatch({type: 'minus', num: 1})}>减</div>
      </div>
  )
}


export default App;
```
useReducer 的类型参数传入 `Reducer<数据的类型，action 的类型>` 然后第一个参数是 reducer，第二个参数是初始数据。
***在 react 里，只要涉及到 state 的修改，就必须返回新的对象，不管是 useState 还是 useReducer***