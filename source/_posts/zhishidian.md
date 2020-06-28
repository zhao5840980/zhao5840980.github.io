---
title: 开发中用到的知识点总结
top_img: ../../../../img/3.jpg
cover: ../../../../img/3.jpg
tags: JS
---
# 开发中用到的知识点总结

## js中将数字格式化为小数点后保留2位
### 1.如果保留两位小数时需要四舍五入

```js
var num=3.446242342;
num=num.toFixed(2);
```
### 2.如果不希望四舍五入，则：
```js
var num=3.446242342;
num=parseInt(num*100)/100;
```