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

  - **路由执行过程** 

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

- ####路由匹配模式

    **注意（pathname就是地址栏上的地址（Link的to属性写的地址）,path就是Route组件的path属性写的地址）**

  - 模糊匹配 (React路由默认情况下是模糊匹配)

     -- **规则**: (1) pathname以path开头就会匹配成功。

    ​              (2) pathname和path想比,pathname至少和path相等.要么就再长一点. 并不是说字符串中包含就可以.应该是一级路径的情况下,完全相同   

  - 精确匹配

     -- 给Route组件添加**exact** 属性,让其变为精确匹配模式。

     -- **规则**: 只有当path和pathname完全一致时才能匹配成功。

- #### 路由的其他组件（了解）

  - Switch组件

     --Switch组件用于包裹Route组件,类似于js中的switch,有一个Route符合就不往下匹配了

    ```js
     //pathname是 /home
     //匹配第一个path /home 符合. 就不再匹配下面的Route了
    <Switch>
        <Route path='/home' component={Home}></Route>
        <Route path='/about' component={About}></Route>
        <Route path='/' component={Home}></Route>
    </Switch>
    ```

  - Redirect组件

    -- 重定向组件(渲染Redirect将使导航到一个新的地址,这个新的地址会覆盖 history 栈中的当前地址)

    ```js
    <Route path='/home' component={Home}></Route>
    <Route path='/about' component={About}></Route>
    <Redirect to='/home'></Redirect>
    或
    <Redirect
      to={{
        pathname: "/home",
      }}
    />
    ```

  - NavLink组件

    -- 和Link组件的功能是一样的.只是会给对应的a标签添加类名（类名为active）

    ```js
     <NavLink to='/about'>about</NavLink>  
    // 渲染后的a标签上面会有一个active类名(默认)
     <a href-="..." class="active"></a>
    ```

    -- NavLink 配合 activeClassName属性 指定类名

    ```js
    <NavLink to='/about' activeClassName="selected">about</NavLink>  
    // 渲染后的a标签上面会有一个selected类名
     <a href-="..." class="selected"></a>
    ```

- #### 编程式导航  

  **--被Route渲染的组件,通过props可以获取到三个属性,分别为history,location,match** 

  -- 功能:在js中通过执行一行代码实现页面跳转

  - history 对象(用于获取浏览器历史记录的相关信息)
    - push('路径',state) --- > push方法可以修改地址栏的路径,添加到历史记录中,state是用来在页面间传递数据的。
    -  replace('路径',state) --- > 可以修改地址栏的路径,替换当前历史记录中的路径,state是用来在页面间传递数据的。
  - location对象 (表示当前程序的位置)
    - location.pathname -->可以获取当前的pathname
    - location.state -->可以获取到push和replace方法中传递过来的第二个参数。
  - match对象 (包含匹配的url信息)
    - match.params -->可以获取**路由参数** 
    - match.path 可以获取匹配的path

- ##### 路由参数

  -- 路由参数是拼接在路径里面的参数 --->协议://域名:端口号/路径/路径/路由参数?查询字符串#hash值

  - 定义路由参数

    ```js
    //如果参数后面不加问号,表示这个参数,必填.如果加了问号,表示这个参数可选
    <Route path="/路径/路径/:参数?"  component={组件}/>
    ```

  - 传递路由参数

    ```js
    //方法1
    <Link  to="/路径/路径/参数"/>
    //方法2
    <NavLink to="路径/路径/参数"/>
    //方法3
    history.push/replace('/路径/路径/参数')
    ```

  - 接收路由参数

    ```js
    this.props.match.params
    ```

- ####withRouter高阶组件

  - **目的/功能**-->withRouter可以帮助我们实现,在非Route渲染的组件中获取到history,location和match对象。(默认只有被Route组件渲染的组件,才能通过props.获取到history,location以及match对象)

  - **使用方法** 

    ```js
    //1导入
     import {withRouter} from 'react-router-dom'
    //2创建
     const WithXXXX = withRouter(组件)
    //3使用
    -- 直接在使用withRouter包裹的组件的地方使用WithXXX即可
    ```

- ​