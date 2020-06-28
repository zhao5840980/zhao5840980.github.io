---
title: 开发中常用的excel导出功能
tags: Vue
keywords: vue 
description: vue
top_img: ../../../../img/2.jpg
cover: ../../../../img/2.jpg
---

# 后台管理系统中的导出功能
## 项目需求：前端需要传入对应的参数给后端，后端写的接口是post方式导出数据

### 1.get的下载方式

通常下载方式如下：
```js
let url =  xxxx.action?a=xx&b=yy;
 window.location.href = url;
 // 或者
 window.open(url, '_self')
```
弊端：当请求参数较多时，get的方式无法使用，这时候需要考虑post的方式，但是直接通过ajax的post的方式无法调用浏览器的下载功能

### 2.Post的下载方式

原理：创建一个隐藏的from表单的提交刷新功能，实现下载，代码如下：
```js
// vue项目代码
  // 导出excel
    postExcelFile(params, url) {
      //params是post请求需要的参数，url是请求url地址
      var form = document.createElement("form");
      form.style.display = "none";
      form.action = url;
      form.method = "post";
      document.body.appendChild(form);
    // 动态创建input并给value赋值
      for (var key in params) {
        var input = document.createElement("input");
        input.type = "hidden";
        input.name = key;
        input.value = params[key];
        form.appendChild(input);
      }

      form.submit();
      form.remove();
    }

    //调用
     this.postExcelFile(
        { currentPage: 2, pageSize: 20 },
        'url/xxxxxxx/'
      );
```

注意点：传给后端的参数不是json对象的形式，而是currentPage=2&pageSize=20 在url地址后面拼接字符串传给后端
