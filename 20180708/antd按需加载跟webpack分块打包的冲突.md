antd按需加载跟webpack分块打包的冲突
============================
antd升级到3.x遇到一个问题按需加载问题，即像antd官网所说的那样；使用 babel-plugin-import（推荐）
```js
// .babelrc or babel-loader option
{
  "plugins": [
    ["import", { "libraryName": "antd", "libraryDirectory": "es", "style": "css" }] // `style: true` 会加载 less 文件
  ]
}
//或是在webpack 的load里面配置
 {
    test: /\.(js|jsx)$/,
    exclude: /node_modules/,
    loader: 'babel',
    query: {
      cacheDirectory: true,
      plugins: [
        ["import", {"libraryName": "antd", "style": "true"}]
      ]
    }
  }
```
然后只需从 antd 引入模块即可，无需单独引入样式。等同于下面手动引入的方式。
```js
// babel-plugin-import 会帮助你加载 JS 和 CSS
import { DatePicker } from 'antd';
```
但是无论是在babelrc或是webpack里面配置，都不起作用还是会报
```js
You are using a whole package of antd, please use https://www.npmjs.com/package/babel-plugin-import to reduce app bundle size.
```
解决方法是：antd 不能分块打包即不能把antd配置在vendor里，进行分块打包；如：
```js
  entry: {
    app: [
      paths.client('entry.jsx')
    ],
    vendor: ['antd'] // 错误例子 这边应该把antd去掉
  }
```