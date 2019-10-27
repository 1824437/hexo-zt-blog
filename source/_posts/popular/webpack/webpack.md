---
title: webpack
categories: popular
tags: [webpack]
---

## link

[中文文档](https://doc.webpack-china.org/)  
**待读** happypack 原理 [https://yq.aliyun.com/articles/67269](https://yq.aliyun.com/articles/67269)

## 资源处理

### 紧密耦合式

1. css处理方案

	#### 直接通过JS内嵌到文档的head中
	1. 安装使用loader: style-loader,css-loader。

	2. 在js中`import 'styles.css'`即可。bundle时会通过JS写入文档的head中。

2. 图片、字体处理方案

	#### js引入

	1. 安装使用file-loader
	2. 在js中 `import img from './assets/images/icon.png'，然后new Image一个对象指向src。bundle后会把文件复制到dist中。

	css中的图片，css-loader也会做同样的事情。无论图片在哪里使用，都会生成一个唯一图片供所有地方引用。

	css中的字体，仍然可以使用file-loader来处理。


3. CSV、TSV、XML文件处理方案

	#### csv,tsv使用csv-loader
	#### xml使用xml-loader

4. scss处理

## 管理输出

1. 利用html-webpack-plugin来生成index.html文件

## 动态热替换

通过webpack-dev-server实现。
如果出现编译异常，可能是包的问题。先看webpack-dev-server的package.json。看webpack与webpack-dev-server的版本号对不对。如果安装的对，如果再有问题，可以参照下面的已装包，看有没有缺失。

```
"webpack-cli": "^3.3.2",
    "webpack-dev-middleware": "^3.6.2",
    "webpack-dev-server": "^3.3.1",
    "webpack-hot-middleware": "^2.24.4"
```

## 代码分离


[学习链接](https://juejin.im/post/5b99b9cd6fb9a05cff32007a)
[2](https://segmentfault.com/a/1190000013476837)

## 各字段详解

**output.path,output.publicPath,devServer.publicPath,devServer.contentBase**

1. output.path, 指定打包输出的目录。

	将源代码打生产包时，生成在哪个文件夹下。所有的包，包含js,css,image,svg,font，及入口文件index.html。
	
2. output.publicPath

	这个配置是将代码内所有的资源引用路径都加上一个前缀。默认的publicPath,输出的index.html对js引用是`/app.*.js`,如果加上`outpub.publicPath = '/abc/'`,那么输出的index.html对js的引用会变成`/abc/app.*.js`.不仅是index.html,所有的所有包的引用路径者会这样变。
	
	注：`output.path: 'dist/abc'与将publicPath设置为'/abc/'是不同的`。`output.path: 'dist/abc'`只会将包输出到dist/abc目录下，不会改变包的引用。
	
	
3. devServer.publicPath

	控制在开发环境下给资源引用路径加上前缀。如果没有配置这个选项，则使用output.publicPath。

4. devServer.contentBase

	在开发环境下指定静态文件(index.html)的位置，会读取指定的index.html而不是HtmlWebpackPlugin生成的。对生产包没有影响。
	这个必须配合devServer.publicPath
	
**[externals](https://www.webpackjs.com/configuration/externals/#externals)**

> 需要以require或import的方式使用，但已通过\<script>链接了公共资源; 打包时需要忽略掉require的源码，这种情况下需要使用。

* string  
`externals:{
key: value}`  
key 表示在js中引用的变
value 表示CDN抛出的全局变量。如

`externals:{
    '$Vue': 'Vue'}`, js中`import Vue from '$Vue'`

## 问题

1. 配置`rules: [{
      test: require.resolve('./src/index.js'),
      use: 'imports-loader?this=>window'
    }]`后，index.js文件内有`import numRef from './ref.json'`引入json报错`ERROR in ./src/index.js 4:0
Module parse failed: 'import' and 'export' may only appear at the top level (4:0)
You may need an appropriate loader to handle this file type.`

2.

```
ERROR in chunk another [entry]
[name].[chunkhash].js
Cannot use [chunkhash] or [contenthash] for chunk in '[name].[chunkhash].js' (use [hash] instead)
```
原因：热更新(HMR)不能和[chunkhash]同时使用。

3. 打包时，hash频繁变动? 

	生产环境添加Plugins: new webpack.HashedModuleIdsPlugin()。
	**但是，当入口文件增加时，hash仍然会变化，怎么解？**
	1. 公共包可以解决，将chunkFilename的名字不加上hash。

4. node-sass安装不成功。

	如果遇到错误 error: xxxx node-sass: Command failed
	
将 sass-binary-site 添加至 config 中

`yarn config set sass-binary-site https://npm.taobao.org/mirrors/node-sass`

`npm config set sass-binary-site https://npm.taobao.org/mirrors/node-sass`

指定 node-sass 从 npm 的淘宝源中下载。
	
	
5. vue中的css被单独打包了，如果不想单独打包，怎么办？

	很简单，如果不需要单独打包，就别写在vue文件中。写在vue文件中，就会被单独打包。

##应用

### [模块热替换](https://www.webpackjs.com/guides/hot-module-replacement/#%E5%90%AF%E7%94%A8-hmr)

**不是配置webpack HMR后，就可以任意热替换所有的模块**。
实际上是通过module.hot.accept来监控某个模块，如a.js, 然后运行a.js内某个方法。例如，修改之前a()是console.log(1);现在更改了a.js的a方法,改成了console.log(2)。热替换就是在module.hot.accept的回调里运行a()能保证是新的，也就是输出2。如果页面有绑定事件执行a()仍然是输出1。
所以不光是webpack.config配置，还是要在js中配合使用。

### css中使用alias

只需要在开头加`~`

### 分包

1. 分包配置 [资料1](https://juejin.im/post/5b99b9cd6fb9a05cff32007a) [资料2](https://juejin.im/post/5af1677c6fb9a07ab508dabb) [资料3](https://www.cnblogs.com/dashnowords/p/9545482.html) [资料4](https://segmentfault.com/a/1190000010317802)

	> 之前个人想法，是不是输出一个专门的入口包文件。事实上很蠢。因为包文件没有公布全局变量。

	使用`optimization.splitChunks`
	
	```
	optimization: {
    splitChunks: {
      chunks: 'initial',
      cacheGroups: {
        vue: {
          name: 'vue-2.6.10',
          test: /vue|vuex|vue-router/
        },
        react: {
          name: 'react-16.8.6',
          test: /react|react-dom/
        }
      }
    }
  }
	```
	
	最麻烦的场景： 如有2个包jquery,lodash。在入口a.js中引入了其中一个jquery。在异步b.js中引了这2个。如果
	
	
### webpack - vue配置

1. 语法检查 eslint-plugin-vue

	1. 安装 eslint-plugin-vue
	2. eslintrc.js 增加extends: 'plugin:vue/recommended'

2. 预处理

	1. 安装 vue-loader, 提示安装vue-template-compiler
	2. 在webpack配置中增加plugins.

		`const VueLoaderPlugin = require('vue-loader/lib/plugin');`
		
		`plugins: [new VueLoaderPlugin()]`


3. 环境场景1: Vue在生产环境中使用CDN,开发环境中使用完整包

	1. dev.config:

		```
		resolve: {
	    alias: {
	      $Vue: 'vue'
	    }
	  }
		```
		为什么要配置webpack.dev.config; 完全可以不用配置也能正常运行。但是打包的时候在app.js里面也会打进vue的源码。只有在dev使用alias,在prod使用externals，打包的时候不会打包vue源。这种原理同样适合所以的包。
		
	2. prod.config:

		```
		externals:{
    '$Vue': 'Vue'
  }
		```
		
	3. html

		```
		<script src="http://library.theme.com/vue/dist/vue.min.js"></script>
    <script src="http://library.theme.com/vue-router/dist/vue-router.min.js"></script>
    <script src="http://library.theme.com/vuex/dist/vuex.min.js"></script>
		```

	4. js

		```
		import Vue from '$Vue';
		if (process.env.NODE_ENV === 'development') {
		  Vue.use(VueRouter);
		  Vue.use(Vuex);
		}
		```
		并不需要引用vuex和vuerouter,开发环境和生产环境都用cdn就可以了。

4. 环境场景2：将vue及插件单独打包，不使用CDN。

	```
	optimization: {
    splitChunks: {
      chunks: 'initial',
      cacheGroups: {
        vue: {
          name: 'vue-2.6.10',
          test: /vue|vuex|vue-router/
        }
      }
    }
  }
	```
	
### 将css打包成文件输出 

使用[mini-css-extract-plugin](https://www.npmjs.com/package/mini-css-extract-plugin)

### css预处理

1. 安装

### js支持es6

### es7装饰器插件

1. `yarn add @babel/plugin-proposal-decorators -D`

2. `"plugins": [
    ["@babel/plugin-proposal-decorators", { "legacy": true }],
  ]`
  
注意: 要放在plugin-proposal-class-properties前面。

	['@babel/plugin-proposal-decorators', { 'legacy': true }],
	'@babel/plugin-proposal-class-properties'

   