## 基本概念

1. `entry` 属性，来指定一个入口起点（或多个入口起点）。指示 webpack 应使用哪个模块，来作为构建其内部*依赖图*的开始。
2. `output`属性告诉 webpack 在哪里输出它所创建的 *bundles*，以及如何命名这些文件。
3. `loader `让 webpack 能够去处理那些非 JavaScript 文件（webpack 自身只理解 JavaScript）。loader 可以将所有类型的文件转换为 webpack 能够处理的有效模块，然后你就可以利用 webpack 的打包能力，对它们进行处理。
   * `test` 属性，用于标识出应该被对应的 loader 进行转换的某个或某些文件。
   * `use` 属性，表示进行转换时，应该使用哪个 loader。
   * 在 webpack 配置中定义 loader 时，要定义在 module.rules 中，而不是 rules。
4. `plugins`想要使用一个插件，你只需要 `require()` 它，然后把它添加到 `plugins` 数组中。多数插件可以通过选项(option)自定义。你也可以在一个配置文件中因为不同目的而多次使用同一个插件，这时需要通过使用 `new` 操作符来创建它的一个实例。

```js
const HtmlWebpackPlugin = require('html-webpack-plugin'); // 通过 npm 安装
const webpack = require('webpack'); // 用于访问内置插件
const path = require('path');
module.exports = {
  entry: './path/to/my/entry/file.js',//入口文件
  output: {
    path: path.resolve(__dirname, 'dist'),//bundle的生成位置
    filename: 'my-first-webpack.bundle.js'// bundle 的名称
  }
 module: {
    rules: [
      { test: /\.txt$/, use: 'raw-loader' }
    ]
  },
  plugins: [
    new webpack.optimize.UglifyJsPlugin(),
    new HtmlWebpackPlugin({template: './src/index.html'})
  ]
};

module.exports = config;
```


## 入口起点(Entry)





## loader

####loader 特性

- loader 支持链式传递。loader 链中的第一个 loader 返回值给下一个 loader。在最后一个 loader，返回 webpack 所预期的 JavaScript。
- loader 可以是同步的，也可以是异步的。
- loader 运行在 Node.js 中，并且能够执行任何可能的操作。
- loader 接收查询参数。用于对 loader 传递配置。
- loader 也能够使用 `options` 对象进行配置。
- 除了使用 `package.json` 常见的 `main` 属性，还可以将普通的 npm 模块导出为 loader，做法是在 `package.json` 里定义一个 `loader` 字段。
- 插件(plugin)可以为 loader 带来更多特性。
- loader 能够产生额外的任意文件。