---
title: webpack 性能优化
tags: webpack
categories: webpack
---

# 项目初始化

## 1.1 安装

```
cnpm i webpack webpack-cli html-webpack-plugin webpack-dev-server cross-env -D
```

## 1.2 webpack.config.js

```
const path = require("path");
module.exports = {
mode: "development",
devtool: 'source-map',
context: process.cwd(),
entry: {
main: "./src/index.js",
},
output: {
path: path.resolve(\_\_dirname, "dist"),
filename: "main.js"
} };
```

## 1.3 package.json

```
"scripts": {
  "build": "webpack",
  "start": "webpack serve"
},
```

# 2.数据分析

## 2.1 日志美化

friendly-errors-webpack-plugin 可以识别某些类别的 webpack 错误，并清理，聚合和优先级，以 提供更好的开发人员体验

### 2.1.1 安装

```
cnpm i friendly-errors-webpack-plugin node-notifier -D
```

### 2.1.2 webpack.config.js

```
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");
/*
const FriendlyErrorsWebpackPlugin = require('friendly-errors-webpack-plugin');
const notifier = require('node-notifier');
const ICON = path.join(__dirname,
*/
'icon.jpg');
module.exports = {
mode: "development"
,
devtool: 'source-map'
,
context: process.cwd(),
entry: {
main: "
./src/index.js"
,
},
output: {
path: path.resolve(__dirname,
"dist"),
filename: "main.js"
},
plugins:[
new HtmlWebpackPlugin(),
/*
new FriendlyErrorsWebpackPlugin({
onErrors: (severity, errors) => {
const error = errors[0];
notifier.notify({
title: "Webpack编译失败"
message: severity + ': ' + error.name,
subtitle: error.file || ''
icon: ICON
});
}
})
*/
]
};
```

## 2.2 速度分析

speed-measure-webpack5-plugin 可以分析打包速度

### 2.2.1 安装

```
cnpm i speed-measure-webpack5-plugin -D
```

### 2.2.2 webpack.config.js

```
+const SpeedMeasureWebpackPlugin = require('speed-measure-webpack5-plugin');
+const smw = new SpeedMeasureWebpackPlugin();
+module.exports = smw.wrap({
  mode: "development",
  devtool: 'source-map',
  ...
+});
```

## 2.3 文件体积监控

webpack-bundle-analyzer 是一个 webpack 的插件，需要配合 webpack 和 webpack-cli 一起使用。 这个插件的功能是生成代码分析报告，帮助提升代码质量和网站性能 它可以直观分析打包出的文件包含哪些，大小占比如何，模块包含关系，依赖项，文件是否重复， 压缩后大小如何，针对这些，我们可以进行文件分割等操作。

### 2.3.1 安装

```
cnpm i webpack-bundle-analyzer -D
```

### 2.3.2 编译启动

#### 2.3.2.1 webpack.config.js

```
+const {BundleAnalyzerPlugin} = require('webpack-bundle-analyzer')
module.exports={
  plugins: [
+
] }
```

#### 2.3.2.2 package.json

```
  "scripts": {
    "build": "webpack",
    "start": "webpack serve",
+    "dev":"webpack  --progress"
},
```

### 2.3.3 单独启动

#### 2.3.3.1 webpack.config.js

```
const {BundleAnalyzerPlugin} = require('webpack-bundle-analyzer')
module.exports={
+ +
plugins: [
  new BundleAnalyzerPlugin({
analyzerMode: 'disabled', // 不启动展示打包报告的http服务器
generateStatsFile: true, // 是否生成stats.json文件 }),
] }
```

#### 2.3.3.2 package.json

```
"scripts": {
  "build": "webpack",
  "start": "webpack serve",
  "dev":"webpack  --progress",
   +"analyzer": "webpack-bundle-analyzer --port 8888 ./dist/stats.json"
}
```

# 3.编译时间优化

- 减少要处理的文件
- 缩小查找的范围

## 3.1 缩小查找范围

### 3.1.1 extensions

- 指定 extensions 之后可以不用在 require 或是 import 的时候加文件扩展名
- 查找的时候会依次尝试添加扩展名进行匹配

```
resolve: {
+extensions: [".js",".jsx",".json"]},
module:{
```

### 3.1.2 alias

- 配置别名可以加快 webpack 查找模块的速度
- 每当引入 bootstrap 模块的时候，它会直接引入 bootstrap ,而不需要从 node_modules 文件夹中 按模块的查找规则查找

```
cnpm i bootstrap css-loader style-loader -S
```

```
+const bootstrap =
path.resolve(__dirname,'node_modules/bootstrap/dist/css/bootstrap.css');
module.exports = smw.wrap({
  mode: "development",
  devtool: 'source-map',
  context: process.cwd(),
  entry: {
    main: "./src/index.js",
  },
  output: {
    path: path.resolve(__dirname, "dist"),
    filename: "main.js"
}, resolve: {
    extensions: [".js",".jsx",".json"],
+    alias:{bootstrap}
},
+ module:{
+ rules:[ +{
+              test: /\.css$/,
+              use: ['style-loader', 'css-loader']
+} +]
} });
```

### 3.1.3 modules

- 对于直接声明依赖名的模块 webpack 会使用类似 Node.js 一样进行路径搜索，搜索 node_modules 目录
- 如果可以确定项目内所有的第三方依赖模块都是在项目根目录下的 node_modules 中的话可以直接 指定
- 默认配置

```
resolve: {
    extensions: [".js",".jsx",".json"],
    alias:{bootstrap},
+   modules: ['node_modules']
},
```

- 直接指定

```
resolve: {
+  modules: ['C:/node_modules','node_modules'],
}
```

### 3.1.4 mainFields

- 默认情况下 package.json 文件则按照文件中 main 字段的文件名来查找文件

```
resolve: {
// 配置 target === "web" 或者 target === "webworker" 时 mainFields 默认值是:
+  mainFields: ['browser', 'module', 'main'],
// target 的值为其他时，mainFields 默认值为: + mainFields: ["module", "main"],
}
```

### 3.1.5 mainFiles

- 当目录下没有 package.json 文件时，我们说会默认使用目录下的 index.js 这个文件

```
resolve: {
+  mainFiles: ['index']
},
```

### 3.1.6 oneOf

- 每个文件对于 rules 中的所有规则都会遍历一遍，如果使用 oneOf 就可以解决该问题，只要能匹配 一个即可退出
- 在 oneOf 中不能两个配置处理同一种类型文件
  webpack.config.js

```
rules: [
    +{
        + oneOf:[
        {
          test: /\.js$/,
          include: path.resolve(__dirname, "src"),
          exclude: /node_modules/,
          use: [
            {
              loader: 'thread-loader',
              options: { workers: 3 }
            }, {
              loader:'babel-loader',
              options: {
                cacheDirectory: true
              }
}] },
        {
          test: /\.css$/,
          use: ['cache-loader','logger-loader', 'style-loader', 'css-loader']
}
+]
} ]
```

### 3.1.7 external

- 如果我们想引用一个库，但是又不想让 webpack 打包，并且又不影响我们在程序中以 CMD、AMD 或者 window/global 全局等方式进行使用，那就可以通过配置 externals

#### 3.1.7.1 安装

```
cnpm i jquery html-webpack-externals-plugin -D
```

#### 3.1.7.2 使用 jquery

- src/index.js

```
const jQuery = require("jquery");
import jQuery from 'jquery';
```

#### 3.1.7.3 index.html

src/index.html

```
<script src="https://cdn.bootcss.com/jquery/3.4.1/jquery.js"></script>
```

#### 3.1.7.4 webpack.config.js

webpack.config.js

```
+ externals: {
+   jquery: 'jQuery',
+ },
module: {
```

### 3.1.8 resolveLoader

resolve.resolveLoader 用于配置解析 loader 时的 resolve 配置,默认的配置

#### 3.1.8.1 logger-loader.js

loaders\logger-loader.js

```
function loader(source){
console.log('logger-loader');
return source;
}
module.exports = loader;
```

#### 3.1.8.2 webpack.config.js

webpack.config.js

```
module.exports = {
resolve: {
extensions: [".js",".jsx",".json"],
alias:{bootstrap},
modules: ['node_modules'],
},
+ resolveLoader:{
+ modules: [path.resolve(__dirname,"loaders"),
'node_modules'],
+ },
module:{
rules:[
{
test:/\.css$/,
use:[
+'logger-loader', 'style-loader', 'css-loader'
]
}
]
},
};
```

## 3.2 noParse

- module.noParse 字段，可以用于配置哪些模块文件的内容不需要进行解析
- 不需要解析依赖（即无依赖） 的第三方大型类库等，可以通过这个字段来配置，以提高整体的构
  建速度
- 使用 noParse 进行忽略的模块文件中不能使用 import 、 require 等语法
