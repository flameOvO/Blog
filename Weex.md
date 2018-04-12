#### 样式差异

- 不支持`display: none;` 无法是用`v-show`
- 样式属性暂不支持简写（提高解析效率）
- flex布局需要注意web的兼容性；部分属性并不支持，如 `align-items:baseline;`、`align-content:space-around;`、`align-self:wrap_reverse;`等。
- 不支持css动画和3D样式

weex 中的所有 css 属性值的单位均为 `px`，也可省略不写，系统会默认为 `px`单位。

weex中的所有标签都为组件。

weex环境中没有DOM

weex只支持有限的事件类型。

Weex 中只支持单个类名选择器，不支持关系选择器，也不支持属性选择器。

```

```

