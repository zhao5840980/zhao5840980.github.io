---
title: bpmn-JS在vue里面的实现
date: 2019-4-19 20:00:00
tags: Vue
keywords: vue 流程图
description: vue 流程图
top_img: ../../../../img/bpmn-js-editor.png
cover: ../../../../img/bpmn-js-editor.png
---
# 最近公司需要做流程图所以做了一些调研
  ## 1. [antvis/g6-editor](https://github.com/antvis/g6-editor)
 现在已经不再维护，上面的超链接有对应的文档
 ![](../../../../img/g6-editor.gif)
 ## 2. [goJS](https://gojs.net/latest/learn/index.html)
 ![gojs](../../../../img/gojs.png)
 ## 3. [d3实现流程图jQ版](https://github.com/zhangyuanliang/flowchart)
 [d3实现流程图vue版](https://github.com/zhangyuanliang/flowchart)

 ## 4. [bpmn](https://github.com/zhangyuanliang/flowchart-vue)
 [bpmn-examples](https://github.com/bpmn-io/bpmn-js-examples) bpmn对应的例子的下载地址

 ![bpmn](../../../../img/bpmn-js-editor.png)
 ### 应领导要求：我最后决定使用bpmn做流程图，基于bpmn做流程图相应功能的扩展
 #### 1-1.将bpmn-js集成到vue中
 ##### 需要下载的包：

```json
    "bpmn-js": "^5.0.4",
    "bpmn-js-properties-panel": "^0.32.1",
    "camunda-bpmn-moddle": "^4.1.2",
```

##### home.vue文件对应的代码：
```js
<template>
  <div class="content with-diagram" ref="drop">
    <div class="canvas" id="js-canvas" ref="canvas"></div>
    <div class="properties-panel-parent" id="js-properties-panel" ref="js-properties-panel"></div>
    <ul class="buttons">
    <li>
      download
    </li>
    <li>
      <a id="js-download-diagram" href title="download BPMN diagram">
        BPMN diagram
      </a>
    </li>
    <li>
      <a id="js-download-svg" href title="download as SVG image">
        SVG image
      </a>
    </li>
  </ul>

  </div>
</template>

<script>
// @ is an alias to /src
import BpmnModeler from 'bpmn-js/lib/Modeler';

import propertiesPanelModule from 'bpmn-js-properties-panel';
import propertiesProviderModule from 'bpmn-js-properties-panel/lib/provider/camunda';
import camundaModdleDescriptor from 'camunda-bpmn-moddle/resources/camunda.json';
import  "bpmn-js/dist/assets/diagram-js.css";
import  "bpmn-js/dist/assets/bpmn-font/css/bpmn-embedded.css";
import  "bpmn-js-properties-panel/styles/properties.less";
// import diagramXML from './resources/newDiagram.bpmn';



import {
  debounce
} from 'min-dash';
export default {
  
 
  data(){
    return{
      bpmnModeler:null
    }
  },
  methods:{
    initBpmnModule(){
      var container = this.$refs.drop;

      var canvas = this.$refs.canvas;

      this.bpmnModeler = new BpmnModeler({
        container: canvas,
        propertiesPanel: {
          parent: '#js-properties-panel'
        },
        additionalModules: [
          propertiesPanelModule,
          propertiesProviderModule
        ],
        moddleExtensions: {
          camunda: camundaModdleDescriptor
        }
      });
      // container.remove('with-diagram');
    },
    openDiagram(xml) {
      this.bpmnModeler.importXML(xml, function (err) {
        if (err) {
          container.removeClass('with-diagram').addClass('with-error');
          container.find('.error pre').text(err.message);
          console.error(err);
        } else {
          container.removeClass('with-error').addClass('with-diagram');
        }
      });
    },
    createNewDiagram() {
      const diagramXML=`<?xml version="1.0" encoding="UTF-8"?>
          <bpmn2:definitions xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:bpmn2="http://www.omg.org/spec/BPMN/20100524/MODEL" xmlns:bpmndi="http://www.omg.org/spec/BPMN/20100524/DI" xmlns:dc="http://www.omg.org/spec/DD/20100524/DC" xmlns:di="http://www.omg.org/spec/DD/20100524/DI" xsi:schemaLocation="http://www.omg.org/spec/BPMN/20100524/MODEL BPMN20.xsd" id="sample-diagram" targetNamespace="http://bpmn.io/schema/bpmn">
            <bpmn2:process id="Process_1" isExecutable="false">
              <bpmn2:startEvent id="StartEvent_1"/>
            </bpmn2:process>
            <bpmndi:BPMNDiagram id="BPMNDiagram_1">
              <bpmndi:BPMNPlane id="BPMNPlane_1" bpmnElement="Process_1">
                <bpmndi:BPMNShape id="_BPMNShape_StartEvent_2" bpmnElement="StartEvent_1">
                  <dc:Bounds height="36.0" width="36.0" x="412.0" y="240.0"/>
                </bpmndi:BPMNShape>
              </bpmndi:BPMNPlane>
            </bpmndi:BPMNDiagram>
          </bpmn2:definitions>`;
  this.openDiagram(diagramXML);
}
  },
  mounted(){
    this.initBpmnModule();
    this.createNewDiagram();

  }

};
</script>
<style lang="less">
* {
  box-sizing: border-box;
}

body,
html {

  font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;

  font-size: 12px;

  height: 100%;
  max-height: 100%;
  padding: 0;
  margin: 0;
}

a:link {
  text-decoration: none;
}

.content {
  position: relative;
  width: 100%;
  height: 100%;
  height: 900px;

  > .message {
    width: 100%;
    height: 100%;
    text-align: center;
    display: table;

    font-size: 16px;
    color: #111;

    .note {
      vertical-align: middle;
      text-align: center;
      display: table-cell;
    }

    &.error {
      .details {
        max-width: 500px;
        font-size: 12px;
        margin: 20px auto;
        text-align: left;
        color: #BD2828;
      }

      pre {
        border: solid 1px #BD2828;
        background: #fefafa;
        padding: 10px;
        color: #BD2828;
      }
    }
  }
  &:not(.with-error) .error,
  &.with-error .intro,
  &.with-diagram .intro {
    display: none;
  }

  .canvas {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
  }

  .canvas,
  .properties-panel-parent {
    display: none;
  }

  &.with-diagram {
    .canvas,
    .properties-panel-parent  {
      display: block;
    }
  }
}


.buttons {
  position: fixed;
  bottom: 20px;
  left: 20px;

  padding: 0;
  margin: 0;
  list-style: none;

  > li {
    display: inline-block;
    margin-right: 10px;

    > a {
      background: #DDD;
      border: solid 1px #666;
      display: inline-block;
      padding: 5px;
    }
  }

  a {
    opacity: 0.3;
  }

  a.active {
    opacity: 1.0;
  }
}

.properties-panel-parent {
  position: absolute;
  top: 0;
  bottom: 0;
  right: 0;
  width: 260px;
  z-index: 10;
  border-left: 1px solid #ccc;
  overflow: auto;
  &:empty {
    display: none;
  }
  > .djs-properties-panel {
    padding-bottom: 70px;
    min-height:100%;
  }
}
  
</style>

```


### 讲解：


##### 1.在html中写了最外层的放置面板，在其中使用了两个div,一个是绘制canvans另一个是属性面板绑定，分别引入BpmnModeler、bpmn-js-properties-panel、propertiesProviderModule、camundaModdleDescriptor、引入对应css库。

##### 2.在mounted中分别实例化Bpmnmodeler,由于Bpmnd都是以模块化来定义的，在实例化时在参数为一个对象中分别赋值挂载对应的模版；在初始化完以后还需要初始化对应的流程图的数据文件，不然是显示不出来的，同时也编辑不了










