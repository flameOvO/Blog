##  注释

`sass`支持//和/**/两种注释方法。

## 变量

SASS允许使用变量，所有变量以$开头。变量可以在`css`规则块定义之外存在。当变量定义在`css`规则块内，那么该变量只能在此规则块内使用。如果它们出现在任何形式的`{...}`块中（如`@media`或者`@font-face`块），情况也是如此：

```scss
　　$blue : #1875e7;　
　　div {
    	$width:100px;
    	width:$width;
    	color : $blue;
　　}

```

在声明变量时，变量值也可以引用其他变量。

```scss
$highlight-color: #F90;
$highlight-border: 1px solid $highlight-color;
.selected {
  border: $highlight-border;
}
//编译后
.selected {
  border: 1px solid #F90;
}
```

变量名中的下划线和中划线用法相互兼容。

```scss
$link-color: blue;
a {
  color: $link_color;
}

//编译后

a {
  color: blue;
}
```

## 嵌套CSS

```scss
#content {
  article {
    h1 { color: #333 }
    p { margin-bottom: 1.4em }
  }
  aside { background-color: #EEE }
}

 /* 编译后 */
#content article h1 { color: #333 }
#content article p { margin-bottom: 1.4em }
#content aside { background-color: #EEE }
```

####父选择器的标识符&;

可以为父级选择器添加`：hover`等伪类时

```scss
article a {
  color: blue;
  &:hover { color: red }
}
//编译后
article a { color: blue }
article a:hover { color: red }
```

可以在父选择器之前添加选择器。

```scss
#content aside {
  color: red;
  body.ie & { color: green }
}

/*编译后*/
#content aside {color: red};
body.ie #content aside { color: green }
```



#### 子组合选择器和同层组合选择器：>、+和~;

```scss
article {
  ~ article { border-top: 1px dashed #ccc }
  > footer { background: #eee }
  dl > {
    dt { color: #333 }
    dd { color: #555 }
  }
  nav + & { margin-top: 0 }
}
//编译后
article ~ article { border-top: 1px dashed #ccc }
article > footer { background: #eee }
article dl > dt { color: #333 }
article dl > dd { color: #555 }
nav + article { margin-top: 0 }
```

#### 嵌套属性

无需反复写`border-style``border-width``border-color`以及`border-*`等。在`sass`中，你只需敲写一遍`border`：

```scss
nav {
  border: 1px solid #ccc {
  left: 0px;
  right: 0px;
  }
}
nav {
  border: {
  style: solid;
  width: 1px;
  color: #ccc;
  }
}
//编译后
nav {
  border: 1px solid #ccc;
  border-left: 0px;
  border-right: 0px;
}
nav {
  border-style: solid;
  border-width: 1px;
  border-color: #ccc;
}
```

## 导入SASS文件

`css`有一个特别不常用的特性，即`@import`规则，它允许在一个`css`文件中导入其他`css`文件。然而，后果是只有**执行到**`@import`时，浏览器才会去下载其他`css`文件，这导致页面加载起来特别慢。

`sass`的`@import`规则在**生成**`css`文件时就把相关文件导入进来。这意味着所有相关的样式被归纳到了同一个`css`文件中，而无需发起额外的下载请求。

使用`sass`的`@import`规则可以省略`.sass`或`.scss`文件后缀

#### 使用SASS部分文件

那些专门为`@import`命令而编写的`sass`文件，并不需要生成对应的独立`css`文件，这样的`sass`文件称为**`局部文件`**。对此，`sass`有一个特殊的约定来命名这些文件。此约定即，`sass`局部文件的文件名以下划线开头。



#### 默认变量！default

` !deafault`的含义：如果这个变量被声明赋值了，那就用它声明的值，否则就用这个默认值。

在下例中，如果用户在导入你的`sass`局部文件之前声明了一个`$fancybox-width`变量，那么你的局部文件中对`$fancybox-width`赋值`400px`的操作就无效。如果用户没有做这样的声明，则`$fancybox-width`将默认为`400px`

```scss
$fancybox-width: 400px !default;
.fancybox {
width: $fancybox-width;
}
```

####  原生的CSS导入

sass的` @import`与css原生的` @import`不同，`sass`兼容原生的`css`，在下列三种情况下会生成原生的`CSS@import`

-   被导入文件的名字以`.css`结尾；
-   被导入文件的名字是一个URL地址（比如http://www.sass.hk/css/css.css），由此可用谷歌字体API提供的相应服务；
-   被导入文件的名字是`CSS`的url()值。



## 混合器

通过`sass`的混合器`mixin`实现大段样式的重用，再通过`@include`来使用这个混合器。

```scss
@mixin rounded-corners {
  -moz-border-radius: 5px;
  -webkit-border-radius: 5px;
  border-radius: 5px;
}
.notice {
  background-color: green;
  border: 2px solid #00aa00;
  @include rounded-corners;
}
//编译后
.notice {
  background-color: green;
  border: 2px solid #00aa00;
  -moz-border-radius: 5px;
  -webkit-border-radius: 5px;
  border-radius: 5px;
}
```

#### 混合器中的CSS规则

混合器中不仅可以包含属性，也可以包含`css`规则，包含选择器和选择器中的属性，引入混合器的那行代码替换成混合器里边的内容。最终，上边的例子如下代码:

```scss
@mixin no-bullets {
  list-style: none;
  li {
    list-style-image: none;
    list-style-type: none;
    margin-left: 0px;
  }
}
ul.plain {
  color: #444;
  @include no-bullets;
}
//编译后
ul.plain {
  color: #444;
  list-style: none;
}
ul.plain li {
  list-style-image: none;
  list-style-type: none;
  margin-left: 0px;
}
```

#### 给混合器传参

```scss
@mixin link-colors($normal, $hover, $visited) {
  color: $normal;
  &:hover { color: $hover; }
  &:visited { color: $visited; }
}

a {
    @include link-colors(
      $normal: blue,
      $visited: green,
      $hover: red
  );
}

//Sass最终生成的是：

a { color: blue; }
a:hover { color: red; }
a:visited { color: green; }
```

#### 默认参数

为了在`@include`混合器时不必传入所有的参数，我们可以给参数指定一个默认值。**参数默认值** 是在**形参列表**中使用`$name: default-value`的声明形式，默认值可以是任何有效的`css`属性值，甚至是其他参数的引用，如下代码：

```scss
@mixin link-colors(
    $normal,
    $hover: $normal,
    $visited: $normal
  )
{
  color: $normal;
  &:hover { color: $hover; }
  &:visited { color: $visited; }
}
```

## 选择器继承

选择器继承是说一个选择器可以继承为另一个选择器定义的所有样式。这个通过`@extend`语法实现

```scss
//通过选择器继承继承样式
.error {
  border: 1px red;
  background-color: #fdd;
}
.seriousError {
  @extend .error;
  border-width: 3px;
}
```

在上边的代码中，`.seriousError`将会继承样式表中任何位置处为`.error`定义的所有样式。以`class="seriousError"` 修饰的`html`元素最终的展示效果就好像是`class="seriousError error"`。相关元素不仅会拥有一个`3px`宽的边框，而且这个边框将变成红色的，这个元素同时还会有一个浅红色的背景，因为这些都是在`.error`里边定义的样式。

`.seriousError`不仅会继承`.error`自身的所有样式，任何跟`.error`有关的**组合选择器样式**也被`.seriousError`以组合选择器的形式继承，如下代码:

```scss
//.seriousError从.error继承样式
.error a{  //应用到.seriousError a
  color: red;
  font-weight: 100;
}
h1.error { //应用到hl.seriousError
  font-size: 1.2rem;
}
```

#### 继承的高级用法

`#main .seriousError`**@extend**另一个选择器`.error` 除了有`class="seriousError"`的` #main`元素之外的元素不会受到影响。



关于`@extend`有两个要点应该知道。

-   跟混合器相比，继承生成的`css`代码相对更少。因为继承仅仅是重复选择器，而不会重复属性，所以使用继承往往比混合器生成的`css`体积更小。如果你非常关心你站点的速度，请牢记这一点。
-   继承遵从`css`层叠的规则。当两个不同的`css`规则应用到同一个`html`元素上时，并且这两个不同的`css`规则对同一属性的修饰存在不同的值，`css`层叠规则会决定应用哪个样式。相当直观：通常权重更高的选择器胜出，如果权重相同，定义在后边的规则胜出。