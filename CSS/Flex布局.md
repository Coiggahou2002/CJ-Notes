# CSS | Flex 布局

## 示意图

flex-row 行布局示意图

![](https://cjpark-1304138896.cos.ap-guangzhou.myqcloud.com/note_img/20211207170923.png)

flex-column 列布局示意图

![](https://cjpark-1304138896.cos.ap-guangzhou.myqcloud.com/note_img/20211207170934.png)

主轴对齐方式

![](https://cjpark-1304138896.cos.ap-guangzhou.myqcloud.com/note_img/20211214100601.png)

副轴对齐方式

![](https://cjpark-1304138896.cos.ap-guangzhou.myqcloud.com/note_img/20211214101144.png)

## 概念



## 基本属性

### 父容器属性

+ `display: flex` 指定为 flex 父容器
+ `flex-direction` 定义主轴
  + row 表示该容器子元素从左到右排（主轴为 x 轴，副轴为 y 轴）` 
  + column 表示该容器子元素从上到下排（主轴为 y 轴，副轴为 x 轴）
  + row-reverse
  + column-reverse
+ `flex-wrap`
  + wrap
  + nowrap
  + wrap-reverse
+ `justify-content` 主轴对齐方式
  + flex-start
  + flex-end
  + center
  + space-between **项目之间等间距**
  + space-around **每个项目两侧等间距**
+ `align-items` 副轴对齐方式
  + flex-start
  + flex-end
  + center
  + baseline 根据文字基线对齐
  + stretch 在副轴方向拉伸

### 子元素属性

+ `flex-grow` 放大比例，默认为 0
+ `flex-shrink`
+ `flex-basis` 初始占据空间，默认 auto，可以接受一般单位
+ `flex: <flex-grow> <flex-shrink> <flex-basis>`
+ `align-self` 定义元素自己的副轴对齐方式，会单独覆盖父元素的 `align-items` 设置

