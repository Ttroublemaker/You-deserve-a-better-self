# Webpack篇
webpack已是前端打包构建的不二选择，必考    
重在配置和使用，不在原理

基本配置  
高级配置  
优化打包速度  
优化产出代码  
构建流程概述  
Babel  

### 1）Webpack基本配置
必须掌握  

#### 1：拆分配置和merge

![config-merge](imgs/webpack/config-merge.png)

#### 2：启动本地服务
webpack-dev-server   
devServer 配置项

#### 3：处理es6
babel-loader .babelrc文件 polyfill

#### 4：处理样式
style-loader css-loader scss-loader postcss-loader(配合autoprefixer添加厂商前缀)

#### 5：处理图片
file-loader/url-loader(配合options limit 配置项可以控制是否按吧base64格式产出

<hr>

### 2）Webpack高级配置
必须掌握  

#### 1：多入口
entry output

![多入口](imgs/webpack/multi-entry.png)

#### 2：抽离css文件
常用于生产环境，开发环境没有必要

![抽离css文件](imgs/webpack/css-split.png)

官网中提到的extract-text-webpack-plugin插件要依赖webpack3的版本。

#### 3：抽离公共代码（重要）
抽离公共代码及第三方代码
![抽离公共代码](imgs/webpack/code-split.png)

#### 4：实现异步加载

![实现异步加载](imgs/webpack/async-import.png)

#### 5：处理JSX和vue
![处理JSX和vue](imgs/webpack/jsx&vue.png)


#### 6：module chunk bundle 的区别
module:各源文件,在webpack中,一切皆模块  
chunk:多个模块合并合成的,比如entry import() splitChunk  
bundle:最终输出文件  

<hr>

### 3）webpack性能优化(重要）

优化打包构建速度---开发体验和效率  
优化产出代码---产品性能  

#### 1）优化打包构建速度
##### 1：优化babel-loader
开启缓存，未修改过的es6+代码，就不会重新编译  
include or exclude(exclude优先级高于前者)

![优化babel-loader](imgs/webpack/babel-loader-optimization.png)

##### 2：IgnorePlugin
忽略第三方包指定目录，让这些指定目录不要被打包进去

![IgnorePlugin](imgs/webpack/ignorePlugin.png)

new Webpack.IgnorePlugin(/\.\/locale/,/moment/)
moment这个库中，如果引用了./locale/目录的内容，就忽略掉，不会打包进去

##### 3：noParse
用了noParse的模块将不会被loaders解析，所以当我们使用的库如果太大，并且其中不包含import require、define的调用，我们就可以使用这项配置来提升性能, 让 Webpack 忽略对部分没采用模块化的文件的递归解析处理。

![noParse](imgs/webpack/noParse.png)

##### 4：happyPack（多进程打包）
多进程打包，提高构建速度（特别是多核CPU）

![happyPack](imgs/webpack/happyPack.png)

##### 5：ParallelUglifyPlugin（多进程压缩js，常用于生产环境）
webpack内置Uglify工具压缩js（单进程）

项目较大，打包较慢时，开启多进程能提高速度
项目较小，打包很快，开启多进程会降低速度（进程开销）
所以，按需使用

![ParallelUglifyPlugin](imgs/webpack/ParallelUglifyPlugin.png)

##### 6：自动刷新（开发环境）
无需自行配置，启动webpack-dev-server会自动开启该功能

![自动刷新](imgs/webpack/auto-fresh.png)

##### 7：热更新（开发环境）
不丢失状态

![HMR](imgs/webpack/HMR.png)


##### 8：DllPlugin 动态链接库插件（开发环境）
前端框架如vue React，体积大，构建慢  
较稳定，不常升级  
同一版本只构建一次即可，不用每次都重新构建  

webpack 已经内置DllPlugin支持  
DllPlugin---打包出dll文件  
DllReferencePlugin---使用dll文件  

![DllPlugin](imgs/webpack/DllPlugin.png)

![DllReferencePlugin](imgs/webpack/DllReferencePlugin.png)



再将react.dll.js文件引入html模板中即可（切勿忘记）

#### 2）优化产出代码（比构建速度更重要）
体积更小  
合理分包，不重复加载  
速度更快，内存使用更小（代码执行更快）  

##### 1：小图片base64编码
##### 2：bundle加hash （使用contentHash，只有文件变更后才加载新内容）
##### 3：懒加载
##### 4：提取公共代码
##### 5：IngorePlugin
（忽略第三方包指定目录，让这些指定目录不要被打包进去）
##### 6：使用CDN加速（通过配置publicPath）
##### 7：使用production模式
自动开启代码压缩、Vue/React等会自动删掉调试代码（如开发环境的warning  ）
自动启动tree-shaking  
为了学会使用 tree shaking，你必须:  
使用 ES2015 模块语法（即 import 和 export）
在项目 package.json 文件中，添加一个 "sideEffects" 入口(注意，任何导入的文件都会受到 tree shaking 的影响。这意味着，如果在项目中使用类似 css-loader 并导入 CSS 文件，则需要将其添加到 side effect 列表中，以免在生产模式中无意中将它删除：)
引入一个能够删除未引用代码(dead code)的压缩工具(minifier)

备注：ES6 Module和Commonjs区别  
ES6 Module是静态引入，编译时引入，Commonjs是动态引入，执行时引入，所以只有ES6 Module才能静态分析，实现Tree-Shaking

![module&commonjs](imgs/webpack/module&commonjs.png)

##### 8：Scope Hoisting
代码体积更小  
创建函数作用域更少  
代码可读性更好  
Scope Hoisting 它可以让webpack打包出来的代码文件更小，运行更快，它可以被称作为 "作用域提升"。
https://blog.csdn.net/qq_36380426/article/details/107298332

![Scope Hoisting](imgs/webpack/Scope%20Hoisting.png)

<hr>

### 4）babel 
https://www.babeljs.cn/setup#installation  
需要了解基本的配置和使用，考察概率不高，但必须会  

#### 1）环境搭建+基本配置
环境搭建/.babelrc配置/presets和plugins
(preset 可以作为 Babel 插件的组合)

备注:  
Babel默认只转换新的JavaScript语法(如箭头函数)，而不转换新的API。 例如，Iterator、Generator、Set、Maps、Proxy、Reflect、Symbol、Promise 等全局对象，以及一些定义在全局对象上的方法(比如 Object.assign)都不会转译。 如果想使用这些新的对象和方法，则需要为当前环境提供一个polyfill

#### 2）babel-polyfill
NOTE:  
As of Babel 7.4.0, this package has been deprecated in favor of directly including core-js/stable (to polyfill ECMAScript features) and regenerator-runtime/runtime (needed to use transpiled generator functions)

![babel-polyfill](imgs/webpack/babel-polyfill.png)

babel-polyfill 存在的问题:  
###### 会污染全局环境  
如果做的是web系统，没有关系。如果作为一个library库，则会有负面影响  
解决方案：babel-runtime  
故如果输出的web系统，使用babel-polyfill，如果输出library库，使用babel-runtime

#### 3）babel-runtime  
@babel/plugin-transform-runtime

![babel-runtime](imgs/webpack/babel-runtime.png)

<hr>

### 5）面试真题
#### 1：前端为何要进行打包和构建
体积更小（Tree-Shaking、压缩、合并），加载更快  
编译高级语言或者语法（TS、ES6+、模块化、scss）  
兼容性和错误检查（polyfill、postcss、eslint）  
统一、高效的开发环境  
统一的构建流程和产出标准  
集成公司构建规范（提测、上线等）  

#### 2：module chunk bundle的区别
module:各源文件,在webpack中,一切皆模块  
chunk:多个模块合并合成的,比如entry import() splitChunk  
bundle:最终输出文件  

#### 3：loader和plugin的区别
loader模块转换器，如less->css
plugin扩展插件，如HtmlWebpackPlugin

#### 4：常见loader和plugin有哪些
参考官网

#### 5：babel和webpack的区别
Babel---js新语法编译工具，不关心模块化  
Webpack---打包构建工具，是多个loader plugin的集合  

#### 6：如何产出一个lib
参考DllPlugin章节
output.library

#### 7：babel-polyfill 和babel-runtime的区别
前者会污染全局，后者不会，产出第三方lib要用babel-runtime

#### 8：webpack 如何实现懒加载
import()  
结合Vue React 异步组件  
结合Vue-router React-router异步加载路由  

#### 9:为何Proxy不能被Polyfill
Class 可以用function模拟  
Promise可以用callback模拟  
但是Proxy的功能用Object.defineProperty模拟

#### 10：优化构建速度
参考之前章节

#### 11：优化产出代码
参考之前章节