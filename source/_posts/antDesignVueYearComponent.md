---
title: antDesign vue 年份选择器组件

tags: Vue
keywords: Vue
description: Vue
top_img: ../../../../img/4.jpg
cover: ../../../../img/4.jpg
---

# antDesign vue 年份选择器组件
## 项目中有需要年份选择组件的需求，但在antDesign官方文档中是没有年份选择器的。
## [antDesign官方文档链接](https://www.antdv.com/docs/vue/introduce-cn/)

```html
 <a-date-picker mode="year" :defaultValue="yearValue" format="YYYY" :value="yearValue"
                         :allowClear="false" :open="isopen" @openChange="handleOpenChange" @panelChange="onChange" />
```
> 上面的代码是在年份组件的相关配置，在这其中需要在vue data中绑定“yearValue：null”和“isopen:false”

```js
   handleOpenChange (status) {
      if (status) {
        this.isopen = true
      } else {
        this.isopen = false
      }
    },
     onChange (val) {
      this.yearValue = val
      console.log(val.format('YYYY'))
      this.startTime = val.format('YYYY')
      this.nowYearData = val.format('YYYY')
      this.isopen = false
      this.initData()
    },
```
> 在选择日期后，获取到的是moment对象，需要格式化一下,然后就可以获取到我们选择的年份

