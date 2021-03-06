## 生命周期



|  vue2.0钩子   |                             描述                             |
| :-----------: | :----------------------------------------------------------: |
| beforeCreate  |        组件实例刚被创建，组件属性计算之前，如data属性        |
|    created    | 组件实例创建完成，属性已绑定，但DOM还未生成，$el属性还不存在 |
|  beforeMount  |                      模板编译/挂载之前                       |
|    mounted    |         模板编译/挂载之后(不保证组件已在document中)          |
| beforeUpdate  |                         组件更新之前                         |
|    updated    |                         组件更新之后                         |
|   activated   |              for` keep-alive` ,组件被激活时调用              |
|  deactivated  |              for `keep-alive` 组件被移除时调用               |
| beforeDestory |                        组件销毁前调用                        |
|   destoryed   |                        组件销毁后调用                        |

![Vue 实例生命周期](https://cn.vuejs.org/images/lifecycle.png)

## 条件渲染

###  v-if和v-show

`v-if` 是“真正”的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。

`v-if` 也是**惰性的**：如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。

相比之下，`v-show` 就简单得多——不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换。

一般来说，`v-if` 有更高的切换开销，而 `v-show` 有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用 `v-show` 较好；如果在运行时条件很少改变，则使用 `v-if` 较好。







```
//为什么v-bind:class的第二个class需要加引号
<div class="static"
     v-bind:class="{ active: isActive, 'text-danger': hasError }">test
</div>

```

因为 {}内的代码是要拿去当js解析的，js中变量是没有用 ' - '号连接
就像 css中 font-size 是用 - 号连接
到了js中 就必须用驼峰的写法



@font-face  CSS unicode-range特定字符使用font-face自定义字体