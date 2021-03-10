---
title: useContext
date: 2021/3/10 18:00
tags: react
categories: react
---

# useContext

## 创建 context 公共文件

context.Provider 提供了一个 Context 对象，这个对象是可以被子组件共享的。

```javascript
import { createContext } from "react";
const myContext = createContext(null);
export default myContext;
```

## 创建父组件

```javascript
import React, { useState } from "react";
import Counter from "./Counter";
import myContext from "./createContext";

function App() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <h4>我是父组件</h4>
      <p>点击了 {count} 次!</p>
      <button
        onClick={() => {
          setCount(count + 1);
        }}
      >
        点我
      </button>

      {/* 关键代码 */}
      {/* 提供器 */}
      <myContext.Provider value={count}>
        <Counter />
      </myContext.Provider>
    </div>
  );
}
export default App;
```

## 创建子组件

```javascript
// useContext()钩子函数用来引入Context对象，从中获取count 属性
import React, { useContext } from "react";
import myContext from "./createContext";

// 关键代码
function Counter() {
  const count = useContext(myContext); // 得到父组件传的值
  return (
    <div>
      <h4>我是子组件</h4>
      <p>这是父组件传过来的值：{count}</p>
    </div>
  );
}
export default Counter;
```
