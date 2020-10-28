### ES6（ES6都增加了哪些）

- 关键字扩展

  - let和块级作用域

    -- **函数作用域**：在ES5中，JS的作用域分为全局作用域和局部作用域.通常是用函数区分的,函数内部属于局部作用域。

    ```javascript
    //ES6之前只有函数才能构成局部作用域
    //案例1
    {var a = 10;}
    console.log(a);
    
    //案例2
    console.log(a);
    if(true){
       var a = 10;
    }
    console.log(a);
    ```

    -- ES5只有全局作用域和局部作用域,没有块级作用域，带来很多不合理的场景

    - 内层变量可能会覆盖外层变量

      ```javascript
      var b = 1;
      fn1();
      function fn1() {
          console.log(b);//undefined
          if (true) {
              var b = 2;
          }
      }
      ```

    - 用来计数的循环变量泄露为全局变量

      ```javascript
      //变量i是var命令声明的,在全局范围内都有效,所以全局只有一个变量i,每一次循环，变量i的值都会发生改变
      for (var i = 0; i < 5; i++) {
          setTimeout(function () {
              console.log(i);//5 5 5 5 5
          })
      }
      ```

    -- **块级作用域**：-- 使用{}括起来的区域叫做块级作用域。

    ​                           -- let关键字声明变量,实际上是为JavaScript新增了块级作用域。

    ​                           -- 块作用域由{}包裹,if语句和for语句里面的{}也属于块作用域。

    ​                           -- 在块内使用let声明的变量,只会在当前的块内有效。

    ​                           -- 块级作用域可以任意嵌套（外层作用域无法读取内层作用域的变量）

            ```js
    //全局声明一个变量b
    let b = 1;
    fn1();
    function fn1() {
        //当前的作用于没有b，沿着作用于寻找到全局的b
        console.log(b);
        if (true) {
            //只在当前的作用于中生效
            let b = 2;
        }
    }
            ```

    --**let关键字**

       -- for循环的计数器,就很适合使用let命令

     ```js
    //计数器i只在for循环体内有效，在循环体外引用就会报错
    for (let i = 0; i < 4; i++) {
        console.log(i);
    }
    console.log(i);
     ```

       --**解析** :

    ​      1、变量i是let声明的,当前的i只在本轮循环有效,所以每一次循环的i其实都是一个新的变量（JavaScript引擎内部会记住上一轮循环的值,初始化本轮的变量i时,就在上一轮循环的基础上进行运算）     

    ​      2、for循环还有一个特别之处,就是设置循环变量的那部分是一个父作用域,而循环体内部是一个单独的子作用域。

    - **let关键字特点**

       1、没有声明提升

       2、不允许重复声明

       3、

  - const关键字

    --- 常量: 不会变化的数据,有些时候不允许修改,所以需要定义常量。

    ---**特点**

    ​     1、const声明一个只读的常量，定义常量可以写大写

         ```js
    const PI = "3.1415926";
    PI = 22;//Assignment to constant variable.(报错：给常量赋值)
         ```

    ​     2、const声明的常量如果是对象,可以修改对象的内容

    ```js
    // const只能保证这个指针是固定的（即总是指向另一个固定的地址），至于它指向的数据结构是不是可变的，就完全不能控制了
    //但是对象的引用不能被修改，比如用一个对象替代这个对象，那么引用就被修改了
    const PATH = {};
    PATH.name = "lily";
    console.log(PATH);//{name:"lily"}
    PATH = {}//Assignment to constant variable
    ```

    ​      3、const一旦声明变量,就必须立即初始化,不能留到以后赋值

    ```js
    const time;//SyntaxError: Missing initializer in const declaration
    ```

    ​     4、const只在声明所在的块级作用域内有效

    ```js
    {
        const PI = 3.14;
    }
    console.log(PI)//ReferenceError(引用错误)PI is not defined
    ```

    ​     5、const命令声明的常量也不提升

    ```js
    console.log(c);//Cannot access 'c' before initialization
    const c = "hello"
    ```

    ​     6、const 声明的常量,也与let一样不可重复声明

    **注意**：let命令,const命令,class命令声明的全局变量,不属于顶层对象的属性。也就是说,从ES6开始,全局变量将逐步与顶层对象的属性脱钩。

    ```js
    let a = 1;
    console.log(window.a);//1
    var a = 2;
    console.log(window.a)//undefined
    ```

  - 块级作用域的函数声明

     -- 函数声明一般常用的是两种，一是function声明,一种是函数表达式。

     ```js
    //函数声明 及 两种声明的区别
    f1()//可以使用
    f2();//f2 is not a function
    
    function f1() {}
    var f2 = function () {
    
    }
     ```

    函数能不能在块级作用域之中声明？这是一个相当令人混淆的问题。

    - ES5 规定，函数只能在顶层作用域和函数作用域之中声明，不能在块级作用域声明。
    - ES6 引入了块级作用域，明确允许在块级作用域之中声明函数。ES6 规定，块级作用域之中，函数声明语句的行为类似于`let`，在块级作用域之外不可引用。
    - ES6 在附录 B里面规定，浏览器的实现可以不遵守上面的规定，有自己的行为方式

     **注意**：考虑到环境导致的行为差异太大,应该避免在块级作用域内声明函数。如果确实需要,也   应该写成函数表达式,而不是函数声明语句。

    ```js
    //尽量避免以下写法
    fn1();
    if (true) {
        function fn1() {
            alert(1);
        }
    }
    fn1();
    
    //如果真的想在声明块级作用域函数，使用函数表达式
    if (true) {
        let fn1 = function () {
            alert(1);
        }
    }
    ```

- 变量的解构赋值

- 字符串扩展

- 数组扩展

- 函数扩展

- Math的扩展

- 对象的扩展

- 新增数据类型

- 新增数据结构

- iterator

- Gennerator

- class

- Promise

- 动态import
