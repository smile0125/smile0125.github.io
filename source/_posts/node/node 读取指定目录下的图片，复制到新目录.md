---
title: nodejs 读取指定目录下的图片，复制到新目录
date: 2021/5/11 20:00
tags: copyFile
categories: node
---

> node 读取指定目录下的图片，复制到新目录

```js
const path = require("path");
const fs = require("fs");
let reg = /(\.jpg|\.jpeg|\.png|\.gif|\.webp)/;
const originPath = path.join(__dirname, "imgs/");
let newPath = path.join(__dirname, "dist");

fs.readdir(originPath, (err, res) => {
  if (!err && res) {
    let imgList = res.filter((item) => reg.test(item));
    imgList.forEach((item) => {
      // 方式一
      /*
      var readStream = fs.createReadStream(`${originPath}/${item}`); // 被复制文件
      // 创建一个写入流
      var writeStream = fs.createWriteStream(`${newPath}/${item}`); // 复制到的目标位置及文件
      // 读取流的内容通过管道流写入到输出流
      readStream.pipe(writeStream)
      */

      // 方式二
      /*
      fs.readFile(`${originPath}/${item}`, (err, res) => {
        fs.writeFile(`${newPath}/${item}`, res, (wErr, wRes) => {
          if (wErr) {
            console.log(wErr);
          }
        });
      });*/

      // 方式三
      /*
      fs.readFile(`${originPath}/${item}`, (err, res) => {
        let base64Img = res.toString("base64"); // 将图片转换成base64
        let decodeImg = Buffer.from(base64Img, "base64"); // 以base64格式创建缓冲区
        // 将缓冲区数据写入到指定目录
        fs.writeFile(`${newPath}/${item}`, decodeImg, (wErr, wRes) => {
          if (wErr) {
            console.log(wErr);
          }
        });
      });*/

      // 方式四
      fs.copyFile(`${originPath}/${item}`, `${newPath}/${item}`, (err, res) => {
        if (err) {
          console.log(err);
        }
      });
    });
  }
});
```

> 复制多张

```js
const path = require("path");
const fs = require("fs");

const originPath = path.join(__dirname, "imgs");
const newPath = path.join(__dirname, "list");

let reg = /(\.jpg|\.jpeg|\.png|\.gif|\.webp)/;
let count = 3;

fs.readdir(originPath, (err, res) => {
  if (!err && res) {
    let imgList = res.filter((item) => reg.test(item));
    imgList.forEach((listItem) => {
      for (let i = 1; i <= count; i++) {
        fs.copyFile(
          `${originPath}/${listItem}`,
          `${newPath}/${i}-${listItem}`,
          (err, res) => {
            if (err) {
              console.log(err);
            }
          }
        );
      }
    });
  }
});
```
