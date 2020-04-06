# code-package-mark
一些工具包记录

## cross-env
* 运行跨平台设置和使用环境变量的脚本
* https://www.npmjs.com/package/cross-env

## vue-fragment
* 解决template中只有一个根元素的问题，vue3.0中有官方解决方案，2.0替代组件库
* https://www.npmjs.com/package/vue-fragment
* 注意点1：不能将fragment组件当成兄弟组件使用，否则会报错

## @babel/plugin-proposal-optional-chaining
* es2017新特性 属性可选链访问
* 在javaScript中，对象的属性链访问，很容易因为某一个属性不存在出现
Cannot read property 'xxx' of undefined的问题，那么Optional Chaining就添加了?.操作符，它会先判断前面的值，如果undefined或者null，就结束后面的调用，直接返回undefined;
* 目前浏览器还不支持此特性，需要使用babel插件进行转义

## postcss-px-to-viewport
* webpack 插件，自动实现px到vw的转化
* https://www.npmjs.com/package/postcss-px-to-viewport

## webpack-bundle-analyzer
*  打包大小分析插件
*  使用方法：在vue.config.js中加入以下代码
```javascript 
	chainWebpack : config => {
        config
        .plugin("webpack-bundle-analyzer")
        .use(require("webpack-bundle-analyzer").BundleAnalyzerPlugin)
        .end();
        config.plugins.delete("prefetch")
    }
```

## big.js
* 使用目的：
1. 完美的十进制运算
2. 超出整数值的安全计算
* 缺点：
不要用于UI或以性能为目的的运算