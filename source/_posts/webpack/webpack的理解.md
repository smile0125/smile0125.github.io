---
title: webpack的理解
date: 2021/3/10 10:30
tags: webpack
categories: webpack
---

# webpack 的理解

- webpack 是一个打包模块化 js 的工具，在 webpack 里一切文件皆模块。
- 通过 loader 转换文件。
- 通过 plugin 注入钩子。
- 最后输出由多个模块组合成的文件。
- webpack 专注构建模块化项目，并且可以把 webpack 看作是模块的打包机器。
- 他做的事情就是分析你的项目结构，找到 js 模块以及其他浏览器不能直接运行的拓展语言，
  例如 scss，ts 等等，并将他们打包处理成浏览器可以识别的代码。

### 什么是 bundle

- bundle 是由 webpack 打包出来的文件

### 什么是 trunk

- trunk 是代码块，一个 trunk 由多个模块组合而成，用于代码的合并和分割

### 什么是 module

- module 是开发中的单个模块，在 webpack 里一切皆模块，一个模块对应一个文件，webpack 会从配置的 entry 中递归找出所有依赖的模块

### 什么是 loader

- loader 是用来告诉 webpack 如何处理某一个类型的文件，并且引入到打包处的文件当中

### 什么是 plugin

- plugin 是用来自定义 webpack 打包过程的方式，一个插件是含有 apply 方法的一个对象，通过这个方法可以参与到整个 webpack 的各个流程

### 有哪些常用的 loader，用来解决什么问题

- 1， file-loader：把文件输出到一个文件夹中，在代码中通过相对 url 去引用输出的文件。
- 2，url-loader：和 file-load 类似，但是能在文件很小的情况下，以 base64 的方式把文件注入到代码中。
- 3，source-map-loader：加载额外的 source map 文件，以方便断点调试。
- 4，image-loader：加载并且压缩图片。
- 5，bable-loader：把 es6 转换成 es5.
- 6，css-loader：加载 css 文件，支持模块化，压缩，文件导入等特性。
- 7，style-loader：把 css 文件注入到 js 当中，通过 dom 操作去加载 css。
- 8，eslint-loader：通过 eslint 检查 js 代码。
