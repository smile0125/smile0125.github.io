---
title: react fiber
date: 2021-03-02 21:55:10
tags: react
categories: react
---

# fiber

- fiber：就是一个数据结构，它有很多属性。
- 虚拟 dom 是对真实 dom 的一种简化。
- 由于一些真实 dom 都做不到的事情，那因此虚拟 dom 更做不到。
- 所以就有了 fiber，有很多属性希望借助于 fiber 上的这些属性来做一些比较厉害的事情。

# fiber 架构

- 为了弥补一下不足，设计了一些新的算法，而为了能让这些算法跑起来，所以出现了 fiber 这种数据结构。
- fiber 架构 = fiber 数据结构 + 新的算法

# react 应用

- react 应用从始至终管理着最基本的三样东西

  - Root（整个应用的根，一个对象，它不是 fiber）`有一个属性指向 current 树，同样也有个属性指向 workInProgress 树`
  - current 树（树上的每一个节点都是一个 fiber，并且每个 fiber 节点都对应着一个 jsx 节点）`保存的是上一次的状态。`
  - workInProgress 树（树上的每一个节点都是一个 fiber，并且每个 fiber 节点都对应着一个 jsx 节点）`保存的是新的状态`

  ## 渲染时的树

- react 一开始创建 Root 的时候就会同时创建一个 uninitialFiber 的东西（为初始化的 fiber）
  让 react 的 current 树指向了 uninitialFiber
  之后再去创建一个本次所需要用到的 workInProgress 树。
  !()[]
  ## react 只要分两个阶段
  - render 阶段：指创建 fiber 的过程
    1，为每个节点创建新的 fiber（workInProgress）生成一颗有新状态的 workInProgress 树。
    2，初次渲染的时候（或新创建了某个节点的时候）会将这个 fiber 创建真实的 dom 实例，并且对当前节点的子节点进行插入。
    3，如果不是初次渲染的话，就会对比新旧 fiber 树的状态，将产生了更新的 fiber 节点，最终通过链表的形式挂载到 RootFiber 上。
  - commit 阶段：指真正操作页面的阶段
    1，执行生命周期。
    2，会从 RootFiber 上获取到那条链表，根据链表上的标识来操作页面。

```javascript
// React.render()返回的结构其实是一个对象
const REACT_ELEMENT_TYPE = Symbol.for("react.element");
function ReactElement(type, key, props) {
  let element = {
    $$typeof: REACT_ELEMENT_TYPE,
    type, // 'div' | 'p' | component组件
    props,
  };
  return element;
}
```

# 手动实现简易版 ReactDOM

```javascript
// 定义一个变量方便后面使用（react源码中没有这个）
let isFirstRender = false;
const ReactDOM = {
  render(reactElement, container, callback) {},
};
export default ReactDOM;
```
