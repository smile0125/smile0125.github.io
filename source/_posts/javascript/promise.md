---
title: Promis
tags: javaScript
categories: javaScript
---

# 手写 Promis

```
// 常量定义3promise的三个状态
const PENDING = "pending";
const FULFILLED = "fulfilled";
const REJECTED = "rejected";

// 处理then链式调用
function resolvePromise(promise2, x, resolve, reject) {
  if (promise2 === x) {
    return reject(
      new TypeError("Chaining cycle detected for promise #<MyPromise>")
    );
  }

  let called = false;
  if ((typeof x === "object" && typeof x !== null) || typeof x === "function") {
    try {
      const then = x.then;
      if (typeof then === "function") {
        then.call(
          x,
          (y) => {
            if (called) return;
            called = true;
            resolvePromise(x, y, resolve, reject);
          },
          (r) => {
            if (called) return;
            called = true;
            reject(r);
          }
        );
      } else {
        resolve(x);
      }
    } catch (error) {
      if (called) return;
      called = true;
      reject(error);
    }
  } else {
    resolve(x);
  }
}

class MyPromise {
  constructor(executor) {
    this.status = PENDING;
    this.value = undefined;
    this.reason = undefined;
    // 成功的回调
    this.onFulfiledCallbacks = [];
    // 失败的回调
    this.onRejectedCallbacks = [];

    const resolve = (value) => {
      if (this.status === PENDING) {
        this.status = FULFILLED;
        this.value = value;
        this.onFulfiledCallbacks.map((fn) => fn());
      }
    };

    const reject = (reason) => {
      if (this.status === PENDING) {
        this.status = REJECTED;
        this.reason = reason;
        this.onRejectedCallbacks.map((fn) => fn());
      }
    };

    try {
      executor(resolve, reject);
    } catch (error) {
      reject(error);
    }
  }

  /**
  * then方法指定了成功的和失败的回调函数
  * 返回一个新的promise对象，它实现了链式调用
  * 返回的promise的结果由onResolved和onRejected决定
  */
  then(onFulfiled, onRejected) {
    onFulfiled =
      typeof onFulfiled === "function" ? onFulfiled : (value) => value;
    onRejected =
      typeof onRejected === "function"
        ? onRejected
        : (reason) => {
            throw reason;
          };
    const promise2 = new MyPromise((resolve, reject) => {
      if (this.status === FULFILLED) {
        setTimeout(() => {
          try {
            let x = onFulfiled(this.value);
            resolvePromise(promise2, x, resolve, reject);
          } catch (error) {
            reject(error);
          }
        }, 0);
      }
      if (this.status === REJECTED) {
        setTimeout(() => {
          try {
            let x = onRejected(this.reason);
            resolvePromise(promise2, x, resolve, reject);
          } catch (error) {
            reject(error);
          }
        }, 0);
      }

      if (this.status === PENDING) {
        this.onFulfiledCallbacks.push(() => {
          try {
            let x = onFulfiled(this.value);
            resolvePromise(promise2, x, resolve, reject);
          } catch (error) {
            reject(error);
          }
        });
        this.onRejectedCallbacks.push(() => {
          try {
            let x = onRejected(this.reason);
            resolvePromise(promise2, x, resolve, reject);
          } catch (error) {
            reject(error);
          }
        });
      }
    });
    return promise2;
  }
  catch(errorCallback) {
    this.then(null, errorCallback);
  }
}

module.exports = MyPromise;
```

## Promise resove 方法

```
/**
  * Promise函数对象的resove方法
  * 返回一个指定结果成功的promise
  */
 // 这个简单 让返回的promise成功就行
 Promise.resolve=function(value){
    return new Promise((resolve,reject)=>{
        if(value instanceof Promise){
            value.then(resolve,reject)
        }else {
            resolve(value)
        }
     })
 }
```

## Promise reject 方法

```
/**
  *  Promise函数对象的reject方法
  * 返回一个指定reason失败的promise
  */
  // 这个简单 让返回的promise失败就行
 Promise.reject=function(reason){
     return new Promise((resove,reject)=>{
        reject(reason)
     })
}
```

## Promise.all

- 所有成功才成功，有一个失败就失败
- 返回一个的 Promise，这个 promise 的结果由传过来的数组决定，一个失败就是失败
  // 循环传入的数组，把成功的 promise 的返回的值放到 values 中
  // 只有当 values 和 promises 相同时，说明全部成功，这时候返回一个成功的数组，有一个失败就失败
  ```
  Promise.all = function (promises) {
  return new Promise((resolve, reject) => {
  let values = []
  promises.map(item => {
  if (item instanceof Promise) {
  item.then(
  (res) => {
  values.push(res)
  }
  , reject)
  } else {
  // 为了正确的放入 values，所以也让其异步
  setTimeout(() => {
  values.push(item)
  })
  }
  })
  // 这里用 setTiemeout 是因上面的 then 方法是异步的，让下面的代码也异步，才能拿到最终的 values 数组
  setTimeout(() => {
  if (values.length === promises.length) {
  resolve(values)
  }
  })
  })
  }
  ```

## Promise.race

```
/**
 * 第一个成功就成功，如果不成功就失败(就是最先拿到谁的值，就成功)
 * 返回一个Promsie
 */
// 只要发现一个promsie成功了，就让返回的promsie成功
Promise.race=function(promises){
    return new Promise((resolve,reject)=>{
        promises.map(item=>{
            if(item instanceof Promise) {
                item.then(
                    resolve
                    ,reject)
            }else {
                resolve(item)
            }
        })
    })
}
```
