# CSS | Position

## 简述

共有五种属性，其中 `static` 是默认值

```css
.box {
    position: static | relative | absolute | fixed | sticky
}
```

## static

`static` 是 `position` 属性的默认值

浏览器会按照源码的顺序，决定每个元素的位置，这称为"正常的页面流"（normal flow）。每个块级元素占据自己的区块（block），元素与元素之间不产生重叠，这个位置就是元素的默认位置。

:warning:此时 `top`、`bottom`、`left`、`right`、`z-index` 属性无效。

## relative, absolute, fixed

这三个属性值有一个共同点，都是相对于某个基点的定位，不同之处仅仅在于基点不同

### relative

表示相对于默认位置（即`static`时的位置）进行偏移，换句话说**，定位基点是元素的默认位置**

![](https://cjpark-1304138896.cos.ap-guangzhou.myqcloud.com/note_img/20211214113303.png)

它必须搭配 `top`、`bottom`、`left`、`right` 这四个属性一起使用，用来指定偏移的方向和距离

![](https://cjpark-1304138896.cos.ap-guangzhou.myqcloud.com/note_img/20211214113342.png)

### absolute

表示相对于距离最近的有定位属性（一般是 relative）的上级元素（一般是父元素）进行偏移，即**定位基点是上级元素**

`absolute` 定位也必须搭配 `top`、`bottom`、`left`、`right` 这四个属性一起使用。

> **MDN 官方说法：**
>
> 元素会被移出正常文档流，并不为元素预留空间，通过指定元素相对于最近的非 static 定位祖先元素的偏移，来确定元素位置。

![](https://cjpark-1304138896.cos.ap-guangzhou.myqcloud.com/note_img/20211214113926.png)

```css
/*父元素设置成relative，子元素用absolute，才能使子元素的定位基点为父元素*/
#dad {
    position: relative;
}
#son {
    position: absolute;
    top: 20px;
}
```

### fixed

`fixed` 表示，相对于视口（viewport，浏览器窗口）进行偏移，即**定位基点是浏览器窗口**。这会导致元素的位置**不随页面滚动而变化**，好像固定在网页上一样。

元素会被移出正常文档流

![](https://cjpark-1304138896.cos.ap-guangzhou.myqcloud.com/note_img/20211214114137.png)

它如果搭配 `top`、`bottom`、`left`、`right` 这四个属性一起使用，表示元素的初始位置是基于视口计算的，否则初始位置就是元素的默认位置。

换言之，如果设置了 `position: fixed` 但却不设置 `top`、`bottom`、`left`、`right`，相当于什么都没干，元素还是会处于文档流的正常位置

## sticky

**一言以蔽之：类似淘宝顶部搜索栏的效果**

`sticky` 跟前面四个属性值都不一样，它会产生动态效果，很像 `relative` 和 `fixed`的结合：一些时候是 `relative` 定位（定位基点是自身默认位置），另一些时候自动变成 `fixed` 定位（定位基点是视口）。

因此，它能够形成"动态固定"的效果。**比如网页的搜索工具栏**，初始加载时在自己的默认位置（`relative`定位）。

## 总结

|                  | 定位基点                     | 搭配属性                                                     | 是否移出文档流 | 常见应用                             |
| ---------------- | ---------------------------- | ------------------------------------------------------------ | -------------- | ------------------------------------ |
| static (default) | 文档流正常位置               | `top`、`bottom`、`left`、`right`、`z-index` 属性无效         | 否             |                                      |
| relative         | 元素原 static 位置           | 需搭配`top`、`bottom`、`left`、`right`，否则与 static 无区别 | 否             |                                      |
| absolute         | 距离最近的非 static 上级元素 | 需搭配`top`、`bottom`、`left`、`right`                       | 是             |                                      |
| fixed            | 浏览器窗口                   | 需搭配`top`、`bottom`、`left`、`right`，否则与 static 无区别 | 是             | 网站右下角客服咨询窗                 |
| sticky           |                              |                                                              |                | 淘宝商品搜索栏，大型表格滑动固定表头 |

