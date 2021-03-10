---
title: webpack分析打包后的结构大小
date: 2021/3/10 23:29
tags: webpack
categories: webpack
---

```javascript
安装 npm install webpack-bundle-analyzer -D
配置webpack.config.js
 const BundleAnalyzerPlugin   = require('webpack-bundle-analyzer').BundleAnalyzerPlugin

plugins:[
    new BundleAnalyzerPlugin()
]
```

运行 npm run dev/start 启动项目

![](https://note.youdao.com/yws/public/resource/e7c91629849089c2c147a1470f593d5e/xmlnote/56406BB422864503948D292CCA694D70/6553)
