###AJAX    

  #### 1、简介

- 概念

​       AJAX(全称Asynchronous JavaScript And XML),就是异步的JS和XML.

​       AJAX是现有标准的一种新方法，不是编程语言，可以在不刷新网页的情况下，和服务器交换数据并且更新部分页面内容，不需要任何插件，只需要浏览器允许运行JavaScript就可以。（ajax的出现是因为：前端常会有这样的需求，我们只要局部刷新，不需要整个页面刷新，从而吹生了这门技术)

- 原理

​       运行过程：

​          1.Browser浏览器通过事件触发方法，本地通过XMLHttpRequest对象,创建并且发送请求通过互联网到服务器;

​          2.Server服务器收到请求内容,响应请求,发送所需数据到浏览器

​          3.Browser浏览器通过XMLHttpRequest对象的onreadystatechange的方法收到请求的数据后，解析和渲染数据到页面中。

​         依赖的标准：

​          1.XMLHttpRequest对象,异步的与服务器交换数据

​          2.JavaScript/DOM,信息显示/交互

​          3.XML作为转换数据的格式（但是现在一般是依赖于JSON来转换数据的格式）

- 优点

     1.可以无需刷新页面而与服务器端进行通信维护数据。

     2.允许你根据用户事件来更新部分页面内容-

- 缺点

     1.没有浏览历史，不能回退

     2.存在跨域问题

     3.SEO不友好

  -----------------------------------------------------------------------------------------------------------------

#### 2、原生JS (依赖于XMLHttpResquest对象)

- GET请求---步骤

     1、创建XMLHttpRequest对象的实例化对象 

  ```
  var xhr = new XMLHttpRequest();
  ```

     2、打开路径,建立连接:使用`XMLHttpRequest`对象的`open` 方法

  ```
  xhr.open('GET','/userinfo?userID=${oUser.value}');

  //open方法的三个参数：请求方式，请求地址，同步or异步（true）
  //如果当前静态文件和请求的资源在同一个服务器下（比如都在http://127.0.0.1）,那么可以直接书写请求接口路径
  ```

     3、发送请求：使用`XMLHttpRequest`对象的`send` 方法

  ```
  xhr.send();
  //get请求，请求体在查询字符串中,所以send中不需要书写参数
  ```

     4、接收响应: 若ajax请求状态发生变化,会自动触发onreadystatechange事件

  ```
  xhr.onreadystatechange = function(){
      /*
         响应数据的接收:
              - xhr.responseXML 接收xml格式的响应数据
              - xhr.responseText 接收文本格式的响应数据
         xhr.readyState--(ajax的请求状态码):
              - 0 : 初始化状态
              - 1 : 代表open调用，但send方法还未调用（没有发送请求）
              - 2 : send方法调用，并接收了部分响应信息：响应首行和响应头                    （比如响应状态码）
              - 3 : 代表接受了部分响应体数据，（如果响应体数据较小就全部接                   受。但是数据如果比较大，就只接受一部分） 
              - 4 : 代表响应体接收完毕，请求结束
              
          判断请求成功，可以开始处理成功返回的响应数据：
             当xhr.readyState == 4 && (xhr.status >= 200 && xhr.status < 300)时表示成功            
      */
  }
  ```

- POST请求

    POST请求与GET请求步骤一样,下面的步骤有些区别

    3、发送请求

  ```
  xhr.send(body);//post请求会传参数
  /*
    注意的是:post请求不读取缓存,一定要写请求头
    参数分为两种:
      1、若body是查询字符串格式,则书写请求头
         xhr.setRequestHeader('Content-Type', 'application/x-www-form-            urlencoded');
         服务器需要使用中间件
         app.use(express.urlencoded(extended: true}))
      2、body如果是json字符串格式,则书写请求头
         xhr.setRequestHeader("content-type", "application/json")
         服务器需要使用中间件`app.use(express.json());
  */  
  ```

  #### 解决IE缓存问题

  ​    问题：在一些浏览器中(IE),由于缓存机制的存在,ajax的get请求只会发送第一次请求,剩余多次请求不会在发送给浏览器而是直接加载缓存中的数据。chrome/firfox执行协商缓存,IE走强制缓存

  ​    解决方式：浏览器的缓存是根据url地址来记录的，所以我们只需要修改url地址即可避免缓存问题

  ```
  //在地址后面拼接一个时间戳
  xhr.open("get","/testAJAX?t="+Date.now());
  ```

--------------------------------------------------------------------------------------------------------------

####  3、JQuery中的AJAX

- GET请求--1  $.get(url,[data],[callback],[type])

  ```
   /*   //使用jquery的ajax的get的请求
        $.ajax({
           type: 'GET',
           // get请求参数写法有三种
           //第一种，直接在地址后面拼接字符串
           // url: `/userinfo?userID=${$('#user').val()}`,
            // 第二种.使用data属性来写参数(data属性写也分两种，一个                            是字符串，一个是对象)
            url:'/userinfo',
            // 第二种
            // data:`userID=${$('#user').val()}`,
            //第三种
            data:{name:$('#user').val(),age:12},
            success(data) {
                 console.log(data);
               },
            error(err) {
                 console.log(err);
               }
         }) */
  ```

- GET请求--2  

  ```
   //jquery还封装了另外的get方法和post方法,进行ajax请求
        /*  $.get('/userinfo',{
                    name:$('#user').val(),
                    sex:'男',
                    age:12
                 },function(data){
                      //成功时,返回的类似success函数
                      console.log('成功的数据---',data)
                  }) */
  ```

- POST请求--1

  ```
   //使用jquery的ajax的post的请求
      /*   $.ajax({
               type: 'POST',
               url:'/userinfo',
             // post请求参数,一定要写请求头，有三种写法
             //post的请求头默认是
          // Content-Type: application/x-www-form-urlencoded;charset=UTF-8                                    
             //第一种，直接传urlencoded编码类型的字符串
             // data: `userID=${$('#user').val()}`,
             
             // 第二种,使用json字符串的形式,要设置请求头为json且charset=utf-8
             // headers:{
             //     "Content-Type": "application/json;charset=utf-8"
             // },
             // data:JSON.stringify({name:$('#user').val(),sex:'nan'}),
             //第三种,对象的形式
                data:{name:$('#user').val(),age:12},
                success(data) {
                      console.log(data);
                  },
                error(err) {
                      console.log(err);
                  }
            }) */
  ```

- POST请求--2

  ```
           $.post('/userinfo',{
                      name:$('#user').val(),
                      sex:'女',
                      age:90
                  },function(data){
                        //成功时,返回的类似success函数
                  })
  ```

---------------------------------------------------------------------------------------------------------------------------

####4、AXIOS的AJAX

- 1
- 2
- 3
- 4
- 5
- 6
- 7
- 8

-------------------------------------------------------------------------------------------------------------------------

####  5、跨域

```
W:为什么会出现跨域问题?
D:出于浏览器的同源策略限制,浏览器会拒绝跨域请求。
  *注:严格的说,浏览器并不是拒绝所有的跨域请求,实际上拒绝的是跨域的读操作.浏览器的同源策略是这样执行的:
     - 通常浏览器允许进行跨域写操作(Cross-origin writes),如链接,重定向;
     - 通常浏览器允许跨域资源嵌入(Cross-origin embedding),如img、script标签;
     - 通常浏览器不允许跨域读操作(Cross-origin reads)。*;
W:什么情况下才算作跨域?
D:非同源请求,均为跨域.----名词解释:同源----如果两个页面拥有相同的协议(protocol),端口(port)和主机(host),那么这两个页面就属于同一个源(origin)
W:为什么有跨域请求?
D:最常见的应用场景就是项目开发中请求第三方其他域下的资源。如果不处理跨域问题,浏览器就会发出警告
```

##### 解决跨域方法

- JSONP

  - JSONP(JSON with Padding),是一个非官方的跨域解决方案,纯粹凭借程序员的聪明才智开发出来的,只支持`get` 请求

  - [实现原理]：

     -- 通过`<script>`标签引入一个`js`文件，这个`js`文件载成功后会执行我们在`url`参数中指定的函数，并且会把我们需要的`json`数据作为参数传入。

  - [实现方式]：需要前后端配合

    - 前端(主要代码)

      ```
      <body>
          <button id="btn">发送</button>
          <script>
              const oBtn = document.getElementById("btn");
              oBtn.onclick = function () {
                  //jsonp的回调函数 一定是一个全局函数，否则jsonp的script标签执行的时候找不到这个函数
                  window.fn = function (data) {
                      console.log(data);
                  }
                  s
                  //script标签的src属性可以进行跨域请求，所以我们使用script标签的src进行请求
                  //创建一个script标签
                  const script = document.createElement("script");
                  //书写请求地址 把函数名发送过去 也可以发送其他的数据
                  script.src = "http://127.0.0.1:3000/user?name=lily&callback=fn&_t=" + Date.now();
                  
                  //插入页面时候，script就会执行
                  document.body.appendChild(script);
              }
          </script>
      </body>
      ```

    - 后端(主要代码)

      ```
      const express = require("express");
      //注册一个服务
      const app = express();

      app.get("/user", (req, res) => {
          console.log(req.query);
          //取到回调函数的名字
          const {
              callback
          } = req.query;
          //要发送给前端的数据
          const data = {
              name: "laowang",
              age: 16
          }
          //如果是script标签请求的，当我们返回一个字符串的话，script标签会把这个字符串换成代码解析
          // res.send("alert(1)");
          // 'fn({name:"laowang",age:16})'
          res.send(`${callback}(${JSON.stringify(data)})`)
      })

      //启动服务
      app.listen(3000, (err) => {
          if (err) {
              console.log("服务器启动错误" + err);
              return;
          }
          console.log("服务器启动成功 http://127.0.0.1:3000");
      })
      ```

  - JQuery的JSONP写法

    ```
    //服务器写法与上面相同
    //下面是前端写法
    <body>
        <button id="btn">发送</button>
        <script>
            const oBtn = document.getElementById("btn");
            oBtn.onclick = function () {
                //jQuery的jsonp跨域
                $.getJSON("http://127.0.0.1:3000/user?callback=?",                    function (data) {
                   //回调函数，请求时，JQuery会自动给函数命名
                    console.log(data);
                })
            }
        </script>
    </body>
    ```

  - JSONP的优缺点

    - 优点：兼容性好(兼容低版本IE)
    - 缺点：
      1. 只支持GET请求而不支持POST请求等其他类型的HTTP请求。
      2. 只支持跨域HTTP请求这种情况,不能解决不同域的两个页面或iframe之间进行数据通信的问题。
      3. JSONP从其他域中加载代码执行，如果该域不安全并且夹带一些恶意代码,会存在安全隐患,要确定JSONP请求是否失败并不容易。
      4. XMLHttpRequest相对于JSONP有着更好的错误处理机制


- CORS (服务端进行操作)

  - CROS（Cross-Origin Resource Sharing）是W3C推荐的一种新的官方方案,能使服务器支持XMLHttpRequest的跨域请求。

  - CORS是通过设置一个响应头来告诉浏览器，该请求允许跨域，浏览器收到该响应以后就会对响应放行。

  - 值得注意的是,使用CORS时,异步请求会被分为简单请求和非简单请求,非简单请求的区别是会先发一次预检请求。

  - [ 简单请求 ]：例子

    ```
    app.get("/user", (req, res) => {
        console.log(req.headers);
        //方式一：cors跨域，这个设置表示该资源可以被人以外域访问
        // res.set("Access-Control-Allow-Origin", "*")

        //方式二：设置白名单
        const safeUrl = ["http://127.0.0.1:5500", "http://baidu.com"];
        const userURL = req.headers.origin;//获取请求来源
        if (safeUrl.includes(userURL)) {
            res.set("Access-Control-Allow-Origin", userURL)

            /* 
                Access-Control-Allow-Credentials: true
                当请求是非get的时候，或者请求头有特殊字段的时候,浏览器会发送一个预检请求（请求方式是options）
                预检请求检查当前是否允许跨域，如果不行 则直接拒绝不在发送
                Access-Control-Allow-Headers：x-class0721-hello
                允许以上设置的请求头字段可以跨域
                Access-Control-Allow-Methods: GET, OPTIONS, HEAD, PUT, POST、
                允许跨域请求的方法
                Access-Control-Allow-Origin: https://juejin.im
                允许跨域请求的地址
                Access-Control-Max-Age: 1800
                预检请求的缓存时间
            */
        }

        res.send("");
    })
    ```

  - [ 非简单请求 ]：在真正请求前会发送预检请求

    #### 特点

       -- 不需要在客户端做任何特殊的操作，完全在服务器中进行处理，支持get和post请求。