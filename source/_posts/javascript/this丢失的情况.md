---
title: this丢失的情况
date: 2021/4/12 10:38
tags: this
categories: javaScript
---

1.函数名是别名

```js
function foo() {
  console.log(this.a);
}
var obj = {
  a: 2,
  foo: foo,
};
var bar = obj.foo;
var a = 1;
bar(); //  输出1  this指向了window
```

2.函数作为参数传递

```js
function foo() {
  console.log(this.a);
}
function Foo(fn) {
  fn();
}
var obj = {
  a: 2,
  foo: foo,
};
var a = 1;
Foo(obj.foo); //输出1
```

obj.foo 作为参数传递给 Foo，然后在 Foo 里相当于独立调用 this 指向了 window

3.出现在内置函数中

```js
function foo() {
  console.log(this.a);
}

var obj = {
  a: 2,
  foo: foo,
};
var a = 1;
setTimeout(obj.foo, 200); //输出1
```

4.函数的赋值

```js
function foo() {
  console.log(this.a);
}

var obj = {
  a: 2,
  foo: foo,
};
var obj2 = {
  a: 3,
};
var a = 1;

(obj2.foo = obj.foo)(); //输出1
```
