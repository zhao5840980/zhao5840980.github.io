---
title: 使用moment处理和获取对应的时间

tags: JS
keywords: JS
description: JS
top_img: ../../../../img/6.jpg
cover: ../../../../img/6.jpg
---
# 使用moment处理和获取对应的时间
## 我们在开发是经常需要处理日期和时间，moment.js是一个很好的处理时间的js第三方库

### [moment官方文档链接](http://momentjs.cn/docs/#/use-it/node-js/)

### 1.获取具体日期：xxxx-xx-xx
```js
const dayData1 = moment(new Date())
      .add('year', 0)
      .format('YYYY-MM-DD')
```

### 2.获取当前的月份

```js
  const monthData1 = moment()
      .locale('zh-cn')
      .format('YYYY-MM')
```
### 3.获取当前的年份

```
const yearData1 = moment()
      .locale('zh-cn')
      .format('YYYY')
```