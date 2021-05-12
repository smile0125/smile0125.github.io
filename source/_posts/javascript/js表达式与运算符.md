---
title: js表达式与运算符
date: 2021/5/10 20:00
tags: javaScript
categories: javaScript
---

# 加法赋值 (+=)

> 加法赋值操作符 (+=) 将右操作数的值添加到变量，并将结果分配给该变量。两个操作数的类型确定加法赋值运算符的行为。加法或串联是可能的。

```js
let a = 2;
let b = "hello";

console.log((a += 3));
// expected output: 5

console.log((b += " world"));
// expected output: "hello world"
```

# 求幂分配（\*\* =）

> 乘幂赋值运算符（\*\*=）将变量的值提高到右操作数的幂。

```js
let a = 3;

console.log((a **= 2));
// expected output: 9

console.log((a **= 0));
// expected output: 1

console.log((a **= "hello"));
// expected output: NaN
```

# 求幂 (\*\*)

> 求幂运算符（\*\*）返回将第一个操作数加到第二个操作数的幂的结果。它等效于 Math.pow，不同之处在于它也接受 BigInts 作为操作数。

```js
console.log(3 ** 4);
// expected output: 81

console.log(10 ** -2);
// expected output: 0.01

console.log(2 ** (3 ** 2));
// expected output: 512

console.log((2 ** 3) ** 2);
// expected output: 64
```

# 逻辑空赋值 (??=)

> 逻辑空赋值运算符 (x ??= y) 仅在 x 是 nullish (null 或 undefined) 时对其赋值。

```js
const a = { duration: 50 };

a.duration ??= 10;
console.log(a.duration);
// expected output: 50

a.speed ??= 25;
console.log(a.speed);
// expected output: 25
```

# 逻辑或赋值 (||=)

> 逻辑空赋值运算符 (x ||= y) 仅在 x 是 false 时对其赋值。

```js
const a = { duration: 50 };

a.duration ||= 10;
console.log(a.duration);
// expected output: 50

a.speed ||= 25;
console.log(a.speed);
// expected output: 25
```

# 逻辑与赋值 (&&=)

> 逻辑空赋值运算符 (x &&= y) 仅在 x 是 ture 时对其赋值。

```js
let a = 1;
let b = 0;

a &&= 2;
console.log(a);
// expected output: 2

b &&= 2;
console.log(b);
// expected output: 0
```

# 空值合并运算符

> 空值合并操作符（??）是一个逻辑操作符，当左侧的操作数为 null 或者 undefined 时，返回其右侧操作数，否则返回左侧操作数。

> 与逻辑或操作符（||）不同，逻辑或操作符会在左侧操作数为假值时返回右侧操作数。也就是说，如果使用 || 来为某些变量设置默认值，可能会遇到意料之外的行为。比如为假值（例如，'' 或 0）时。

```js
const foo = null ?? "default string";
console.log(foo);
// expected output: "default string"

const baz = 0 ?? 42;
console.log(baz);
// expected output: 0
```

# 可选链操作符

> 可选链操作符( ?. )允许读取位于连接对象链深处的属性的值，而不必明确验证链中的每个引用是否有效。?. 操作符的功能类似于 . 链式操作符，不同之处在于，在引用为空(nullish ) (null 或者 undefined) 的情况下不会引起错误，该表达式短路返回值是 undefined。与函数调用一起使用时，如果给定的函数不存在，则返回 undefined。

```js
const adventurer = {
  name: "Alice",
  cat: {
    name: "Dinah",
  },
};

const dogName = adventurer.dog?.name;
console.log(dogName);
// expected output: undefined

console.log(adventurer.someNonExistentMethod?.());
// expected output: undefined
```

# 展开语法(...)

> 展开语法(Spread syntax), 可以在函数调用/数组构造时, 将数组表达式或者 string 在语法层面展开；还可以在构造字面量对象时, 将对象表达式按 key-value 的方式展开。(译者注: 字面量一般指 [1, 2, 3] 或者 {name: "mdn"} 这种简洁的构造方式)

```js
function sum(x, y, z) {
  return x + y + z;
}

const numbers = [1, 2, 3];

console.log(sum(...numbers));
// expected output: 6

console.log(sum.apply(null, numbers));
// expected output: 6
```
