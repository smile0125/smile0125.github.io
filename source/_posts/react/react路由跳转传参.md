---
title: react路由跳转传参
date: 2021/3/10 18:30
tags: react
categories: react
---

## params 传参

- 路由表配置：参数地址栏显示

```javascript
路由页面：
<Route path='/demo/:id' component={Demo}></Route> //配置 /:id

- 路由跳转并传递参数：
链接方式：
<Link to={'/demo/' + '2'}>XX</Link>
js方式：this.props.history.push('/demo/' + '2')
获取参数：this.props.match.params.id

```

## query 传参

- query 方法：参数地址栏不显示，刷新地址栏，参数丢失

```javascript
路由页面：
<Route path='/demo' component={Demo}></Route> //无需配置
路由跳转并传递参数：
链接方式：
<Link to={{path:'/demo',query:{name:'dahuang'}}}>XX</Link>
js方式：this.props.history.push({pathname:'/demo',query:{name:'dahuang'}})
获取参数： this.props.location.query.name
```

## state 传参( 同 query 差不多，只是属性不一样，而且 state 传的参数是加密的)

```javascript
state方法：参数地址栏不显示，刷新地址栏，参数不丢失
路由页面：
<Route path='/demo' component={Demo}></Route> //无需配置
路由跳转并传递参数：
链接方式：
<Link to={{path:'/demo',state:{name:'dahuang'}}}>XX</Link>
js方式：this.props.history.push({pathname:'/demo',state:{name:'dahuang'}})
获取参数： this.props.location.state.name
```
