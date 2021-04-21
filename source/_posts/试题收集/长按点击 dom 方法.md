---
title: 长按点击 dom 方法
date: 2021/4/10 15:30
tags: dom click
categories: 试题收集
---

```js
- 长按点击 dom 方法
- @param{dom} 点击的 dom
- @param{callback} 点击后的回调

function clickLongEvent(dom, callback) {
  let time = 0; // 记录鼠标按下的时间
  let longTime = 500; // 定义长按的时间间隔
  let flag = false; // 是否超过长按所定义的时间

  // 按下鼠标时记录当前时间
  dom.addEventListener(
    "mousedown",
    function () {
      time = new Date().getTime();
    },
    false
  );

  // 松开鼠标时判断 flag 符合长按条件后执行回调函数
  dom.addEventListener(
    "mouseup",
    function () {
      flag = new Date().getTime() - time > longTime;
      if (flag) callback.call(this);
    },
    false
  );

  // 点击事件完成后恢复flag
  dom.addEventListener(
    "click",
    function () {
      flag = false;
    },
    false
  );
}
```
