**92. 你用过那些 webpack 插件？**

html-webpack-plugin 自动生成 html 插件，它会在 dist 目录下自动生成一个 index.html（或多个）

babel-loader 对 ES6 语法进行转码

css-loader 对 css 文件打包

style-loader 将样式添加进 dom 中

url-loader 图片自动转成 base64 编码

UglifyJsPlugin //压缩混淆插件

DefinePlugin //决定打成 dev 包还是 production 包

**98. 为什么要用 Babel**

\1. 可以把 ES6 转为 ES5

\2. 可以在 VUE 中使用 JSX 语法

\3. 可以使用大括号按需加载