---
title: CSS特指度
date: 2021/4/11 14:33
tags: CSS特指度
categories: css
---

CSS 优先级和特指度
1、ID 选择符 > 类选择符 > 元素选择符，特指度高的优先级高
2、行内样式 > 内嵌样式 > 链接样式
3、设定的样式 > 继承的样式
特指度表示一个 css 选择器表达式的重要程度（也可看成是权重）
特指度可以用一个公式 I-C-E 来计算，其中
I 是 ID
C 是 Class
E 是 Element
在 css 选择器，遇到一个 id 就往特指度数值中加 100，遇到一个 class 就往特指度数值中加 10，遇到一个 element 就往特指度数值中加 1
数值越大 优先级越高，设置的样式高于特指度的继承（在做练习的时候犯了没有精准定位的错误，导致优先级不管用，以后切记！@\_@）

```css
//样式
 <style type="text/css">

.box {
       height: 800px;
       width: 800px;
        padding:20px;
        border: 17px ridge #28d94a;
        margin:50px 30 50 250 ;
            }
#div1{
       padding:20px;
        border: 17px ridge #28d94a;
}
span{
         height: 100%;   //设置样式
         width: 100%;
            }
 </style>
```

CSS 选择器表达式 特指度计算结果
p 1
p.large 11
P#large 101
div p#large 102
div p#large ul.list 113
div p#large ul.list li 114
