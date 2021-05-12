---
title: react ref(forwardRef) 获取子组件中的元素
date: 2021/5/11 20:00
tags: react
categories: react
---

```js
# 父组件
import { useEffect, useRef } from 'react';
import Child from './child';

const Detail = () => {
  const parentUseRef = useRef();
  useEffect(() => {
    console.log(parentUseRef.current);
    // <div>child dom</div>
  }, []);
  return (
    <div>
      <Child myRef={parentUseRef} />
    </div>
  );
};

export default Detail;
```

```js
# 子组件
import React from 'react';
const Child = (props) => {
  return <div ref={props.myRef}>child dom</div>;
};

export default Child;
```

## forwardRef

```js
# 父组件
import { Link } from 'umi';
import { useEffect, useRef, forwardRef, createRef } from 'react';
import Child from './child';

// const NewChild = forwardRef((props, ref) => <div {...props} ref={ref}>child</div>)
const NewChild = forwardRef((props, ref) => <Child {...props} myRef={ref}/>)

const Detail = () => {
  const parentUseRef = useRef();
  useEffect(() => {
    console.log(parentUseRef.current);
    // <div>child dom</div>
  }, []);
  return (
    <div>
      <h3>detail</h3>
      <Link to="/">首页</Link>
      <NewChild ref={parentUseRef} />
    </div>
  );
};
export default Detail;
```

```js
# 子组件
import React from 'react';
const Child = ({myRef}) => {
  return <div ref={myRef}>child dom</div>;
};

export default Child;
```
