---
title: 使用React搭建后台管理系统
date: 2019-10-10
tags: React
keywords: React
description: React
top_img: ../../../../img/1.jpg
cover: ../../../../img/1.jpg
---



# 使用React搭建后台管理系统
## 1.初始化react脚手架
```
npx create-react-app 项目名称
npm init create-react-app 项目名称
yarn create-react-app 项目名称
```
## 2.进入我们创建的react 项目下，运行:``` yarn start```可以启动我们的项目
## 3.安装node-sass包:```yarn add npde-sass```
## 4.配置路由 运行```yarn add react-router-dom```
    将路由引入App.js(react框架的主入口js文件)

```js
import React from 'react';
import {BrowserRouter as Router,Switch  } from  "react-router-dom"

import './App.scss';

function App() {
  return (
   <Router>
     <Switch>
       <Route path="/" exact></Route>
       <Route path="/home" render={(props) => {}}></Route>
       <Route component="Empty"></Route>
     </Switch>
   </Router>
  );
}

export default App;

```

 
