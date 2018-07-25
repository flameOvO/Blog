##AMD

`AMD`，异步模块定义（Asynchronous Module Definition），它是依赖前置 (依赖必须一开始就写好)会先尽早地执行(依赖)模块 。换句话说，所有的`require`都被提前执行（require 可以是全局或局部 ）。 



## export default 和 export 区别：

1.export与export default均可用于导出常量、函数、文件、模块等
2.你可以在其它文件或模块中通过import+(常量 | 函数 | 文件 | 模块)名的方式，将其导入，以便能够对其进行使用
3.在一个文件或模块中，export、import可以有多个，export default仅有一个
4.通过export方式导出，在导入时要加{ }，export default则不需要