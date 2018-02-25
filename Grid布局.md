# Grid布局

## 快速入门

Grid布局是一种二维网格布局。Grid 布局由两个核心组成部分是 **wrapper**（父元素）和 **items**（子元素）。 wrapper 是实际的 grid(网格)，items 是 grid(网格) 内的内容。

使用Grid布局非常简单，你只需要给容器（container）定义：display:grid，并设置列（grid-template-columns）和 行（grid-template-rows）的大小，然后用grid-column和grid-row定义容器子元素（item）的位置。

我们为 `grid-template-columns` 写入了 n 个值，我们就会得到 n 列。 

我们为 `grid-template-rows` 指定了 m 个值，我们就得到 了 m 行。

```css
.wrapper {
display: grid;
grid-template-columns: 200px 50px 100px;//3列
grid-template-rows: 100px 30px; //2行
}     
```
![Grid 布局的Columns(列) 和 rows(行)](http://newimg88.b0.upaiyun.com/newimg88/2017/12/1_M9WbiVEFcseUCW6qeG4lSQ.png))

要定位和调整 items(子元素) 大小，我们将使用 `grid-column` 和 `grid-row` 属性来设置

```css
.item1 {
    grid-column-start: 1;
    grid-column-end: 4;
}
```

如果你不明白我们设置的只有 3 列，为什么有4条网格线呢？看看下面这个图像

![](C:\Users\flame\Desktop\QQ图片20180202094407.png)

## Grid Container 属性

### display

### grid-template-columns

### grid-template-rows

### grid-template-areas

### grid-template

###grid-column-gap

###grid-row-gap

###grid-gap

###justify-items

###align-items

###justify-content

###align-content

###grid-auto-columns

###grid-auto-rows

###grid-auto-flow

###grid

## Grid Items 属性

### grid-column-start

###grid-column-end

###grid-row-start

###grid-row-end

###grid-column

###grid-row

###grid-area

###justify-self

###align-self