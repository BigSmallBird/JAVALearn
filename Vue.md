# VUE 前端框架

## 什么是 Node.js

Node.js 是一个开源，跨平台的 JavaScript 运行环境。

## 什么是 VUE

Vue 是用于构建用户界面的渐进式框架，在有 HTML， CSS， JavaScript 的基础很容易上手

<a href="https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-5-Vue/1/video/vue%20%E5%AE%98%E6%96%B9%E4%BB%8B%E7%BB%8D.mp4 ">Vue 简介</a>

## 本地部署软件安装以及 Vue 配置

其实我们需要的不是 node.js 而是 node.js 里面的 npm ,安装 node.js

就等同于安装了 npm

下载使用国内源下载：

[CNPM Binaries Mirror](https://npmmirror.com/mirrors/node/)

或者在其官网下载：

https://www.nodejs.tech/en

## 安装 Vue CLI（手脚架）

Vue CLI 为 Vue 工程的升级版

对于 windows 系统下载命令

```powershell
npm install -g @vue/cli
```

直接下载是使用 国外源下载，网速慢

开源通过 CNPM ( 淘宝镜像 )下载国内源

```powershell
npm install -g cnpm --registry=https://registry.npmmirror.com

cnpm install -g @vue/cli
# 安装后验证一下是否安装成功
vue -v
```

## 创建 Vue 工程

我们现在开源用 Vue 来创建工程了

```powershell
// 语法 vue create xxxx
vue create vue3.0-project
```
启动项目：  
```bash
cnpm run serve
```

## Vite 脚手架安装 

Vite 是一种新型前端构建工具，能够显著提升前端开发体验，最明显的感觉就是快  

我们使用 Vue CLI 的时候，没当我们更改一个文件的部分代码，Vue CLI 就会重新编译，编译时间会随着工程的日渐庞大变的愈来愈慢， Vite 的强大就是无论项目多大，都可以实现秒编译。  

### 环境需求
> Node.js > 12.0.0

### 创建项目

创建项目的命令：
```bash
npm create vite@4 xxx --template vue
# 或者
yarn create vite@4 xxx --template vue
```
接下来我们比 Vue CLI 多一步：
```bash
//下载依赖
npm install
//启动项目
npm run dev
```
> npm 下载地址在国外服务器，为全世界服务，优点是最标准，稳定。缺点是国内下载慢，有时候会失败。cnpm 是淘宝做的国内镜像，但是有时候会出现兼容性问题

## Vue 工程目录介绍

Vue 创建完成后，我们对其工程目录简单了解

src  
 |- assets: 存放项目中需要用到的资源文件，css, js, images 等。   
 |- componets: 存放 Vue 开发中一些公共组件：例如项目初始化的 header.vue, footer.vue 就是公共组件。   
 |- router: vue 路由的配置文件。   
 |- views: 存放页面文件。   
 |- app.vue: 根组件。
 |- main.js：项目的入口文件，定义了 vue 实例，并引入根组件 app.vue， 将其挂载到 index.html 中的 id 为 ‘app’ 的节点上

 ## 前后端分离  

* 前端：主要负责页面展示（View）和逻辑（Controller） 
* 后端：主要负责 Model，业务逻辑，数据校验，数据处理，数据存储。
  
在前后端耦合的框架下，前端页面展示， 受到 java 写的 control 代码的限制。
而前后端分离的框架下，前端页面展示可以相对独立完成，减少对后端服务的依赖，只需要项目初期约定好 Model。  

> 前端开发是，可以假设后端 API 响应返回，模拟一份 model 数据，即可不依赖后端独立开发，这个过程叫做 mock

## 声名式渲染 

### 初识 Vue 代码结构

我们先学习在 app.vue 里面写代码
对于
```html
// template 即模板的意思，每个 vue 文件里必须有一个，
<template>
    <div id="app"></div>
</template>

// 在这里写 js 逻辑相关代码
<script>
    export default {
        name: "app"
    };
</script>

// 这里写样式代码
<style></style>
```

通过对上面的片段解读，我们知道，每一个 Vue 文件由三部分组成，template, script, style, 它们分别对应 HTML， JavaScript， CSS。   
另外需要知道的式，在 template 里面只允许由一个块状元素，通常情况下在 template 里面写的都是 div。  
像下面这几种的写法都是**错误**的：  
```html

<!--错误示范1-->
<template>
    <div></div>
    <div></div>
</template>

<!--错误示范2-->
<template>
    <div></div>
    <ul>
    </ul>
</template>

<!--错误示范3-->
<template>
    <div></div>
    <span></span>
</template>
```
总之， template 标签里面只能由一给块状元素，其他元素在这个元素内部

### 声名式渲染  

在学习 JavaScript 的时候我们使用的模板字符串来将数据渲染在页面中的，那时候我们用的一个符号是 ```${}```,具体代码如下：  
```html
let element = '<div>${data.name}</div>';
```
这个代码很麻烦，

在 Vue 里面，只需要使用**差值表达式**即可解决，就是俩大括号```{{}}```   
### 差值表达式渲染--字符串
先从最简单的入手，如果在页面中渲染一串字符串（啦啦啦）  
在 js 里面：
```js
let str = "啦啦啦";
let template = '<h2>${str}</h2>';
```
但是在 Vue 里面，我们先在 template 部分明确插值表达式的位置，并填充变量名：  
```html
<template>
    <h2>{{title}}</h2>
</template>
```
在 script 里面定义字符串变量：
```html
<script>
    export default {
        // 模板名称
        name: "app",
        // 页面中数据存放的地方
        data() {
            return {
                title: "啦啦啦"
            };
        }
    };
</script>
```
还可以添加样式
```html
<script scope>
    h2 {
        color: deeppink;
        border: 1px solid #ccccccc;
    }
</script>
```
