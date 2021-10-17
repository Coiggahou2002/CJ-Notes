# CSS | 基础知识

## 基本属性

大小

```css
width: ;
height: ;
```

### 颜色

#### 表示方法

+ 颜色英文单词 (pink, red, ...)
+ 十六进制色码 `#FF0000`
+ RGB 模式 `rgb(255, 255, 255)`
+ RGBA 模式 `rgba(red, green, blue, alpha)` (多一个透明度)

#### 相关属性

```css
color: 前景色（字体色）;
background-color: 背景颜色;
```

### 文本样式

#### 文字样式

可以直接套上 strong 标签来加粗文字，套 u 来加上下划线

```html
<strong>BOLD_TEXT</strong>
<u>UNDERLINED_TEXT</u>
<em>ITALIC_TEXT</em>  <!--斜体-->
<s>DELETED_TEXT</s>   <!--删除线-->

font-size
font-weight: 200-800;
line-height: 行高;
```

大小写转换可以用 `text-transform` 设置

+ lowercase
+ uppercase
+ capitalize 首字母大写
+ initial 使用默认值
+ inherit 继承父元素的 text-transform 值
+ none

#### 对齐

```css
text-align: justify;  /*贴紧两边*/
text-align: center;
text-align: right;
text-align: left;
```

### 分割

```html
<hr/> <!--水平分割线-->
```

### 边框

```css
border
```

### 阴影

box-shadow

+ offset-x 水平偏移
+ offset-y 垂直偏移
+ blur-radius 模糊半径
+ spread-radius 扩展半径
+ color 阴影颜色

```css
box-shadow: 0 10px 20px rgba(0,0,0,0.19), 0 6px 6px rgba(0,0,0,0.23);
/*Google Material 卡片阴影样式*/
```

### 透明度

元素透明度使用 `opacity` 设置

+ 1 完全不透明
+ 0.5 半透明
+ 0 完全透明

### 边距

一次性连着写四个，按顺时针算 (top, right, bottom, left)

```html
padding: 10px 20px 10px 20px;
margin
```

## 选择器

不仅仅可以根据id和class作选择，还可以自定义选择器

```css
[type='radio'] {
  margin: 20px 0px 20px 0px;
}
```

## 度量单位

### 绝对单位

```css
in: 英寸
mm: 毫米
px: 像素
```

### 相对单位

```css
em: 基于元素字体的字体大小
rem: 同上
```

## 优先级

important 标记优先级最高

```css
.pink-text {
    color: pink !important;
  }
```

内联样式（在 HTML 标签中直接设置）会覆盖 CSS 文件的样式

id 选择器的优先级总是高于 class 选择器（一定会覆盖 class 的样式）

对于 class 选择器之间：

+ HTML 元素中 class 里面设置的先后顺序不会影响样式优先级
+ CSS 文件里样式声明的先后顺序决定了渲染优先级，后声明的会覆盖前面的

## 变量

```css
/*前面加两个杠就可以创建变量*/
--penguin_skin: gray;

/*调用变量的值，要套一个var()，后面跟的是调取不到变量的默认值*/
background: var(--penguin_skin, black);

/*对付浏览器兼容性的办法，前面加一行传统声明*/
background: red;
background: var(--red-color);
```

**继承问题：**

![](https://cjpark-1304138896.cos.ap-guangzhou.myqcloud.com/note_img/20211017150224.png)

在后面如果声明了与 `:root` 选择器中同名的变量，会覆盖 `:root` 里声明的值

## 伪类

`a:hover` 悬停状态

## 盒模型

类型

+ 块级元素自动换行（h, p, div...）
+ 行内元素前后跟随

### 相对定位

下面代码表示：使用相对定位的方式，p 元素相对其底部向上偏移 10 px

```css
p {
	position: relative;
	bottom: 10px;
}
```

### 绝对定位

使用绝对定位会将元素从文档流移除

而且，**“绝对”是相对于其被包含块来说的，而不是相对于整个大页面**

```css
p {
	position: absolute;
	top: 10px;
	right: 10px;
}
```

### 相对窗口的绝对定位

`fixed` 将元素**相对于浏览器窗口**直接定位，最显著的特点是不会随滚动条移动.

```css
position: fixed;
```

## 浮动

### float

### z 层顺序

`z-index` 越大，显示在越上层

## 布局

在应用设计中经常需要把一个块级元素水平居中显示。 一种常见的实现方式是把块级元素的 `margin` 值设置为 auto。

同样的，这个方法也对图片奏效。 图片默认是内联元素，但是可以通过设置其 `display` 属性为 `block`来把它变成块级元素。

## 动画

CSS 属性 `transform` 里面的 `scale()` 函数可以用来改变元素的显示比例

接下来要介绍的 `transform` 属性是 `skewX()`：它使选择的元素沿着 X 轴（横向）翻转指定的角度。

下面的代码沿着 X 轴翻转段落元素 -32 度。

```
p {
  transform: skewX(-32deg);
}
```

### animation

为需要设置动画的对象添加 `animation-name`， `animation-duration` ，`animation-fill-mode` 属性，他们分别制定动画的名称、持续时间以及动画结束之后元素的样式.

`animation-iteration-count` 可以控制动画循环次数

然后再使用 `@keyframes` 设置关键帧

```css
#rect {
    animation-name: rainbow;
    animation-duration: 4s;

  }

  @keyframes rainbow {
    0% {
      background-color: blue;
    }
    50% {
      background-color: green;
    }
    100% {
      background-color: yellow;
    }
  }
```

在 CSS 动画里，`animation-timing-function` 用来定义动画的速度曲线。 速度曲线决定了动画从一套 CSS 样式变为另一套所用的时间。

默认值是 `ease`，动画以低速开始，然后加快，在结束前变慢。

其它常用的值

`ease-out`：动画以高速开始，以低速结束；

`ease-in`，动画以低速开始，以高速结束；

`linear`：动画从头到尾的速度是相同的。

## 媒体查询

下面是一个媒体查询的例子，当设备宽度小于或等于 `100px` 时返回内容：

```css
@media (max-width: 100px) { /* CSS Rules */ }
```

以下定义的媒体查询，是当设备高度大于或等于 `350px` 时返回内容：

```css
@media (min-height: 350px) { /* CSS Rules */ }
```

注意，只有当媒体类型与所使用的设备的类型匹配时，媒体查询中定义的 CSS 才生效。

### 自适应

用 CSS 来让图片自适应其实很简单。 你只需要给图片添加这些属性:

```css
img {
  max-width: 100%;
  height: auto;
}
```

设置 `max-width` 值为 `100%` 可确保图片不超出父容器的范围；设置 `height` 属性为 `auto` 可以保持图片的原始宽高比。

除了使用 `em` 或 `px` 设置文本大小，你还可以用视窗单位来做响应式排版。 视窗单位和百分比都是相对单位，但它们是基于不同的参照物。 视窗单位是相对于设备的视窗尺寸（宽度或高度），百分比是相对于父级元素的大小。

四个不同的视窗单位分别是：

+ `vw`：如 `10vw` 的意思是视窗宽度的 10%。
+ `vh：` 如 `3vh` 的意思是视窗高度的 3%。
+ `vmin：` 如 `70vmin` 的意思是视窗的高度和宽度中较小一个的 70%。
+ `vmax：` 如 `100vmax` 的意思是视窗的高度和宽度中较大一个的 100%。

## Flex 布局

References：

[Flex 布局教程：语法篇](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)

[Flex 布局教程：实例篇](http://www.ruanyifeng.com/blog/2015/07/flex-examples.html)

父容器设置 `display: flex`，子容器自动成为 `flex` 容器成员.

以下六个属性设置在容器上：

+ ```
  flex-direction
  ```

   控制排列顺序

  + row 行从左到右
  + row-reverse 行从右到左
  + column 列从上到下
  + column-reverse 列从下到上

+ ```
  flex-wrap
  ```

   控制排在一条轴线上的项目是否换行

  + nowrap 不换行
  + wrap 换行，新增元素放下一行
  + wrap-reverse 换行，新增元素放上一行

+ `flex-flow` 可以把上面两个属性写在一起

+ ```
  justify-content
  ```

   定义项目在主轴上的对齐方式

  + flex-start 左对齐
  + flex-end 右对齐
  + center 居中
  + space-between 两端贴墙壁，中间元素互相等距
  + space-around 两端不贴墙壁，项目间等距
  + space-evenly 两端不贴墙壁，项目间均匀

+ ```
  align-items
  ```

   定义项目在交叉轴上如何对齐

  + flex-start 起点对齐
  + flex-end 终点对齐
  + center 中点对齐
  + baseline 第一行文字基线对齐
  + stretch 占满高度

+ `align-content` 定义多根轴线的对齐方式

下面的属性设置在 flex 元素上：

+ `order` 定义项目的排列顺序，小数靠前
+ `flex-shrink` 定义空间不足需要缩小时，缩小比例的权重
+ `flex-grow` 定义有空间剩余时，占用剩余空间的权重
+ `flex-basis`
+ `flex`
+ `align-self` 允许单个项目有与其他项目不一样的对齐方式，可覆盖 align-items 属性