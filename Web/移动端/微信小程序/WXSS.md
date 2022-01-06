`/pages/{pageName}/index.wxss` 页面样式，应用到同目录下的 `index.wxml`

`/app.wxss` 项目公共样式，会被注入到每个页面

## 官方 UI 组件库

https://github.com/Tencent/weui-wxss

## 尺寸

rpx (responsive pixel) 响应式像素，能够自动适配不同大小设备

小程序编译后，rpx会做一次px换算。换算是以375个物理像素为基准，也就是在一个宽度为375物理像素的屏幕下，1rpx = 1px。
$$
1 \, rpx = \frac{屏幕宽度}{物理像素数量} \cdot 1 \, px
$$

## 引用

`@import './test_0.wxss'` 

会被编译打包，不会产生多余的文件请求



## 内联样式

允许动态更新

```html
{
  eleColor: 'red',
  eleFontsize: '48rpx'
}
-->
<view style="color: {{eleColor}}; font-size: {{eleFontsize}}"></view>
```



## 选择器

| **类型**     | **选择器** | **样例**      | **样例描述**                                   |
| ------------ | ---------- | ------------- | ---------------------------------------------- |
| 类选择器     | .class     | .intro        | 选择所有拥有 class="intro" 的组件              |
| id选择器     | #id        | #firstname    | 选择拥有 id="firstname" 的组件                 |
| 元素选择器   | element    | view checkbox | 选择所有文档的 view 组件和所有的 checkbox 组件 |
| 伪元素选择器 | ::after    | view::after   | 在 view 组件后边插入内容                       |
| 伪元素选择器 | ::before   | view::before  | 在 view 组件前边插入内容                       |

优先级

![](https://cjpark-1304138896.cos.ap-guangzhou.myqcloud.com/note_img/20211201103431.png)