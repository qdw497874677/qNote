# 安装



~~~
npm i element-ui -S
~~~

初始化新的项目

用到路由

~~~
vue init webpack 项目名
~~~

安装elementui

下载依赖

在项目中执行

~~~
npm i element-ui -S
~~~

指定项目使用UI

在main.js

~~~js
// 引入elementui
import ElementUI from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';
// 让vue使用elementui
Vue.use(ElementUI);
~~~



还是改为用idea编辑了

删除helloworld组件

新建一个组件

elementui的所有组件的属性都写在组件的标签上。

相当于根据属性的值返回标签样式。