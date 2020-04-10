#### 关于 portal 项目拆分具体过程

一. [共同部分](http://gitlab.tengsaw.cn/xuman/portal-common)提取

- 提取了 components,directives,filters, icons,styles,utils
- 使用：

  ![3.png](https://github.com/ghostInHeart/work/blob/master/images/portal-separate/3.png)

  页面中引入：

  ![4.png](https://github.com/ghostInHeart/work/blob/master/images/portal-separate/4.png)

二. [RISK-风控模块](http://gitlab.tengsaw.cn/xuman/risk-package) 提取

- 复制一份项目，去除了与风控无关的东西，保留了最主要的 src 下的风控的一些基本页面(/views/risksystem),及其相关联的 api，router，assets，store,这些子模块私有的东西可以自己维护。

- 子模块的配置文件都保留,才能独立启动项目,各种配置文件可以只保留私有的配置,但是格式建议与主模块保持一致。

- 在入口文件(/src/index.ts)中导出所有页面,注意路径问题

![5.png](https://github.com/ghostInHeart/work/blob/master/images/portal-separate/5.png)

三. [主模块](http://gitlab.tengsaw.cn/xuman/portal-main)提取

- 保留系统配置，其他东西剔除

- 菜单权限，layout 等全局的东西在主模块配置，各子模块直接展示

- 引入子模块后, 在/src/router/\_import.ts 中配置,根据后端返回的菜单来动态导入相应的页面,不在菜单上的比如详情页面,直接引入路由即可。

![6.png](https://github.com/ghostInHeart/work/blob/master/images/portal-separate/6.png)

![7.png](https://github.com/ghostInHeart/work/blob/master/images/portal-separate/7.png)

四. 拆分过程中遇到的问题

- 引入 risk 子模块详情路由文件时, webpack 循环依赖机制引起问题,
  当主模块与子模块都引入了 Layout,导致导入的子模块的详情页不能显示

  ![1.png](https://github.com/ghostInHeart/work/blob/master/images/portal-separate/1.png)
  ![2.jpg](https://github.com/ghostInHeart/work/blob/master/images/portal-separate/2.jpg)

  解决方法：更换引入的顺序，参考[该文档](https://stackoverflow.com/questions/35240716/webpack-import-returns-undefined-depending-on-the-order-of-imports)
