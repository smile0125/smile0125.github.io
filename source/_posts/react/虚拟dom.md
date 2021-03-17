---
title: 虚拟dom
date: 2021-03-17 23:08:10
tags: 虚拟dom
categories: react
---

> React 是把真实的 DOM 树转换为 JS 对象树，也就是 Virtual DOM。每次数据更新后，重新计算 VM，并和上一次生成的 VM 树进行对比，对发生变化的部分进行批量更新。除了性能之外，VM 的实现最大的好处在于和其他平台的集成。

- 真实 dom

```javascript
<button class="myButton">
  <span>this is button</span>
</button>
```

- Virtual DOM

```javascript
{
  type: 'button',
  props: {
  	className: 'myButton',
    children: [{
      type: 'span',
      props: {
        type: 'text'
        children: 'this is button'
      }
    }]
  }
}


```

因为 VM 并不是真实的操作 DOM，通过 diff 算法可以避免一些不变要的 DOM 操作，从而提高了性能。

同时也不是一定会提高性能，因为 VM 只是通过 diff 算法避免了一些不需要变更的 DOM 操作，最终还是要操作 DOM 的，并且 diff 的过程也是有成本的。
对于某些场景，比如都是需要变更 DOM 的操作，因为 VM 还会有额外的 diff 算法的成本在里面，所以 VM 的方式并不会提高性能，甚至比原生 DOM 要慢。
