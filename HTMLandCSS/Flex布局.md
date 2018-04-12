

### flex-direction

### justify-content

### align-items



`flex` 是 `flex-grow`、`flex-shrink`、`flex-basis`的缩写。

`flex` 的默认值是以上三个属性值的组合。假设以上三个属性同样取默认值，则 `flex` 的默认值是 0 1 auto。

当 `flex` 取值为一个非负数字，则该数字为 `flex-grow` 值

当 `flex` 取值为一个**长度**(带单位的数字)或百分比，则视为 `flex-basis` 值



`flex-basis` 规定的是子元素的基准值。所以是否溢出的计算与此属性息息相关。`flex-basis` 规定的范围取决于 `box-sizing`。这里主要讨论以下 `flex-basis` 的取值情况：

- `auto`：首先检索该子元素的主尺寸，如果主尺寸不为 `auto`，则使用值采取主尺寸之值；如果也是 `auto`，则使用值为 `content`。
- `content`：指根据该子元素的内容自动布局。有的用户代理没有实现取 `content` 值，等效的替代方案是 `flex-basis` 和主尺寸都取 `auto`。
- 百分比：根据其包含块（即伸缩父容器）的主尺寸计算。如果包含块的主尺寸未定义（即父容器的主尺寸取决于子元素），则计算结果和设为 `auto` 一样。

```css
<div class="parent">
    <div class="item-1"></div>
    <div class="item-2"></div>
    <div class="item-3"></div>
</div>

<style type="text/css">
    .parent {
        display: flex;
        width: 600px;
    }
    .parent > div {
        height: 100px;
    }
    .item-1 {
        width: 140px;
        flex: 2 1 0%;
        background: blue;
    }
    .item-2 {
        width: 100px;
        flex: 2 1 auto;
        background: darkblue;
    }
    .item-3 {
        flex: 1 1 200px;
        background: lightblue;
    }
</style>
```

- 主轴上父容器总尺寸为 600px

- 子元素的总基准值是：0% + auto + 200px = 300px，其中

  ```
  - 0% 即 0 宽度
  - auto 对应取主尺寸即 100px
  ```

- 故剩余空间为 600px - 300px = 300px

- 伸缩放大系数之和为： 2 + 2 + 1 = 5

- 剩余空间分配如下：

  ```
  - item-1 和 item-2 各分配 2/5，各得 120px
  - item-3 分配 1/5，得 60px
  ```

- 各项目最终宽度为：

  ```
  - item-1 = 0% + 120px = 120px
  - item-2 = auto + 120px = 220px
  - item-3 = 200px + 60px = 260px
  ```

- 当 item-1 基准值取 0% 的时候，是把该项目视为零尺寸的，故即便声明其尺寸为 140px，也并没有什么用，形同虚设

- 而 item-2 基准值取 `auto` 的时候，根据规则基准值使用值是主尺寸值即 100px，故这 100px 不会纳入剩余空间