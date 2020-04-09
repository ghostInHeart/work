#### 关于 portal 项目拆分

一. 使用 webpack 简洁版搭建项目，基本除了 vue 没什么依赖都需要自己配置, 这个模板版本太低了，兼容性不好，无法进行子项目开发

二. 使用 webpack 完整版（cli）搭建项目，打包之后引入到父模块,存在问题：

- 子模块路由与父模块路由重复定义的问题 (调用两次 vue.use 导致)。
- 子模块打包后体积过大

三. 使用 webpack 完整版（cli）搭建项目, 不打包，直接动态导入，子模块可独立开发，目前可行

- 子模块路径需要相对路径 @绝对查找会从父开始
- 路由相关问题 子路由是否写死 需要后端提供字段判断是从哪个模块引入的包
- 哪些公用的组件 或者 插件需要提出来 细节化

#### 关于 portal 项目拆分具体过程

一. 共同部分的提取

二. RISK-风控模块 提取

三. 主模块提取

四. 使用过程中遇到的问题

1. 引入 risk 子模块详情路由文件时, webpack 循环依赖 存在问题
   <br>
   主模块与子模块都引入了 Layout
   <br>
   ![1.jpg](https://github.com/ghostInHeart/work/blob/master/images/portal-separate/1.jpg)
   ![2.jpg](https://github.com/ghostInHeart/work/blob/master/images/portal-separate/2.jpg)
   <br>
   解决方法：更换引入的顺序，参考[该文档](https://stackoverflow.com/questions/35240716/webpack-import-returns-undefined-depending-on-the-order-of-imports)
