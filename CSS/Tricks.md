# CSS

## 技巧

### div 垂直居中的办法

```css
// 子元素
.loginCard {
  position: relative;
  top: 50%;
  transform: translateY(-60%);
}

// 父元素
.fullScreenHeight {
  height: 100%;
}
```

### 背景图无重复无拉伸自适应

```css
#logiBackground {
  background-image: url("../../public/bg3.jpg");
  background-attachment: fixed;
  background-size: cover;
  background-repeat: no-repeat;
}
```

