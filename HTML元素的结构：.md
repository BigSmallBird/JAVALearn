# HTML元素的结构：

HTML 的语言形式为标签（如<p>）包围的 HTML元素，如:

## HTML 标签:

 //<p>//    就是一个典型的 HTML 标签。 HTML标签有以下特点

* 由**尖括号**包围关键词组成，比如： // <p>, <h1>, <div>, <span> // 等
* 通常成对出现，比如// <div> 和 </div> //第一个是开始标签，第二个是结束标签，要注意开始标签和结束标签区别在于 **结束标签多了一个“/”**
* 虽然标签通常成对出现，但是并不是所有标签都对应有结束标签 比如 // <input>, <img> //等，它们往往是单独出现。

**注意HTML里的标签对大小写不敏感**

### 刨析一个 HTML 元素：

![img](https://document.youkeda.com/P3-1-HTML-CSS/1.2/2.jpg)

## HTML中的嵌套：

![img](https://document.youkeda.com/P3-1-HTML-CSS/1.2/3.png)

在HTML中，元素可以发生**嵌套**。

**第一个元素**

<p>HTML是一门伟大的语言！</p>

开始标签：//<p>// HTML 是一门伟大的语言//</p>//

**第二个元素**

<div><p>HTML是一门伟大的语言</p></div>

开始标签// <div> // 结束标签// </div> //

第一个元素整体为作为// <div></div> // 的内容，镶嵌在// <div></div> //内部，从结构上发生**嵌套**关系。

div 是 p 的父元素，![img](https://document.youkeda.com/P3-1-HTML-CSS/1.2/16.png)

## 对于完整的HTML文件：

![img](https://document.youkeda.com/P3-1-HTML-CSS/1.2/4.jpg)

有以上元素

1. //**<!DOCTYPE html>**//    
   
   **作用**：告知浏览器该页面的文档类型，指示 web 浏览器使用哪个 HTML 版本编写页面。    
   
   **位置**：//<!DOCTYPE html>// 声名必须是 HTML 文档的第一行， 位于 // <html> // 标签之前。     
   
   //**<!DOCTYPE html>** // 声名对大小写没有要求。而且其声名没有结束标签   

2. //**<html lang="en">...</html>**//
   
   此元素可以告知浏览器其自身是一个 HTML 文件。
   
   //<html> 与 </html> // 标签限定了文档的开头和结尾，在他们之间是文档的头部和主体。文档的头部由 //<head>// 标签定义，而主体由 //<body>// 标签定义。
   
   **.lang 属性（语言属性）:** 当搜索引擎或者浏览器拿到语言属性后，由可能做一些针对指定语言的辅助操作， ‘en’ 表示英文。

3. **标签属性**
   
   标签可以拥有0或者多个标签属性，注意：标签属性与标签名称，与标签属性之间需要一个空格隔开。
   
   标签属性可以赋予标签更多信息，如 **key = "value"** 
   
   常见的标签属性 ： class, id, style, lang, src 等 

4. **文档的头部// <head>...</head> //**
   
   head 元素定义文档的头部，我们通常在这里引用样式表，提供元素信息
   
   文档的头部 // <title>...</title> //定义文档的标题，在网页上体现为网页标签的标题
   
   一个//<head>// 元素只能包含一个//<title>// 元素。
   
     （元信息。 又叫元数据，就是描述数据的数据。这里主要指文件的概要信息）

5. **文档的主体 //<body>..</body>//**
   
   .body 元素被定义为文档的主体，包含文档的所有内容（比如文本，超链接，图像，表格和列表等等）。

## 对于 HTML 的注解：

![img](https://document.youkeda.com/P3-1-HTML-CSS/1.2/6.png)

## 对于 // <img> //:

// <img> // 标签除了存放 jpd, jpeg, png 等图片文本之外，还可以存放 gif 动态图片

## 对于 ATL：

图片标签存在 alt 属性，这个属性为器图片的代替文本。如果图片 URL 错误或者不支持格式，则显示代替文本。

## 对于链接标签：

HTML 提供了一个链接标签 //<a>// 给文章添加标签链接。 //<a>// 是一个内联标签，用户点击后，浏览器会跳转到指定网址。

![img](https://document.youkeda.com/P3-1-HTML-CSS/1.3/12.png)

对于//<a>// 标签，不仅可以放置文字，也可以放置其他元素，比如段落，图像，多媒体等。

![img](https://document.youkeda.com/P3-1-HTML-CSS/1.3/14.png)

### 标签属性：

* **href** herf 属性是给出链接指向的网址，他的值一般是一个 URl 或者是一个锚点。
* **title** title 属性给出链接的说明信息。鼠标悬停在链接上方，浏览器会将这个属性的值，以提示块的形式显示出来。
* **target** taget 属性指向如何展示打开的链接。target 属性的值也可以是“self”, "blank", "parent", "top" 四个关键词之一。![img](https://document.youkeda.com/P3-1-HTML-CSS/1.3/19.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

对于_self标签，其描述链接为在本基础上打开。

而_blank标签，其为另外开启一个页面打开。

## 对于列表标签：

HTML 提供了无序列表标签//<ul>//和有序列表标签//<ol>// 

### 对于无序列表：

![img](https://document.youkeda.com/P3-1-HTML-CSS/1.3/20.png)

使用//<ul> .... </ul>// 经行连接。

### 对于有序列表：

![img](https://document.youkeda.com/P3-1-HTML-CSS/1.3/21.png)

使用//<ol>...</ol>// 经行连接

## 对于//<form>//标签：

在web中，用户的交互主要是通过“链接”来浏览网址的。但是我们也需要其进行输入。

- 注册并登录网站
- 输入个人信息（姓名，地址，信用卡详细信息……）
- 过滤内容（使用下拉菜单，复选框……）
- 进行搜索
- 上传文件
- ...

为了满足这些需求，HTML 提供了交互式 表单控件：

- 文本输入（一行或多行）
- 单选按钮
- 复选框
- 下拉菜单
- 上传小部件
- 提交按钮
- ...

## **//<form>//** 标签存在输入跨，按钮等表单元素。属于同一个表单的控制键，要在同一个 form 块状里。

* **action：** 一个处理表单信息的程序所在的 URL ，所述表格信息将在表单提交时被发送到指顶的URL 里
* **method：** 他的值可以是 GET 或者 POST ，用来规定 如何 发送表单信息。 

通常，表单会被发送到**服务器里** 。 

```html
<!-- <form>是块状标签，要注意：<form>标签不能嵌套<form>标签 -->
<form action="">
  <!-- 这里会有一些表单控件 -->
</form>
<!-- action=""则表单信息将提交到当前页面 -->
<form action="">
  <input type="text" />
</form>
```

单行文本输入框：input type = “文本类型”

##### 占位文本：placeholder

//<input type="text" placeholder="XXX"/>//

##### 输入框名字：name

为了区别输入框，我们需要给输入框加上标签属性 name

//<input type="text" placeholder="sb" name="章二三"/>

对于这个 name 其实就是提交表单数据后的哪个 **键**

##### 输入框的值：value

如果希望在输入框中预填信息，可以用标签属性 value 表示

//<input type="text" placeholder="您贵姓" name="我是键" value="我是值">//

##### 不可修改的输入框 readonly 与 disabled

一些情况，我们会给玩家分配信息，这些信息不希望被改动 那么：

//<input type="text" placeholder="XXX" name="XXX" value="SB" readonly>//  **该文本框现在我只读权限**

![image-20250516180340279](C:\Users\15956\AppData\Roaming\Typora\typora-user-images\image-20250516180340279.png)

对于该两个属性的区别：

## 多行文本输入  和  密码输入：

### 多行文本输入：

个性签名是个多行文本框。当多行文本输入框中输入内容超过一行的长度的时候，它会自动换行，而单行不会

### 这里我们用 textarea 进行多行文本输入：

```html
<!-- name属性表示表单元素的名称，placeholder属性表示表单元素的占位文本 -->
<textarea
  name="sign"
  rows="5"
  cols="30"
  placeholder="请输入个性签名"
></textarea>
```

其中 **rows** 和 **cols** 分别表示行数（高度）和文本域的可视宽度：rows：行数；cols：文本域的可视宽度：

这两个属性可写可不写，//<textarea></textarea>//就表示一个多行输入框

## 密码输入框：

密码输入框和昵称输入框有点区别，用户输入内容会以黑点的形式显示

如何做到输入内容不直接显示？我们只需要把表单标签//<imput>//中的标签属性 **type="text"**改成**type=“password”** 

```html
<!-- type属性表示表单元素的类型，name属性表示表单元素的名称，placeholder属性表示表单元素的占位文本 -->
<input type="password" name="password" placeholder="密码" />
```

## 单选框和复选框：

### 单选题：

对于一些情况我们需要单**选框**

```html
<!-- type属性表示表单元素的类型，name属性表示表单元素的名称，value属性表示表单元素的值 -->
<input type="radio" name="gender" value="male" />
<input type="radio" name="gender" value="female" />
```

所谓单选框，其实是把控件类型 **type="text"** 改为 **type="radio"** 大部分 表单元素都是通过改变标签属性type的值来实现。

//*// 属于同一到单选题的每个单选按钮，应该用**相同**的 **name** 属性值。

我们会用 **value=“male”** 表示男性 **value="female"** 表示女性

```html
<input type="radio" name="gender" value="male" />男
<input type="radio" name="gender" value="female" />女
```

但是这样的点击范围比较小，我们可以用//<label></label>//进行镶嵌；

```html
<input id="male" type="radio" name="gender" value="male" />
<label for="male">男</label>
<input id="female" type="radio" name="gender" value="female" />
<label for="female">女</label>
```

### 复选框：

复选框其实就是到单选题，用户可以进行多个选项的选择。类型是 checkbox

//<input type="checkbox"/>//

```html
<label> <input type="checkbox" name="interest" value="coding" />编程 </label>
<label> <input type="checkbox" name="interest" value="other" />其他 </label>
```

属于同一道多选题的每个复选框元素，应该有想同的 name 属性

## 选项菜单：

### 单选菜单：

![image-20250519125540163](C:\Users\15956\AppData\Roaming\Typora\typora-user-images\image-20250519125540163.png)

对于某种情况：我们给用户提供几个固定选项，因为实际问题可以产生很多选项，属于我们不采用单选框，而是应用新标签 // <select> // 和 // <option> // 选项菜单。![img](https://document.youkeda.com/P3-1-HTML-CSS/1.4/6.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

```html
<select name="career">
  <option value="default">请选择职业</option>
  <option value="staff">公司职员</option>
  <option value="freelancer">自由职业者</option>
  <option value="student">学生</option>
  <option value="other">其他</option>
</select>
```

这是一个单选菜单，如果用户选择“学生” 那么表单数据就是 {key="career" value="student"} 提交的内容并不是 “学生” 而是 //<option>// 标签的标签属性 value 的值， 所以每个 option 的 value 值要互不相同。

### 对于多选菜单：

我们只需要给 //<select>// 标签添加一个属性  multiple  就行了，可以通过ctrl + 鼠标 进行多个选项的选择。

```html
<select name="career" multiple>
  <option value="default">请选择职业</option>
  <option value="staff">公司职员</option>
  <option value="freelancer">自由职业者</option>
  <option value="student">学生</option>
  <option value="other">其他</option>
</select>
```

## 按钮：

我们将使用 html 里的 //**<button>/**/ 标签

// <botton> 注册 </botton>//

应为 **botton标签是闭合标签** 所以我们可以在其中放上文字，图片，图标等等。

这个botton 放在 form 中会在点击的时候自动提交表单数据，但是在 botton 提交表单数据这一点是有浏览器兼容性问题， 一般还需要加上 type=“submit” 来却保数据的提交。

```html
<button type="submit">注册</button>
```

```html
<form action="">
  <input type="text" name="name" placeholder="请输入昵称" />
  <textarea
    name="sign"
    rows="5"
    cols="30"
    placeholder="请输入个性签名"
  ></textarea>
  <input name="password" type="password" placeholder="请输入密码" />

  <label> <input type="radio" name="gender" value="male" />男 </label>
  <label> <input type="radio" name="gender" value="female" />女 </label>

  <label> <input type="checkbox" name="interest" value="coding" />编程 </label>
  <label> <input type="checkbox" name="interest" value="other" />其他 </label>

  <select name="career">
    <option value="default">请选择职业</option>
    <option value="staff">公司职员</option>
    <option value="freelancer">自由职业者</option>
    <option value="student">学生</option>
    <option value="other">其他</option>
  </select>

  <button type="submit">注册</button>
</form>
```

![img](https://document.youkeda.com/P3-1-HTML-CSS/1.4/9.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

## HTML内部添加样式：

关于如何在标签里引入CSS样式。

### 在标签里添加声名：

声名的关键词是 **style** 后接 = 再接 “” ， 即： style=“”

```html
<input type="text" placeholder="手机号码" style="">
```

* 声名位置不分先后

```html
<input type="text" style="" placeholder="手机号码">
<!-- 或者 -->
<input style="" type="text" placeholder="手机号码">
```

* 与其他关键词之间用空格隔开
  
  关键词和关键字之间要用空格隔开，比如 **type** 和 **placehorld**
  
  <input type="text" placeholder="手机号码" style="">

* 在引号之间添加样式

```html
<p style="font-size:14px;color:white"></p>
```

## 字体大小/字体粗细：

在CSS中，样式是由属性和值组成，中间用冒号（：）隔开，用分号（；）收尾，其中属性可以理解位身高，体重，值可以理解为 1.8 m，60 kg，在现实生活中，我们用这样一对组合描述人，在CSS我们来描述字体粗细，大小，颜色

![img](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-1-HTML-CSS/1.5/key_value.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

#### 字体大小：

设置格式：font-size:36px

```html
<!-- 设置字体的大小为12px -->
<p style="font-size: 12px;">
  一个轻量级和模块化的前端框架，用于开发快速和强大的web接口。
</p>
<!-- 设置字体的大小为24px -->
<p style="font-size: 24px;">
  一个轻量级和模块化的前端框架，用于开发快速和强大的web接口。
</p>
```

#### 字体加粗:

设置格式：font-weight:100

```html
<p style="font-weight: 200;">优课达--学的比别人好一点～</p>
<p style="font-weight: lighter;">优课达--学的比别人好一点～</p>
<p style="font-weight: 400;">优课达--学的比别人好一点～</p>
<p style="font-weight: normal;">优课达--学的比别人好一点～</p>
<p style="font-weight: 700;">优课达--学的比别人好一点～</p>
<p style="font-weight: bold;">优课达--学的比别人好一点～</p>
```

#### 字体颜色：

设置格式：color:XXX

```html
<h3 style="color:#ff9a9e;font-weight:700;font-size: 24px;">UIzards</h3>
<h4 style="font-size: 16px;color: #474d5d;font-weight: 400;">
  Senior UX Designer
</h4>
<p style="font-size: 14px;color:#84868d;">
  Nam liber tempor cum soluta nobis eleifend option congue nihil imperdiet
  doming id quom placerat facer possim assum. Typi non habent claritatem
  insitam; est usus legentis in iis qui faorum claritatem. Investigationes
  demonstraverunt lectores legere me lius quod ii legunt saepius.
</p>
```

#### 文字居中/居左/居右：

设置格式：

text-align: center

text-align: left

text-align: right

#### 行高：![img](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-1-HTML-CSS/1.5/%E8%A7%A3%E9%87%8A%E8%A1%8C%E9%AB%981.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

行高设置格式：

line-height: 30px

**其实际作用：第一个作用：改变段落中行与行之间的距离**

```html
<p>
  We understand every aspect of project and we put a great amount of time in
  understanding the project.
</p>

<p style="line-height:32px;">
  We understand every aspect of project and we put a great amount of time in
  understanding the project.
</p>
```

**第二个作用是使文字上下居中**

```html
<button
  style="width: 120px;height:50px;text-align: center;line-height:50px;color:white;font-size: 18px;"
>
  提交
</button>
```

### 字体：

关键字 + 值：font-family: sans-serif;

* 多个字体之间用英文逗号隔开
* 字体名称中间有空格要加引号，单引号和双引号都可`"Times News Roman"`
* 中文名称要加引号。

## CSS三种引入方式：

1. **行内样式**
2. ![img](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-1-HTML-CSS/1.6/line-style-css.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

```html
<p style="font-size: 18px;font-weight: 700;color: blue;">
  这是一个p标签，和第三个p标签样式一样
</p>
<p>这是一个中立的p标签</p>
<p style="font-size: 18px;font-weight: 700;color: blue;">
  这是一个p标签，和第一个p标签样式一样
</p>
```

对于多个HTTP标签的style，我们要想办法把每一段标签中的CSS样式抽出来，搞成已段，放在HTTP的某个位置。——内部映入

2. **内部引入**
   
   ![img](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-1-HTML-CSS/1.6/head-style-css.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

**抽离步骤** 

(1). 首先我们将每一个标签里的CSS样式抽离出来。

(2). 然后在和 head 标签里声名与i个 //<style> </style>//标签

(3). 将这些样式放在//<style></style>// 标签里![img](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-1-HTML-CSS/1.6/%E5%A4%B4%E9%83%A8%E6%A0%B7%E5%BC%8F%E6%8A%BD%E7%A6%BB.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

(4). 将相同标签的样式写在相同的大括号里，大括号前面加上标签名：

```css
p {
  font-size: 16px;
  color: #ffffff;
}
```

![img](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-1-HTML-CSS/1.6/%E6%A0%B7%E5%BC%8F%E9%87%8D%E7%82%B9%E8%A7%A3%E9%87%8A.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

* 不要忘记写声名标签// <style></style>//
* 样式要用花括号括起来
* 每个样式后面要用分号结尾

#### 外部样式

随着代码量的增加，整个HTML 文件会显得头重脚轻，CSS代码会比HTML多，所以我们 让代码实现分离HTML负责结构，CSS负责样式

![img](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-1-HTML-CSS/1.6/%E5%A4%96%E9%83%A8%E6%A0%B7%E5%BC%8F%E5%BC%95%E5%85%A5%E6%AD%A5%E9%AA%A4.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

* 新建一个 **index.php** 文件
* 将 html 代码头部中的 **style** 标签内的样式全部拷贝出来
* 将复制的 CSS 样式粘贴进 index.php 文件
* 建立起 HTML 与 CSS 文件的联系，即用 **link** 标签引入 CSS 文件

### 对于 CSS ：

CSS 进行注解的方法是 /*  */ 

### 对于 **link** 标签的属性：

```html
<link rel="stylesheet" type="text/css" href="index.css" />
```

1. **rel** 属性： **rel** 属性规定了当前文档与被链接文档之间的关系，但是 rel 属性的 **stylesheet** 值被所有的浏览器支持（stylesheet 的意思就是文档的外部样式）
2. **type** 属性： **type** 属性规定了被链接的为文档的 MIME 类型。type 属性的常见值是 **text/css**  该类型描述的样式表
3. **herf**  属性： **herf** 属性后跟的是要引入的链接地址

## 绝对路径：

是指文件在硬盘上的真正存在的路径。当文件在不同客户端传递时，会造成文件不存在的读取错误

## 相对路径：

**相对路径避免了对路径造成的页面资源丢失现象，在引入外部资源的时候，都会选择使用相对路径**

* **./**   :当前文件夹目录，比如在下面这个目录结构（test文件夹下有index.css和index.html两个文件夹），要在index.html中引入index.css就需要用 **./**![img](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-1-HTML-CSS/1.6/dangqianmulu.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

```html
<link rel="stylesheet" href="./index.css">
<!-- 或者./去掉也可以，效果是一样的 -->
<link rel="stylesheet" href="index.css">
```

* **../**   :回到上一级文件夹目录，比如下面文件目录结构中（index.html在test文件夹里面，index.css 和test文件夹在同一级目录下），我们要在index.html中引入index.css文件，需要先从test文件夹出来，才能找到index.css，从某个文件夹里面出来久用**../**![img](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-1-HTML-CSS/1.6/fanhuishangyijimulu.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

```html
<link rel="stylesheet" href="../index.css">
```

* 找到引用资源的文件所在位置，以引入资源的文件夹为基础，寻找资源
* **../** 返回上一层，如果有多层，就用多个 **../** 
* 进入某个文件夹：加入从test文件夹出来在进入css文件夹找到index.css写成**../css/index.css**

## 常见选择器：

### 标签选择器：

下图可以清楚的看到标签选择器的作用--选所有叫 p 的标签，然后就可以统一给所有的 怕标签设置样式

![img](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-1-HTML-CSS/1.6/xuanzeqi-1.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

```html
<h3 style="font-size: 25px;color: #330867;">孟航沛</h3>
<h4 style="font-size: 18px;color: #30cfd0;">平面设计师</h4>
<p style="font-size: 14px;line-height: 28px;color: #4a5252">
  专业综合性强，具有较强的综合能力，学习认真刻苦，虽然我的专业是思想政治教育，
  但是我们所学的除了马克思主义的相关理论之外，同时还学习了管理学、心理学和法学的相关理论知识，
  专业具有较强的综合性，使我具备较强的综合能力，基本能够胜任行政设计师助力这个岗位。
</p>
```

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>标签选择器</title>
    <style>
      /* 给h3标签添加样式 */
      h3 {
        font-size: 25px;
        color: #330867;
      }

      /* 给h4标签添加样式 */
      h4 {
        font-size: 18px;
        color: #30cfd0;
      }

      /* 给p标签添加样式 */
      p {
        font-size: 14px;
        line-height: 28px;
        color: #4a5252;
      }
    </style>
  </head>

  <body>
    <h3>孟航沛</h3>
    <h4>平面设计师</h4>
    <p>
      专业综合性强，具有较强的综合能力，学习认真刻苦，虽然我的专业是思想政治教育，但是我们所学的除了马克思主义的相关理论之外，同时还学习了管理学、心理学和法学的相关理论知识，专业具有较强的综合性，使我具备较强的综合能力，基本能够胜任行政设计师助力这个岗位。
    </p>
    <h3>江晓风</h3>
    <h4>UI</h4>
    <p>
      专业综合性强，具有较强的综合能力，学习认真刻苦，虽然我的专业是思想政治教育，但是我们所学的除了马克思主义的相关理论之外，同时还学习了管理学、心理学和法学的相关理论知识，专业具有较强的综合性，使我具备较强的综合能力，基本能够胜任行政设计师助力这个岗位。
    </p>
    <h3>左小青</h3>
    <h4>插画师</h4>
    <p>
      专业综合性强，具有较强的综合能力，学习认真刻苦，虽然我的专业是思想政治教育，但是我们所学的除了马克思主义的相关理论之外，同时还学习了管理学、心理学和法学的相关理论知识，专业具有较强的综合性，使我具备较强的综合能力，基本能够胜任行政设计师助力这个岗位。
    </p>
    <h3>蒋小鱼</h3>
    <h4>Java工程师</h4>
    <p>
      专业综合性强，具有较强的综合能力，学习认真刻苦，虽然我的专业是思想政治教育，但是我们所学的除了马克思主义的相关理论之外，同时还学习了管理学、心理学和法学的相关理论知识，专业具有较强的综合性，使我具备较强的综合能力，基本能够胜任行政设计师助力这个岗位。
    </p>
  </body>
</html>
```

### 选择性的叠加属性：

* 添加新属性
* 修改之前已经存在的属性效果

### 类选择器：

在前面的标签选择器中，我们通过标签选择器将很多重复的模块批量化的设置他们的样式，但是在实际开发中，仅仅一个标签害不足满足日常需求。

#### 定义：

```html
<p class="article">
  class是定义类的关键字，article是类名，类名可以任意，但是要符合规范
</p>
```

class 是定义类的关键字，artical 是类名

#### 使用：

```css
.article {
  color: red;
  font-size: 14px;
}
```

如果是内部样式，上面代码要写在//<style></style>//标签间，如果是外部 直接写在 css 文件里

#### id 选择器：

```css
#p-item {
  font-size: 24px;
  font-weight: 400;
}
```

id 选择器在文档中只会出现一次，像下面这种方式是**错的**

```html
<a href="#" id="link">点击进入详情</a>
<!-- link这个id名已经被使用，就不可以再次定义 -->
<a href="#" id="link">点击进入主页</a>
```

id 选择器 不能被 link 反复利用

不能像选择器一样，一个标签上定，下面是**错的**

```html
<a href="#" id="link linkto">点击进入详情页</a>
```

### 选择器优先级：

id选择器  > 类选选选择器 > 标签选择器

## 盒子模型：

在网页布局中，页面是有一个个大小不一样的小块区域构成，

![img](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-1-HTML-CSS/1.7/taobao-demo.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

要画一个格子，首先要了解一个新标签  --div 标签。

在学习HTML的时候，我们已经接触了类似于 div 的标签，比如

* h 标签
* p 标签
* ul 标签
* li 标签

这些标签有一个共同属性们就是独占一行

**div** 标签是一个干净透彻的矩形![img](https://style.youkeda.com/img/ham/course/f1/boxmodel.jpeg?x-oss-process=image/resize,w_655/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_ne,x_10,y_10):

由四部分组成

* 内容区 content
* 内边距 padding
* 边框 border
* 外边框 margin

![img](https://style.youkeda.com/img/ham/course/f1/boxsample.png?x-oss-process=image/resize,w_1024/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

## 内容区：content

**div** 标签写出来的时候是没有高度的 但是有宽度，宽度默认是父标签的宽度一样 比如：

```html
<div></div>
```

![img](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-1-HTML-CSS/1.7/div%E9%BB%98%E8%AE%A4%E7%9A%84%E9%AB%98%E5%BA%A6.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

## 内边距--padding：

![img](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-1-HTML-CSS/1.7/taobao-padding.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

edg：

```html
<div class="box">
    All afternoon his tractor pulls a flat wagon with bales to the barn, then back to the waiting chopped field. 
    It trails a feather of smoke. Down the block we bend with the season: shoes to polish for a big game,storm windows to batten or patch. 
    And how like a field is the whole sky now that the maples have shed their leaves, too.
</div>
```

```css
.box{
    width:300px;
    height:300px;
    background-color:purple;
    padding:20px;
    color:white;
}
```

![image-20250530144548288](C:\Users\15956\AppData\Roaming\Typora\typora-user-images\image-20250530144548288.png)

![img](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-1-HTML-CSS/1.7/padding-explore.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

> 注意：padding 区域是包含在背景颜色区域内的，也就是说背景颜色包含了 padding 和 content

### padding 分开写：

padding 默认是给矩形四周添加相同的内边距，但是实际应用当中，会又四周内边距不同的情况，因此我们要分别设置

```css
.box {
    padding: 20px;
}
```

等价于

```css
.box {
    padding-top: 20px; /*上内边距*/
    padding-right: 20px; /*右内边距*/
    padding-bottom: 20px; /*下内边距*/
    padding-left: 20px; /*左内边距*/
}
```

### padding 的简写：

padding 分为 **top** **bottom** **right** **left** 

```css
div{
    padding:20px 20px 20px 20px;
}
```

### box-sizing：

box-sizing 规定了如何计算一个元素的总宽度和高度，他有两个值 content-box， border-box，默认是 content-box。

* content-box 尺寸计算公式为：
  
  ```html
  width = 内容宽度
  height = 内容高度
  ```

* border-box 尺寸计算公式：
  
  ```html
  width = border + padding + 内容宽度
  height = border + padding + 内容高度
  ```

edg:

```css
<div class="father">
    <div class="son"></div>
</div>
```

```css
.father{
    width: 200px;
    height: 100px;
    backgroung-color: #22222;
}

.son{
    box-sizing: content-box;
    width: 100%;
    height: 40px;
    backgrounf-color: #22222;
}
```

如果在原代码里添加 padding： 0px 20px; 那么就会变成这样

![image-20250530151618182](C:\Users\15956\AppData\Roaming\Typora\typora-user-images\image-20250530151618182.png)

以为黄色部分为son，其宽 20px 是在继承父辈 width 100%而变的。 

```css
.father{
    width:200px;
    height: 100px;
    background-color: #5C70CA; 
}

.son{
    /* 修改box-sizing */
    box-sizing: border-box;
    width: 100%;
    height: 40px;
    /* 添加padding */
    padding: 0px 20px;
    background-color:#FEC03E;
}
```

而box-sizing 设置成 border-box；则不会超过父辈，因为border-box 的 sizing 是根据父辈而来，而 content-sizing 是根据当前内容而来。

## 盒模型--border：

对于border（边框），其实就是包裹在padding外的一层线，会有粗细，颜色等参数。

### 给矩形设置边框：

```css
.box{
    width: 200px;
    height: 30px;
    border-width: 2px;
    border-color: grey;
    border_style: solid;
}  
```

### 对于参数：

* **border-width：** 边框粗细，单位是 px
* **border-color：** 边框的颜色
* **border-style：** 边框的线型，**solid**为实线，**dashed**为虚线

### 边框的简写：

```css
.box{
    border: 2px solid blue;
}
```

> border 后面的三个值之间使用空格隔开，值的顺序是可以忽略的

### 分别设置边框：

edg：

```css
.box{
    border-top: 1px solid black;
    border-right: 3px solid orange;
    border-bottom: 5px dashed pink;
    border-left: 10px dashed purple;
}
```

### 利用叠加原理设置边框：

```css
.box{
    width: 300px;
    height: 300px;
    background-color: white;
    border: 2px solid black;
    border-bottom: 5px solid orange;
}
```

### 无边框:

```css
.box{
    border-bottom: none;
}
```

## 圆角：

圆角的统一设置

```css
.box{
    border-radius: 12px;
}
```

完整形式：

```css
div{
    width: 200px;
    height: 200px;
    border-radius: 18px;
    border: 1px solid black;
}
```

![image-20250530155216262](C:\Users\15956\AppData\Roaming\Typora\typora-user-images\image-20250530155216262.png)

```css
div{
    width: 200px;
    height: 200px;
    background-color: violet;
    border-radius: 18px;
}
```

### 圆角分开设置

与 padding, border 属性一样， border-radius 属性也是可以拆开来设置的，只不过它没有上下左右，而是左上角左下角....：

```css
.box{
    width: 200px;
    height: 200px;
    background-color: violet;
    border-top-left-radius: 5px;
    border-top-right-radius: 10px;
    border-bottom-left-radius: 20px;
    border-bottom-right-radius: 15px;
}
```

## 阴影：

### 标签：

```html
<div class="box"></div>
```

```css
.box{
    width: 200px
    height: 200px;
    border: 1px solid #c4c4c4;
    /* x偏移量 | y偏移量 | 阴影模糊半径 | 阴影扩散半径 | 阴影颜色 */
    box-shadow: 2px 2px 2px 1px rgba(0, 0, 0, 0.2);
    border-radius: 15px;
}
```

阴影可以看成是在矩形下面有一个重叠的，同样大小的矩形，如果它在 x 轴，y 轴上移动，就会有阴影效果。

* x 偏移量： 向右为正
* y 偏移量： 向下为正
* 阴影模糊半径： 就是边线的清晰度
* 阴影扩散半径： 就是向外伸展
* 阴影颜色： 就是矩形下面那矩形的背景色

## 盒模型--margin：

margin---外边距，就是矩形与矩形之间的距离

![img](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-1-HTML-CSS/1.7/margin1.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

如果想要红色边框矩形与绿色矩形之间优边距，就可以给红色边框设置与i个 margin 属性。

```html
<!DOCTYPE html>
<html lang="en">

    <head>
        <meta charset="UTF-8">
        <title>案例</title>
        <style>
            div {
                width: 300px;
                height: 100px;
                background-color: #D5E8D4;
                border: 1px solid #82B366;
            }

            .box {
                background-color: #F5F5F5;
                border: 1px solid #FF0818;
                margin-top: 20px;
                margin-bottom: 20px;
            }
        </style>
    </head>

    <body>
        <div></div>
        <div class="box">
        <div></div>
    </body>
</html>
```

## 盒模型--display:block/none:

### display:block:

#### 块元素性质一：独占一行

在 HTML 中，块元素一般独占一行：

```html
<span> 这是一个块标签 </span>
<h3> ＋1 </h3>
<h4> +1 </h4>
```

![image-20250604222428919](C:\Users\15956\AppData\Roaming\Typora\typora-user-images\image-20250604222428919.png)

**这是块元素的性质：**独占一行：

比如下图：

![img](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-1-HTML-CSS/1.7/block1.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

第一行是两个内元素，可以并排显示，第二行是一个块元素，内容部分是蓝色，其他部分是灰色占位。所以他不能跟其他元素并行。

#### 块元素性质二： 可以设置宽高：

对于一个内元素设置宽高：

```html
<span class="XX"> 这是一个内标签 </span>
```

```css
.XX {
    width: 300px;
    height: 100px;
    background-color: #FFFFFF
}
```

![image-20250604223134175](C:\Users\15956\AppData\Roaming\Typora\typora-user-images\image-20250604223134175.png)

对于一个块标签：

```html
<div class="XX">
    这是一个div标签
</div>
```

```css
.XX {
    width: 300px;
    height: 100px;
    background-color: #fff2cc;
}
```

<img src="C:\Users\15956\AppData\Roaming\Typora\typora-user-images\image-20250604223647821.png" alt="image-20250604223647821" style="zoom: 80%;" />

#### 行内元素和块状元素之间的转换：

标签是行内元素还是块状元素，其跟本是其参数 **display**属性 ：

* 块状元素默认的 display 值为 block
* 行内元素默认的 display 值为 inline

我们想给 <span> 标签设置宽和高，首先要让 span 标签转换为块元素：

```html
<span class="XX"> XXX </span>
```

```css
.XX {
    display: block;
    width: 100px;
    height:300px;
    background-color:#CCCCCCC;
}
```

而块状元素转行内元素则相反

### display/none:

none 值就是无，也就是说，当给标签设置这个属性值，小钱就会消失，在网页布局中最长用就是 none，block 值来控制元素的显示与隐藏。

```html
<div> 盒子 </div>
<div class=".XX"> 盒子2 </div>
<div> 盒子3 </div>
```

```css
div {
    width: 300px;
    height: 100px;
    text-align: center;
    margin-bottom: 10px;
    background-color: #EEEEEE;
    line-height: 100px;
}

.XX {
    display: none;
}
```

![image-20250604225038479](C:\Users\15956\AppData\Roaming\Typora\typora-user-images\image-20250604225038479.png)

## 盒模型--display:inline/inline-block:

### display:inline:

#### 行内元素不能设置宽高；

#### 行内元素可以设置 padding：

```html
<a href="#"> 超链接 </a>
```

```css
a {
    background-color: #fff2cc;
    //内边距
    padding: 20px;
}
```

![image-20250604230744032](C:\Users\15956\AppData\Roaming\Typora\typora-user-images\image-20250604230744032.png)

#### 行内元素可以设置左右 margin，但是不能设置上下margin

```html
<a herf="#"> 点击跳转 </a>
<span> 哈哈骗你的 </span>
<div></div>
```

```css
a {
    margin-left: 40px;
    margin-right: 30px;
    margin-top: 400px;
    margin-bottom: 400px;
}

span {
    margin-left: 20px;
}

div {
    width: 300px;
    height: 50px;
    background-color: #XXXXXX;
}
```

![image-20250604231434530](C:\Users\15956\AppData\Roaming\Typora\typora-user-images\image-20250604231434530.png)

对于 inline 行内标签，margin-top 与 margin-bottom 元素其实是没有作用的

### inline-block：

**inline-block** 既有 block 的性质， 又有 inline 的性质inline-block **是一个可以在同一行显示的块状元素**

inline-block 要比 block 在应用方面更加广泛，因为我们更多的时候需要一个能和其他元素共存的盒子，

关于 inline-block 这个值：

![image-20250604232102286](C:\Users\15956\AppData\Roaming\Typora\typora-user-images\image-20250604232102286.png)

这两个盒子之间多了一个空白，但是我们没有设置 margin 的值，其原因就是我们在两个 div 之间打了一个回车，在 html 中，回车会被当成文字解析

#### **解决办法：**

1. **去除回车**

```html
<div class="box1"></div><div class="box2"></div>
```

```css
div {
    width: 200px;
    height: 50px;
    display: inline-block;
}

.box1 {
    background-color: #EEEEEE;
}

.box2 {
    background-color: #XXXXXX;
}
```

![image-20250604232550565](C:\Users\15956\AppData\Roaming\Typora\typora-user-images\image-20250604232550565.png)

2. **给父类元素添加 word-spacing 属性**

word-spacing 属性就是单词与单词之间的距离，这里将这个距离调成负数就可以了值尽量小，一般小于-20；

```html
<div class="father">
    <div class="son1"></div>
    <div class="son2"></div>
</div>
```

```css
.father {
    word-spaceing: -30px;
}

.son1 {
    wodth: 20px;
    height: 50px;
    display: inline-block;
    background-color: #EEEEEE;
}

.son2 {
    width: 20px;
    height: 30px;
    display: inline-block;
    backgroud-color: #AAAAAA;
}
```

![image-20250604233048861](C:\Users\15956\AppData\Roaming\Typora\typora-user-images\image-20250604233048861.png)

3. **给父类元素设置 font-size：0px;**

因为 回车 被设置成了文字，那么我们将 font-size：设置成 0 就可以了。 

## position - static（默认定位）:

对于 CSS 中的关键元素 —— positon ，这个属性在 CSS 用与定位 DOM 元素，修改 DOM 元素布局。

对于一个代码，你看看：

```html
<!DOCTYPE html>
<head>
    <link rel="stylesheet" type="text/css" href="./index.css"/>
    <title> aaa </title>
</head>
<body>
    <h1 class="title">
        MOUNTAIN
    </h1>
    <p>
        The Facebook post was heartfelt. We like our little town just as it is:
    Little. Homey. Just us’ns.
    </p>
    <div class="img-box">
        <img
          alt=""
          class="first"
          src="https://document.youkeda.com/P3-1-HTML-CSS/1.8/1.jpg?x-oss-process=image/resize,h_300"
        />
        <img 
          alt=""
          src="https://document.youkeda.com/P3-1-HTML-CSS/1.8/2.jpg?x-oss-process=image/resize,h_300"
        />
        <img
          alt=""
          src="https://document.youkeda.com/P3-1-HTML-CSS/1.8/3.jpg?x-oss-process=image/resize,h_300"
         />
        <img
          alt=""
          src="https://document.youkeda.com/P3-1-HTML-CSS/1.8/4.jpg?x-oss-process=image/resize,h_300"
         />
    </div>
    <h2> LISTEN </h2>
    <p>
        Listen, I can empathize. As someone who’s lived in the Denver area since
    1971 — right about the time John Denver’s songs were enticing folks to move
    </p>
    <footer></footer>
</body>
```

```css
body {
    margin: 0;
    font-family: Sans-serif;
    color: rgba(0,0,0,0.84);
    font-size: 16px;
    padding: 30px;
}

img {
    width: 100%;
}
```

> 注意：img 默认会根据图片自身宽度展示，因此我们给他设置 width：100%，撑满左右

浏览器会自动给所有的 DOM 元素添加 position： static ；属性

position 除了 static 属性外，还有 4 个常用的值，分别为：

* relative (相对位置)
* absolute（绝对位置）
* fixed （固定位置）
* sticky （粘性定位）

## position - relative (相对定位)：

如果想让：

![image-20250605213442764](C:\Users\15956\AppData\Roaming\Typora\typora-user-images\image-20250605213442764.png)

这样的话：

如果用 margin 边距属性，那么会出现这样的情况：

```css
.first {
    margin-left: 50px;
    margin-top: 50px;
}
```

![image-20250605213657801](C:\Users\15956\AppData\Roaming\Typora\typora-user-images\image-20250605213657801.png)

结果会和我我们想的不一样，第一张图片确实动了，但是文档下部分也同样网下移动了 50 px; 也就是 margin 会引起文档流的变化。

这里就可以利用 left ， top 来实现想要的结果。在上一节，我们知道 position：static 下不能使用 left，top，right，bottom 属性，如果我们想在当前位置进行偏移，同时不影响整体页面布局，可以使用 relatve；

```css
.first {
    position: relative;
    left: 50px;
    top: 50px;
}
```

> relative 先遵循默认的文档流布局，也就是说 static 布局，然后再在不改变页面布局的前提下根据 left，right，top， bottom 调整元素布局

## position - absolute（绝对定位）：

1. 相对定位没有偏离文档流，而绝对定位偏离了文档流
2. 相对定位是根据原先位置进行参考，而绝对定位根据网页的左上角进行定位

区别：

```css
.first {
    position: absolute;
    left: 50px;
    top: 50px;
}
```

![image-20250605221540191](C:\Users\15956\AppData\Roaming\Typora\typora-user-images\image-20250605221540191.png)

![image-20250605221557215](C:\Users\15956\AppData\Roaming\Typora\typora-user-images\image-20250605221557215.png)

> absolute 被称为 **绝对定位** 是不为元素预留空间的，通过指定元素相对于最近的非 static 定位的祖先元素的偏移，来确定元素位置

## position - fixed (固定定位)：

在很多场景下，文章的标题会一直停留在浏览器的顶部，不会随着页面的滚动而消失，比如![image-20250608132413956](C:\Users\15956\AppData\Roaming\Typora\typora-user-images\image-20250608132413956.png)

无法动态展示，自行脑补

MOUNTAIN 这个单词会随着页面而滚动，一直停留在顶部50px处，这就是 fixed 的功能，英文翻译（固定）

我们给 H1 标签添加 fixed 属性。

```css
h1 {
    position: fixed;
    left: 30px;
    top: 50px;
}
```

让我们来get一段：

```css
body {
    margin: 0px;
    font-family: Sams-serif;
    color: rgba(0, 0, 0, 0.84);
    font-size: 16px;
    padding: 30px;
}

img {
    width: 100%;
}

h1 {
    position: fixed;

    padding: 0px;
    margin: 0px;
}

.img-body {
    position: relative;
}
```

发现，无论我们怎么滚动，MOUNTAIN 永远在窗口的左上角。

> fixed 为**固定定位**
> 
> 固定定位和绝对定位类似，但是元素的包含块为屏幕视口
> 
> 固定定位不为元素预留空间，而是通过指定元素相对于屏幕视口的位置来指定元素位置，元素的位置在屏幕滚动时不会改变

在滚动时，会有固定标签内容被其他模块遮挡的情况：![img](http://document.youkeda.com/P3-1-HTML-CSS/1.8/4-fixed/2.jpeg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

着需要 **z-index** 来解决

#### z-index：

**absolute，fixed** 模块 与其他模块共同处于 HTML 中，对于模块的有限级，由 **z-index** 来决定

> 1. 默认非 static 元素的 z-index 为0
> 2. z-index 越大，则越在最上面
> 3. 同样的 z-index ，在 HTML 的位置越靠后越在上面

为了让 MUNTAIN 在上面，我们只要修改 h1 标签的 z-index 就行了

```css
h1 {
    position: fixed;
    left: 30px;
    top: 30px;
    z-index: 1;
    color: yellowgrenn;
}
```

## Position-sticky:

![image-20250608145347039](C:\Users\15956\AppData\Roaming\Typora\typora-user-images\image-20250608145347039.png)

我们用这一张图，来演示 sticky：

注意：MOUNTAIN 的效果，在它滚动到顶部的时候，黏在了顶部，而当前页面下滑的时候， MOUNTAIN 又恢复其在文档中的位置，这种效果是 sticky

```css
h1 {
    position: sticky;
    color: yellowgreen;
    top: 50px;
    z-index: 1;
}
```

sticky 是一个新标签，有很多应用场景：![img](https://document.youkeda.com/P3-1-HTML-CSS/1.8/6-sticky/2.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

![img](https://document.youkeda.com/P3-1-HTML-CSS/1.8/6-sticky/3.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

![img](https://document.youkeda.com/P3-1-HTML-CSS/1.8/6-sticky/4.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

## Float：

**float ** 是CSS中最常用的布局标签，使用它可以让元素靠左或者靠右排版，

![image-20250608145924990](C:\Users\15956\AppData\Roaming\Typora\typora-user-images\image-20250608145924990.png)

使用这个标签，上面的导航栏会跟着滚轮一起滑动

我们写一下导航栏的代码：

```html
<!DOTYPE html>
<html>
    <head>
        <title>sss</title>
        <link rel="stylesheet" type="text/scc" href="./index.css"/>
    </head>
    <body>
        <nav></nav>
        <main>
            <img
                 src= src="https://document.youkeda.com/P3-1-HTML-CSS/1.8/1.jpg?x-oss-process=image/resize,h_500"
      />
      <img
        src="https://document.youkeda.com/P3-1-HTML-CSS/1.8/2.jpg?x-oss-process=image/resize,h_500"
      />
      <img
        src="https://document.youkeda.com/P3-1-HTML-CSS/1.8/3.jpg?x-oss-process=image/resize,h_500"
      />
      <img
        src="https://document.youkeda.com/P3-1-HTML-CSS/1.8/4.jpg?x-oss-process=image/resize,h_500"
      />
        </main>
    </body>
</html>
```

然后用 fixed 将头部导航栏 fixed 住

```css
body {
    padding: 0;
    margin: 0;
    background-color: #f5f5f5;
}

nav {
    position: fixed;
    width: 100%;
    height: 68px;
    border: 1px solid #f4f4f4;
    background-color: #fff;
}

img {
   width: 100%;
}

main {
    padding-top: 68px;
}
```

因为 nav 的 fixed 的属性会导致 main 导航栏脱离文档流，使得下方图片被遮挡，因此需要设置 padding-top 属性来解决遮挡问题。

 ![image-20250608151507363](C:\Users\15956\AppData\Roaming\Typora\typora-user-images\image-20250608151507363.png)

头部导航栏的实现：

补全 html 代码：

```html
<nsv class="nav">
    <img
         class="logo"
         src="https://style.youkeda.com/img/ykd-components/logo.png"
     />
    <img
         class="avatar"
         src="https://thirdqq.qlogo.cn/g?b=oidb&k=xnT9D0hzSGjSOOZkzqoutA&s=100&t=1555898643"
         />
</nsv>
```

在导航栏内部加入图片，

```css
.log {
    width: 100px;
    height: 36px;
}

.avater {
    width: 36px;
    height: 36px;
}
```

![image-20250608151900032](C:\Users\15956\AppData\Roaming\Typora\typora-user-images\image-20250608151900032.png)

已经创建了头部元素，怎么使得ykd图标靠左，头像靠右？

**float**——中文意思为 **浮动** 其有两个基本属性：

* left

* right
  
  ```css
  .logo {
     float: left;
  }
  
  .avater {
     float: right;
  }
  ```

![image-20250608152136976](C:\Users\15956\AppData\Roaming\Typora\typora-user-images\image-20250608152136976.png)

## 模态框：

来看一下模态框的例子![image.png](https://document.youkeda.com/P3-1-HTML-CSS/1.9/combat-1-modal/1.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)![image.png](https://document.youkeda.com/P3-1-HTML-CSS/1.9/combat-1-modal/2.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)![image.png](https://document.youkeda.com/P3-1-HTML-CSS/1.9/combat-1-modal/3.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

> 1. 模态框总是在浏览器的中间，浏览器随意的放大缩小，模态框还是在浏览器中间，
> 2. 模态框总是有一个半透明的背景

目标：

我们继续开发在 float 案例，建立一个模态框

![img](https://document.youkeda.com/P3-1-HTML-CSS/1.9/combat-1-modal/4.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

之前的代码：

```html
<!DOTYPE html>
<head>
    <link rel="stylesheet" type="text/css" href=".index.css" />
</head>
<body>
    <nav class="nav">
        <img
      class="logo"
      src="https://style.youkeda.com/img/ykd-components/logo.png"
    />
    <img
      class="avatar"
      src="https://thirdqq.qlogo.cn/g?b=oidb&k=xnT9D0hzSGjSOOZkzqoutA&s=100&t=1555898643"
    />
    </nav>
    <main>
        <img
      src="https://document.youkeda.com/P3-1-HTML-CSS/1.8/1.jpg?x-oss-process=image/resize,h_500"
    />
    <img
      src="https://document.youkeda.com/P3-1-HTML-CSS/1.8/2.jpg?x-oss-process=image/resize,h_500"
    />
    <img
      src="https://document.youkeda.com/P3-1-HTML-CSS/1.8/3.jpg?x-oss-process=image/resize,h_500"
    />
    <img
      src="https://document.youkeda.com/P3-1-HTML-CSS/1.8/4.jpg?x-oss-process=image/resize,h_500"
    />
    </main>
</body>
```

```css
body {
    padding: 0;
    margin: 0;
    background-color: #f5f5f5;
}

nav {
    position: fixed;
    width: 100%;
    height: 64px;
    border:10px solid #f4f4f4;
    background-color: #fff
}

img {
    width: 100%;
}

main {
    padding-top: 68px;
}

.logo {
    float: left;
    width: 100px;
    height: 36px;
    margin-left: 30px;
    margin-top: 16px;
}

.avater {
    float: right;
    width: 36px;
    height: 36px;
    margin-right: 30px;
    margin-top: 17px;
    border-radius: 50%;
}
```

### 第一步：完成半透明背景：

整个半透明背景铺满整个浏览器，覆盖在网页上面，我们需要设置一个 fixed 的容器放在 BODY 的最下面，如下面的 HTML 代码中的 《div class=“mask”》《/div》

```html
<!DOTYPE html>
<head>
    <link rel="stylesheet" type="text/css" herf="./index.css"/>
</head>
<body>
    <nav class="nav">
    </nav>
    <main>
    </main>
    <div class="mask">
    </div>
</body>
```

```css
.mask{
    position：fixed;
    left:0;
    right:0;
    top:0;
    bottom:0;
    background-color:rgba(0,0,0,0.7)
}
```

> 1. 通过 fixed ，上下左右都是0，设置 mask 是盛满在整个屏幕的
> 
> 2. background-color: 我们用 rgba 设置颜色调节透明度为 

![image-20250608211324499](C:\Users\15956\AppData\Roaming\Typora\typora-user-images\image-20250608211324499.png)

### 第二步：完成模态框：

![image-20250608211348774](C:\Users\15956\AppData\Roaming\Typora\typora-user-images\image-20250608211348774.png)

```html
<!DOCTYPE html>
<head>
  <link rel="stylesheet" type="text/css" href="./index.css" />
</head>
<body>
  <nav class="nav">
    ……
  </nav>
  <main>
    ……
  </main>
  <div class="mask"></div>
  <div class="modal">
    <img src="https://style.youkeda.com/img/ykd-components/logo.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10" />
  </div>
</body>
```

```css
.modal {
  background-color: #fff;

  /* 设置长宽 */
  width: 300px;
  height: 150px;

  /* 设置圆角20px */
  border-radius: 20px;
}

.modal > img {
  display: block;
  width: 200px;
  margin: 39px auto; /*1*/
}
```

### 元素的水平垂直居中：

#### 元素水平居中：

1. 如果是行内元素的话，我们可以在父辈元素上使用 text-align: center
2. 如果是内部元素的话，我们可以在子容器上使用 margin：0 auto（如果中国元素不是块状元素，那么我们设置 display: block）

#### 元素垂直居中：

之后会学到 flex 实现元素的垂直居中，这里可以使用 margin

### 第三步：完成模态框布局：

我们需要把整个中间的区域显示在浏览器中间，肯定设置为 position：fixed;（固定位置）

```css
.modal {
    position: fixed;
    left: 50%;
    top: 50%;

    margin-left: -150px;
    margin-top: -75px;

    background-color: #fff;
    width: 300px;
    height: 150px;
    border-radius: 20px;
}

.model > img {
    display: block;
    width: 200px;
    margin: 39px auto;
}  /* .model > img 是子选择器，这个css 只会匹配 .model 里面的 img 标签 */
```

> 对于这个位置的调整，我们需要知道，那个 left: 50% 与 top: 50%; 其实是标签的左上角的位置

![image-20250609130536700](C:\Users\15956\AppData\Roaming\Typora\typora-user-images\image-20250609130536700.png)

我们需要用 margin 元素边距属性进行修改

```css
    margin-left: -150px; /*1*/
    margin-top: -75px; /*2*/
```

## 搜索框：

很多网站头步都有一个搜索框，

![image-20250609145652078](C:\Users\15956\AppData\Roaming\Typora\typora-user-images\image-20250609145652078.png)

注意图片里面红色部分，，这部分由两个组件构成，一个搜索框，一个搜索结果列表

在第二节，不含模态框的 HTML 代码如下：

```html
<!DOTYPE html>
<head>
    <meta charset="UTF-8" />
</head>
<body>
    <nav class="nav">
    <img
      class="avatar"
      src="https://thirdqq.qlogo.cn/g?b=oidb&k=xnT9D0hzSGjSOOZkzqoutA&s=100&t=1555898643"
    />
  </nav>
  <main>
    <img
      src="https://document.youkeda.com/P3-1-HTML-CSS/1.8/1.jpg?x-oss-process=image/resize,h_500"
    />
    <img
      src="https://document.youkeda.com/P3-1-HTML-CSS/1.8/2.jpg?x-oss-process=image/resize,h_500"
    />
    <img
      src="https://document.youkeda.com/P3-1-HTML-CSS/1.8/3.jpg?x-oss-process=image/resize,h_500"
    />
    <img
      src="https://document.youkeda.com/P3-1-HTML-CSS/1.8/4.jpg?x-oss-process=image/resize,h_500"
    />
  </main>


</body>
```

```css
body {
  padding: 0;
  margin: 0;
  background-color: #f5f5f5;
}

nav {
  position: fixed;
  width: 100%;
  height: 68px;
  border: 1px solid #f4f4f4;
  background-color: #fff;
}

.logo {
  width: 100px;
  height: 36px;
  margin-top: 16px;
  margin-left: 30px;
  float: left;
}

.avatar {
  height: 34px;
  width: 34px;
  margin-top: 17px;
  border-radius: 50%;
  margin-right: 30px;
  float: right;
}
```

### 第一步：完成搜索框：

![img](https://document.youkeda.com/P3-1-HTML-CSS/1.9/combat-2-search/2.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

在效果图中，ykd 靠左，搜索框和头像靠右，通过第二节的学习，我们知道可以用 float 来实现。在这里，我们需要把右侧的输入框和头像整体包裹起来，实现靠右效果，我们来看 HTML 代码：

```html
<nav class="nav">
    <div class="right">
        <div class="serach">
            <input placeholder="搜你想搜的东西" />
            <img
                 src="///style.youkeda.com/img/ykd-components/search.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10"
      />"
        </div>
        <img
             class='avatar'
             srec="https://thirdqq.qlogo.cn/g?b=oidb&k=xnT9D0hzSGjSOOZkzqoutA&s=100&t=1555898643"
    </div>
</nav>
```

我们通过 <div class="right"> 容器包裹 <div class="serach"> 和 <div class="right"> 靠右，那其所有的内容都会跟着靠右。我们田间了一些 CSS 样式：

```css
.right {
    float: right:
}

.search {
    float: left;
    margin-right: 20px;
    margin_top: 16px;
}

.search > input {
    width: 220px;
    height: 36px;

    font-size: 12px;
    box-sizing: border-box;
    padding:0 50px 0 15px;
    background-color: #ededed;
    border-radius: 18px;

    border: none;
    outline: none;
}

.search > img {
    width: 34px;
    height: 34px;
}
```

对于这段代码的解释：

* margin-top: 16px 是计算出来的，（nav高度  -  input高度）/ 2 ， 和模态框一样
* box-sizing 在同时设置 width ， height ，padding 的时候一般会使用，防止宽度和高度 异常
* 去掉默认的 input 效果
* ">" CSS 代表直接子元素

![image-20250609152009634](C:\Users\15956\AppData\Roaming\Typora\typora-user-images\image-20250609152009634.png)

可以看见 **搜索图标** 还占用页面位置，我们希望其能往左下移动到 Input 框里面。我们还可以用 position：absolute 让其脱离文档流

我们个给 .search > img 元素添加定位属性，需要设置父辈元素 position：relative

```css
.search {
    position: relative;
}

.serach > img {
    position: absolute;
    right: 10px;
    top: 10px;

    width: 34px;
    height: 34px;
}
```

![img](https://document.youkeda.com/P3-1-HTML-CSS/1.9/combat-2-search/4.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

* 搜索结果宽度和头部搜索框的宽度是一致的
* 搜索结果脱离文档流，是浮在所有元素上面的

我们在原来的 search 元素中加入搜索结果：

```html
......
<nav class="nav">
    <div class="right">
        <div class="search">
            <input placeholder="搜你想搜的">
            <img 
                 src=""
            />
            <ul class="search-result">
                <li>XXX</li>
                <li>XXX</li>
            </ul>
        </div>
        <img
             class="avatar"
             src=""
    </div>
</nav>
```

注意 HTML 元素含义 search-result 属于搜索框内容，所以和输入框，搜索图标，一起包裹在 search 里面

```css
.search-result {
    position: absolute;
    left: 0;
    top: 60px;

    padding: 0 15px;

    background-color: #fff;
    border-radius: 5px;

    box-shadow: 0px 1px 11px 0px rgba(0, 0, 0, 0.16);
}

.rearch-result {
    font-size: 12px;
    color: #1f2c41;

    height: 36px;
    line-height: 36px;

    width: 190px;
    boder-bottom: 1px solid #f3f3f3;
}
```

**重点的解释一下标注 1， 2， 3**

1. 因为要脱离文档流, 并且是相对于 search 父元素进行定位，因此使用绝对布局。
2. 这个 CSS 的新属性—— 阴影，合适的阴影让元素具有层次感 **MDN 阴影**
3. 为了使得文字在每个 li 元素中垂直居中，我们使用 line-height = height 进行解决。

最后我们加入 ul li 默认样式

```css
ul {
    margin: 0;
    padding: 0;
}

li {
    margin: 0;
    padding: 0;
    list-style: none:
}
```

## 颜色背景：

### 渐变色：

设置颜色的几种方法：

```css
background: red;
background: #ffffff;
background: rgba(200, 200, 200);
background: rgba(0, 0, 0, 0.5);
```

这些颜色都是纯色，这里我们学习一些高级技巧，利用 background 来设置渐变色。

eg：

![img](http://document.youkeda.com/P3-1-HTML-CSS/1.10/1-gradient/1.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

```css
左边的色值:   #95CA47
右边的色值为:  #4DC891
```

来实现这个按钮：

```html
<!DOCTYPE html>
<head>
    <meta charset="utf-8"/>
    <link rel="stylesheet" type="text/css" herf="./index.css" />
    <title>XXX</title>
</head>
<body>
    <button class="publish">
        傻逼
    </button>
</body>
```

```css
.publish {
    width: 100%;
    height: 50px;
    line-height: 50px;

    color: #fff;
    font-size: 18px;
    border-radius: 4px;
    font-weight: 500;
    box-shadow: 0 2px 6px 0 rgba(104, 200, 116, 0.3);
}
```

![img](http://document.youkeda.com/P3-1-HTML-CSS/1.10/1-gradient/2.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

在图中，我们看见已近有了**边框，阴影，圆角**效果，文字因为是白色所有看不出来，下面我们需要给按钮加入渐变背景色

现在给 background 配上新标签：

### linear-gradient：

![image-20250622144914026](C:\Users\15956\AppData\Roaming\Typora\typora-user-images\image-20250622144914026.png)

根据上图，我们设置好**渐变类型， 渐变方向， 开始颜色， 结束颜色**即可实现简单的渐变效果。

### 渐变方向：

渐变方向使用的语义化英文实现，具体由如下值：

* to right / to left   向右 / 向左 渐变

* to top / to bottom    向上 / 向下渐变

* to left bottom / to left top    向左下 / 向右上渐变

* xxxdeg   xxx范围 （0 到 360）   更加精准的渐变方向

### 渐变位置：

渐变不一定是从开始到结束，我们可以设置各种中间状态。例如从 30% - 70% 的渐变

![img](http://document.youkeda.com/P3-1-HTML-CSS/1.10/1-gradient/4.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

使用：

```css
background: linear-gradient(to right, xxxx 30%, xxx 70%);
```

我们可以在每个色值后面跟一个值 **百分比**，**px**，来约定变色起止位置。

## 背景图片：

css 除了设置背景颜色，还可以设置背景图片，背景图片在网页中使用十分常见。我们来看一下网上一些背景图片使用案例。![image-20250630125635091](C:\Users\15956\AppData\Roaming\Typora\typora-user-images\image-20250630125635091.png)

![image-20250630125645533](C:\Users\15956\AppData\Roaming\Typora\typora-user-images\image-20250630125645533.png)

![image-20250630125702272](C:\Users\15956\AppData\Roaming\Typora\typora-user-images\image-20250630125702272.png)

### 基础使用：

我们现在来学习背景图片的使用方法，先看看我们的目标，实现如下简单的效果

![image-20250630125857224](C:\Users\15956\AppData\Roaming\Typora\typora-user-images\image-20250630125857224.png)

> 一个容器中，有一串 HELLO WORLD 文字，同时有一张居中的 ykd LOGO 的背景。

我们继续来看下面的 HTML 代码

```html
<!DOCTYPE html>
<head>
  <meta charset="utf-8" />
  <link rel="stylesheet" type="text/css" herf="./index.css" />
  <title>sss</title>
</head>
<body>
  <div class="box">HELLO WORLD</div>
</body>
```

```css
.box {
    width: 100%;
    height: 250px;
    border: 1px solid #e8e8e8;
    font-size: 30px;
    font-weight: bold;
    color: yellowgreen;
}
```

下面我们需要给 box 元素设置背景图片：

```css
{
    background-img: url(XXXXXXX)
}
```

> 主要 url  里面的图片地址**不需要用引号包裹**

### 问题：

#### 背景图出现了重复：

当背景图片长宽任意一项小于容器的长宽，默认 CSS 会让图片重复，直到铺满整个容器位置。

我们可以使用 background-repeat: no-repeat; 禁止图片的重复。

下面学习background-repeat的值

### background-repeat 的值：

| 值         | 描述                                 |
|:---------:|:----------------------------------:|
| repeat    | 这是**默认值**，如果背景图片比容器小，将在垂直和水平方向进行重复 |
| repeat-x  | 背景图片只在**水平方向**重复                   |
| repeat-y  | 背景图片只在**垂直方向**重复                   |
| no-repeat | 背景图片将只显示一次，**不重复**                 |

### 背景图片不居中：

默认情况下，背景图片是从容器的左上角开始布局，为了使容器垂直水平居中，我们可以使用，background-position：center; 来解决，完整代码：

```css
.box {
    width: 350px;
    height: 250px;
    border: 1px solid #e8e8e8;
    font-size: 30px;
    font-weight: bold;
    color: yellowgreen;

    background-image: url(https://style.youkeda.com/img/ykd-components/logo.png)
    background-repeat: no-repeat;
    background-position: center;
}
```

我们来完整的学习一下 background-position 的值。

![image-20250630140626758](C:\Users\15956\AppData\Roaming\Typora\typora-user-images\image-20250630140626758.png)

### 高级特性：

#### 背景图片撑满整个容器:

在上面的基础上，我们希望背景图片放大撑满整个容器，如下图所示：

![image-20250630141304580](C:\Users\15956\AppData\Roaming\Typora\typora-user-images\image-20250630141304580.png)

我们需要认识一个新属性 **background-size** ，用这个属性可以设置背景图片的大小，我们直接来看看这个属性的值。

| 值       | 描述                                                |
|:-------:|:-------------------------------------------------:|
| cover   | 把背景图片扩展到足够大，使得背景完全覆盖区域。但是背景图片的某些部分也许无法显示在背景定位区域里面 |
| contain | 把图像扩展至最大尺寸，使得其宽度和高度完全适应内容区域                       |
| xpx ypx | 手动设置长宽                                            |
| x% y%   | 手动设置宽度和高度相对于容器的百分比                                |

### background 合并写法：

```css
.box {
    width: 350px;
    height: 250px;
    border: 1px solid #e8e8e8;
    font-size: 30px;
    font-weight: bold;
    color: yellowgreen;

    background-image: url(XXXXXXXXXXXXXXXXXXXXXXX);
    background-repeat: no-repeat;
    background-size: contain;
    background-position: center;
}
```

但是对于background属于有一种非常简单的写法：

```css
{
    background: [background-color] [background-image] [background-repeat] [background-position] / [background-size] [background-clip];
}
```

比如：

```css
{
    background: url(XXXXX) no-repeat center / contain;
}
```

### 其他属性：

position-attachment:  https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-attachment

position-clip:  https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-clip

## qq注册页：

对于 qq 注册页的头部开发：![img](https://document.youkeda.com/P3-1-HTML-CSS/1.11/demo.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)
