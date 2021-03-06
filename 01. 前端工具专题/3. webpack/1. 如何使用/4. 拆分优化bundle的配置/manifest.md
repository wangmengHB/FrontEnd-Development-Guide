# manifest
在使用 webpack 构建的典型应用程序或站点中，有三种主要的代码类型：
1. 你自己的代码
2. 依赖的第三方库代码
3. webpack的 runtime 和 manifest, 管理所有模块的交互.

runtime, 以及伴随的 manifest 数据，主要是指：在浏览器运行时，webpack用来连接模块化的应用程序的所有代码.
runtime 包含：在模块交互时，连接模块所需的加载和解析逻辑. 包括浏览器中的已加载模块的连接，以及懒加载模块的执行逻辑.

一旦你的应用程序中，形如 index.html 文件、一些 bundle 和各种资源加载到浏览器中，会发生什么？你精心安排的 /src 目录的文件结构现在已经不存在，所以 webpack 如何管理所有模块之间的交互呢？这就是 manifest 数据用途的由来

当编译器(compiler)开始执行、解析和映射应用程序时，它会保留所有模块的详细要点。这个数据集合称为 "Manifest"，当完成打包并发送到浏览器时，会在运行时通过 Manifest 来解析和加载模块。
无论你选择哪种模块语法，那些 import 或 require 语句现在都已经转换为 __webpack_require__ 方法，此方法指向模块标识符(module identifier)。通过使用 manifest 中的数据，runtime 将能够查询模块标识符，检索出背后对应的模块。

如果你决定通过使用浏览器缓存来改善项目的性能，理解这一过程将突然变得尤为重要。

通过使用 bundle 计算出内容散列(content hash)作为文件名称，这样在内容或文件修改时，浏览器中将通过新的内容散列指向新的文件，从而使缓存无效。一旦你开始这样做，你会立即注意到一些有趣的行为。即使表面上某些内容没有修改，计算出的哈希还是会改变。这是因为，runtime 和 manifest 的注入在每次构建都会发生变化。

查看管理构建文件指南的 manifest 部分，了解如何提取 manifest，并阅读下面的指南，以了解更多长效缓存错综复杂之处。


```js
optimization: {
    splitChunks: {
        ...
    },

    runtimeChunk: {
        name: "manifest",
    },
},
```