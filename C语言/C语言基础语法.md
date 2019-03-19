## C语言基本语法

[TOC]

## 一、html

### 1、关于闭合标签

HTML5没有特殊要求，可以加可以不加，比如

`<img src="" alt="">` or `<img src="" alt="" />`

### 2、属性

`<img src="" alt="">`

src(source)和alt(alternative)就是属性

### 3、复合元素

- `<h1>`这样的标题
- `<ul><li></li></ul>`这样的复合元素
- `<p><em></em></p>`这样的嵌套标签

### 4、HTML文档剖析

```html
# HTMl5语法编写的最简单HTML页面模板
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8" />
        <title>An HTML Template</title>
    </head>
    <body>
        <!-- 这里是网页内容 -->
    </body>
 </html>
```

### 5、文档对象模型

![](public/img/html+css/dom.jpg)

## 二、css

### 1、css工作原理

#### 1、css规则

![](public/img/html+css/rule.jpg)

#### 2、选择器

- 分组选择符 以`逗号`作为分隔符
- 上下文选择符 以`空格`作为分隔符
  - 子选择符 以`>`作为分隔符
  - 紧密兄弟选择符 以`+`作为分隔符
  - 一般兄弟选择符 以`~`作为分隔符
  - 通用选择符 以`*`作为分隔符
- id和class选择符
  - class 以`.`作为分隔符 **不唯一**
  - id 以`#`作为分隔符 **唯一**
- 属性选择符
  - 属性名 img[src]
  - 属性值 img[src=""]

#### 3、伪类

##### 1、链接伪类

```css

a:link {color: black;} //等着用户点击
a:visited {color: gray;} //用户点击过
a:hover {text-decoration: none;} //鼠标悬停
a:active {color: red;} //正在点击，鼠标没释放

```

- :focus
- :target

##### 2、结构化伪类

- :first-child
- :last-child
- :nth-child()

#### 4、伪元素

- ::first-letter
- ::first-line
- ::before
- ::after

#### 5、继承

#### 6、层叠

- 规则一：找到所有元素和属性的声明
- 规则二：按照顺序和权重 !important;
- 规则三：按元素优先级  **I-C-E**
- 规则四：顺序决定权重

#### 7、规则声明

- 文本值
- 数字值
- 颜色值
