  ### ES6

​      -- ES6内置了模块化的实现

​      -- 暴露和引入的方法

* 方法一 

  - 分别暴露

    ```js
    export let bl = '暴露一个变量'
    export  function foo(){console.log('暴露一个函数')}
    ```

  - 引入

    ```js
     import {bl,foo} from '模块路径/模块名'
    ```

* 方法二

  - 暴露多个

    ```js
     var xxx = value1
     let yyy = value2
     export {xxx, yyy}
    ```

  - 引入

    ```js
    import {xxx,yyy} from '模块路径/模块名'
    ```

* 方法三

  - 暴露一个默认的模块（一个文件中只能暴露一个）

    ```js
    export default {msg:'默认暴露一个对象'}
    ```

  - 引入

    ```js
    import XXX from '模块路径/模块名'
    // 备注：XXX是我们自定义的名字
    ```

  ​

  -- ES6的格式，浏览器也是不支持的

  - 两种步骤解决
    - （使用Babel）首先将ES6--->ES5,这是代码是CommonJS的规范，浏览器也是不支持的
    -  (使用Browserify)再将ES5(CommonJS格式的)--->浏览器可执行的

  ​

  -- 依赖的包

  ```js
  (1) npm init -y 初始化
  (2) 安装babel-cli, babel-preset-es2015和browserify
        * npm install babel-cli browserify -g （babel-cli相当于babel的秘书/经纪人，要想使用babel需要先下载babel-cli（联系秘书/经纪人）,在使用）
        * npm install babel-preset-es2015 --save-dev （preset 预设(将es6转换成es5的所有插件打包)）
   (3) 定义一个文件.babelrc文件（rc就是runtime control）,在当前项目根目录(babel的配置文件)
  ```

  ​

  -- 最后进行编译

  ```js
   (1) 使用Babel将ES6编译为ES5代码(但包含CommonJS语法) : babel js/src -d js/lib
        解析：
           js/src --- ES6标准的目录文件
           -d     --- dir 文件夹
           js/lib --- 转换为ES5后的目录文件
           
   (2) 使用Browserify编译js : browserify js/lib/app.js -o js/lib/bundle.js
  ```

  ​

   -- 引入第三方模块

  ```js
   1). 下载jQuery模块: 
        - npm install jquery@1 --save
   2). 在app.js中引入并使用
          import $ from 'jquery'
          $('body').css('background', 'red')
  ```

  ​

  