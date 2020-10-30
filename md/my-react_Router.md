###React-router

```JS
-- 现在前端应用大多都是SPA(单页应用程序single page application),也就是只有一个HTML页面的应用程序。
**单页应用程序的好处**
   - 用户体验较好,对服务器的压力更小,所以更受欢迎。
**前端路由**
   - 为了有效的使用单个页面来管理原来多页面的功能,前端路由应运而生。
   - 功能: 实现一个页面导航到另一个页面(即实现A页面跳转到B页面)。
   - 前端路由是一套映射规则,在React中,是URL路径与组件的对应关系
```

- 基本使用

  - 安装包  npm i react-router-dom  (不用指明下载版本,生产环境也是要使用的)

  - 导入路由的三个核心组件

    ```js
    import {BrowserRouter as Router,Link,Route} from 'react-router-dom'
    ```

  - Router、Link、Route核心组件

    - Router -->作用:监听浏览器地址栏路径的变化,然后让页面所有的Route组件的属性path都和路径去进行匹配。

      **注意**： Router在应用中只写一次, 使用这个组件包裹根标签,或者包裹根标签的结构都可以,总之就只写一次。

    - Link --> 作用: 修改浏览器地址栏的路径, 但是不给服务器发送请求。**一般作为导航菜单(路由入口)** 

       **特点:** 相当于a标签,Link中的to属性相当于a标签的href, 区别就是：a标签会像服务器发送请求,Link内部应该是阻止了默认事件。

      ```js
      // to属性：浏览器地址栏中的pathname（location.pathname） 
      <Link to="/first">页面一</Link> 
      ```

    - Route --> 作用：当浏览器地址栏发生变化,Route组件会使用自己的path属性的值,和地址栏路径进行匹配,如果匹配成功,就将component属性对应的组件,渲染到页面上 。**配置路由规则和要展示的组件（路由出口）** 

        ```js
      // path属性：路由规则 
      // component属性：展示的组件  
      // Route组件写在哪，渲染出来的组件就展示在哪 
      <Route path="/first" component={First}></Route> 
        ```

  - 路由执行过程

    ```js
    (1) 点击Link组件(a标签),修改了浏览器地址栏中的url。
    (2) React路由中Router组件监听到地址栏url的变化。
    (3) React路由内部遍历所有的Route组件,使用路由规则(path),与pathname(地址栏地址)进行匹配。
    (4) 当路由规则(path)能够匹配地址栏中的pathname时,就展示该Route组件的内容。
    ```

  - 默认路由

     **问题:** 现在的路由是点击导航菜单显示的,如何在进入页面就默认显示某个页面呢？

     **解决:** 默认路由,就是表示进入页面就会匹配的路由。

    ```js
    //默认路由的path为'/'
    <Route path="/" component={Home} /> 
    <Route path='/home' component={Home}></Route>
    <Route path='/about' component={About}></Route>
    ```

  - 3

  - 4

  - 5

    ​

- 1

- 2

- 3

- 4

- 5