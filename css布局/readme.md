## 水平居中

### inline元素

父元素设置`text-align:center`使得inline元素水平居中

### 定宽块级元素

设置`margin:0 auto`使得定宽块级元素居中

### 不定宽块级元素

```css
#parent{
  position:relative;
}
#id{
  position:absolute;
  left:50%;
  transform:translateX(-50%);
}
```

使用定位和`transform`来解决居中

### flex

```css
#parent{
  display:flex;
  direction:column;
  justify-content:center;
}
```

## 垂直居中

### inline元素

行内元素没有高度，可以通过设置行高：`line-height`

### 块级元素

1. 通过定位和水平居中一样
2. flex，设置`align-items:center`

## 两栏布局，左定宽，右不定

### 圣杯布局（两栏版）

[demo](./demo1.html)

### 双飞翼布局（两栏版）

[demo](./demo2.html)

### 定位

[demo](./demo3.html)

### flex布局

[demo](./demo4.html)

## 三栏布局，三栏定宽

### flex布局

[demo](./demo5.html)

## 三栏布局，两边定宽，中间不定

和两栏布局类似:

* 圣杯
* 双飞翼
* 绝对定位
* flex布局