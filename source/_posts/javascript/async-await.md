---
title: async/await
date: 2021/3/5 14:00
tags: javaScript
categories: javaScript
---

# async

**定义**：async 函数是使用 async 关键字声明的函数。 async 函数是 AsyncFunction 构造函数的实例， 并且其中允许使用 await 关键字。async 和 await 关键字让我们可以用一种更简洁的方式写出基于 Promise 的异步行为，而无需刻意地链式调用 promise。

```javascript
function resolveAfter2Seconds() {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve("resolved");
    }, 2000);
  });
}

async function asyncCall() {
  console.log("calling");
  const result = await resolveAfter2Seconds();
  console.log(result);
  // expected output: "resolved"
}
asyncCall();
```

## 语法

```javascript
async function name([param[, param[, ... param]]]) {
    // ...something
}
```

## 返回值

一个[Promise](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise)，这个 promise 要么会通过一个由 async 函数返回的值被解决，要么会通过一个从 async 函数中抛出的（或其中没有被捕获到的）异常被拒绝。

## 作用

- async 函数负责返回一个 Promise 对象
- 如果在 async 函数中 return 一个直接量，async 会把这个直接量通过 Promise.resolve() 封装成 Promise 对象;
- 如果 async 函数没有返回值,它会返回 Promise.resolve(undefined)。

## 描述

async 函数可能包含 0 个或者多个[await](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/await)表达式。`await表达式会暂停整个async函数的执行进程并出让其控制权，只有当其等待的基于promise的异步操作被兑现或被拒绝之后才会恢复进程。`promise 的解决值会被当作该 await 表达式的返回值。使用 async / await 关键字就可以在异步代码中使用普通的 try / catch 代码块。

```
await关键字只在async函数内有效。如果你在async函数体之外使用它，就会抛出语法错误 SyntaxError 。

async/await的目的为了简化使用基于promise的API时所需的语法。async/await的行为就好像搭配使用了生成器和promise。

async函数一定会返回一个promise对象。如果一个async函数的返回值看起来不是promise，那么它将会被隐式地包装在一个promise中。
```

例如，如下代码:

```
async function foo() {
   return 1
}

等价于:

function foo() {
   return Promise.resolve(1)
}
```

```
async函数的函数体可以被看作是由0个或者多个await表达式分割开来的。

从第一行代码直到（包括）第一个await表达式（如果有的话）都是同步运行的。

这样的话，一个不含await表达式的async函数是会同步运行的。

如果函数体内有一个await表达式，async函数就一定会异步执行。
```

例如：

```
async function foo() {
   await 1
}

等价于

function foo() {
   return Promise.resolve(1).then(() => undefined)
}
```

在 await 表达式之后的代码可以被认为是存在在链式调用的 then 回调中，多个 await 表达式都将加入链式调用的 then 回调中，返回值将作为最后一个 then 回调的返回值。

# await

**定义**：`await` 操作符用于等待一个 Promise 对象。它只能在异步函数 async function 中使用。

## 表达式

[返回值] = await 表达式;

一个 Promise 对象或者任何要等待的值。

## 返回值

返回 Promise 对象的处理结果。如果等待的不是 Promise 对象，则返回该值本身。

## 描述

- await 表达式会暂停当前 async function 的执行，等待 Promise 处理完成。若 Promise 正常处理(fulfilled)，其回调的 resolve 函数参数作为 await 表达式的值，继续执行 async function。

- 若 Promise 处理异常(rejected)，await 表达式会把 Promise 的异常原因抛出。

- 另外，如果 await 操作符后的表达式的值不是一个 Promise，则返回该值本身。

## 🍉

如果一个 Promise 被传递给一个 await 操作符，await 将等待 Promise 正常处理完成并返回其处理结果。

```
function resolveAfter2Seconds(x) {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve(x);
    }, 2000);
  });
}

async function f1() {
  var x = await resolveAfter2Seconds(10);
  console.log(x); // 10
}
f1();
```

如果该值不是一个 Promise，await 会把该值转换为已正常处理的 Promise，然后等待其处理结果。

```
async function f2() {
  var y = await 20;
  console.log(y); // 20
}
f2();
```

如果 Promise 处理异常，则异常值被抛出。

```
async function f3() {
  try {
    var z = await Promise.reject(30);
  } catch (e) {
    console.log(e); // 30
  }
}
f3();
```
