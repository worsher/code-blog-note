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
* es2017新特性 属性链访问
* 在javaScript中，对象的属性链访问，很容易因为某一个属性不存在出现
Cannot read property 'xxx' of undefined的问题，那么Optional Chaining就添加了?.操作符，它会先判断前面的值，如果undefined或者null，就结束后面的调用，直接返回undefined;
* 目前浏览器还不支持此特性，需要使用babel插件进行转义
