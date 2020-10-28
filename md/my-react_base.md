### React 

   - 是一个用于构建用户界面的javaScript库

   - 特点：
      * 声明式(jsx)，React.createElement() 是命令式
      * 组件化
        *

    - 基本使用

      - 创建一个id为root的根标签`<div id='root'></div>`

      - 引入react构建页面所依赖的js包

        - 先引入react（react的核心api）, 后引react-dom(操作dom的api)。

      - 命令式创建react元素（不是真正的dom元素,是React元素(虚拟dom)）

        ```js
        /* 命令式创建react元素
        第一个参数:标签名
        第二个参数:是设置你创建的元素的属性，比较特殊的属性class和label标签的for属性应该写成className和htmlFor,如果没有属性要使用null占位
        第三个参数及后面的:子节点
        */
        // const title = React.createElement('h1',null,'hello React');
        const title = React.createElement('h1',{className:'box'},'hello React');
        //渲染到页面（将React元素转成真正的dom元素，然后渲染到页面上）
        //ReactDOM.render(react元素/组件,渲染的位置)
        ReactDOM.render(title,document.querySelector('#root'));
        ```

    - #### JSX

       -- JSX是JavaScript XML,是React提供的Syntax Sugar(语法糖),能让我们可以在JS中写html标记语言。

​       -- 浏览器并不认识jsx,所以需要引入babel(@babel/preset-react)将jsx编译成 React.createElement的形式

- **基本使用**

​       -- 引入babel.min.js包

​       -- js代码写入的script中的type属性值应为text/babel,被babel编译后的代码是严格模式下的。

```html
 <div id="root"></div>
    <script src="./js/react.development.js"></script>
    <script src="./js/react-dom.development.js"></script>
    <script src="./js/babel.min.js"></script>
    <script type="text/babel">
    //创建元素
    const h1 = <h1> 我是jsx创建的</h1>
    //渲染
    ReactDOM.render(h1,document.querySelector('#root'));
 
    </script>
```

- **插入数据** 

  -- 如果要在jsx中插入数据,可以直接在要插入数据的地方写{}(插值表达式)

   -- 插值表达式中,可以写任意类型的表达式

  -  ** 常见的表达式：字面量,变量,常量,数组,函数,对象,运算,三元,注意不能写if和循环语句

  -- 如果要给属性设置值，直接将值写成{}

     **注意的是: 插值表达式中,可以传入对象和函数,但是不能直接去渲染，如果渲染的时候,插值表达式写了对象和函数,就会报错。因为react不知道如何将对象和函数渲染到页面上,如果要写对象,应该是一个react元素**

  ```js
         //插入数据
          let data = 'giaogiaogiao'
          let data1 = {name:'lili'}

          //会报错,{}中写了对象，react不知道怎么去渲染
          const p = <p style={{color:'red'}}>{data1}</p>
          ReactDOM.render(p,document.querySelector('#root'));

           //这种写法可以,插入一个react元素
          /* const h2 = <h1 title={'我是h1标签'}>{<span>123</span>}</h1>
          ReactDOM.render(h2,document.querySelector('#root')); */
  ```

- **条件渲染** 

  ```js
    let age = 17
              // 工作中经常使用与逻辑实现条件判断
            // 注意: 如果是有条数据,不是渲染a,就是渲染b的时候,应该使用if或三元
            // 还有一些情况是,有一条数据,要么是渲染,要么就不渲染

              // 需求: 如果大于18岁,展示酒的信息
              let arr = ['红酒', '黄酒', '白酒', '啤酒', '鸡尾酒']
              const h1 = <h1>{age >= 18 && arr}</h1>

              ReactDOM.render(h1, document.getElementById('root'))
  ```

- **列表渲染** 

  -- react列表的渲染,都是使用数组的map方法,得到一个包含了jsx标签的数组,直接渲染这个数组实现

   -- 在执行map的过程中,最外表的那个标签必须要加一个key属性,属性的值要求是唯一的。

  ```js
  let arr = [{
                  name: '呸hua',
                  age: 38,
                  gender: '不详细',
                  hobby: '抽烟喝酒植发'
              },{ name: '静哥',
                  age:48,
                  gender: '男',
                  hobby: '京城会所'}]
               //可以渲染的形式,要是数组中的每个元素都是jsx标签
              // let arr1 = [<li>Peihua</li>, <li>jg</li>]

              const arr1 = arr.map((item,index)=>{
                  return <li key={index}><div>名字: {item.name}</div><div>年龄: {item.age}</div><div>性别: {item.gender}</div><div>爱好: {item.hobby}</div></li>
              })

              let ul = <ul>{
                 arr1
              }</ul>

              ReactDOM.render(ul, document.getElementById('root'))
  ```

- **样式处理** 

  -- 行内样式 : 若样式是数值,可以省略单位。

  ```js
  <div style={ { color: 'red', fontSize: 30 } }>web</div>
  ```

   -- **类名(推荐使用)**

  ```js
   <div className="abc">web</div>
  ```

  **注意的是**

    -- React元素的属性名使用小驼峰命名法

    -- 没有子节点的React元素可以写成自闭式和标签形式

- **事件处理** 

  -- React中的事件对象叫做：合成事件(兼容所有浏览器,无需担心跨浏览器兼容性问题)

  --**注意的是**

  ​    ** react-jsx中注册事件,事件名和dom中是一样的.只是需要写成小驼峰命名法。

  ​    ** react中事件处理函数不能使用return false阻止默认事件行为,需要使用事件对象的                                          preventDefault()实现。

  ​    ** 如果在控制台打印事件对象,属性值都是null，如果一定要看的话,调用事件对象.presist()方法。

  ```js
  <script type="text/babel">
          let link = <a href='' onClick={(e)=>{
              console.log('被点击了');
               
              // 阻止默认事件, 在react中不能使用return false阻止默认事件
              e.preventDefault();
              //注意的是：默认在控制台，打印事件对象，看不到属性的值，但是可以通过代码获取到，如果非要在控制台看，调用persist（这个函数就可以） 
              e.persist();
          }}>点击我</a>

          //渲染
          ReactDOM.render(link,document.querySelector('#root'))

          </script>
  ```

- React案例

  ```js
  <script type='text/babel'>
                /* 
                  实现评论列表功能

                  - 如果有评论数据，就展示列表结构 li（ 列表渲染 ）要包含a标签
                  - name 表示评论人，渲染 h3
                  - content 表示评论内容，渲染 p
                  - 如果没有评论数据，就展示一个 h1 标签，内容为： 暂无评论！
                  - 根据自己的喜好添加样式
                  - 给a标签注册点击事件, 打印内容

                */
                const list = [{
                        id: 1,
                        name: 'jack',
                        content: 'rose, you jump i jump'
                    },
                    {
                        id: 2,
                        name: 'rose',
                        content: 'jack, you see you, one day day'
                    },
                    {
                        id: 3,
                        name: 'tom',
                        content: 'jack,。。。。。'
                    }
                ]
                // const list = []

                const arr = list.map(item=>{
                    return <li key={item.id}><a href='' onClick={(e)=>{
                        //阻止默认事件
                        e.preventDefault();
                        // console.log(item.content)
                        //e.target事件目标,点击那里就获取到对应的对象
                        console.log(e.target.innerText)
                    }}><h3>{item.name}</h3><p>{item.content}</p></a></li>

                })
              
                  //1. 完成列表渲染
                  let ul = <ul>
                   {arr}
                </ul>
                //条件判断
                const data = arr.length > 0 ?ul:<h1>暂无评论</h1>
                //渲染
                ReactDOM.render(data,document.querySelector('#root'))
            </script>
  ```

- #### React的组件 

   -- 组件允许你将UI拆分为独立可复用的代码片段,并对每个片段进行独立构思.

   -- 组件从概念上类似于JavaScript函数。它接受任意的入参,并返回用于描述页面展示内容的React元素。

  **一个React应用就是由一个个的组件组成的** 

  - **创建方式** 

    - 函数组件(不能定义状态)

       **注意** ：

       --- 组件名首字母必须大写,因为react以此来区分组件和react元素。

       --- 必须要有返回值,返回的内容就是组件呈现的结构,如果返回值为null,表示不渲染任何内容。

       --- 组件内部如果有多个标签,必须使用一个根标签包裹,只能有一个根标签。 

      ```js
         function Header(){
                return <div>我是函数组件</div>
             }
             //渲染
             ReactDOM.render(<Header></Header>,document.getElementById('root'))
      ```

    - 类组件(可以定义状态 , 状态: 组件内部私有的数据)

       **注意** :

        --- 组件名首字母大写必须大写,因为react以此来区分组件和react元素。

        --- 类组件中必须要声明一个render函数。

        --- render函数中必须有返回值。

        --- 组件内部如果有多个标签,必须使用一个根标签包裹.只能有一个根标签。

        --- 类组件应该继承 React.Component 父类，从而可以使用父类中提供的方法或属性 。

      ```js
      class Header extends React.Component{
              render(){
                   return <div>我是类组件</div>
              }
            }

            //如果组件没有子节点,也可以写成自闭合标签
            ReactDOM.render(<Header/>,document.getElementById('root'))
      ```

      ​

  - **组件的状态State** 

     --  函数组件又叫做无状态组件(一般只负责渲染静态结构),类组件又叫做有状态组件(负责更新UI,让页面“动”起来)。

    - 基本使用

      -- 状态(state) 即数据, 是组件内部的私有数据, 只能在组件内部使用。

       -- state的值是对象, 表示一个组件可以有多个数据。

    - **获取** ：this.state

      ```js
          class Hello extends React.Component{
                    constructor(){
                      super()
                      this.state = {count=0}//初始化一个状态state
                    }
                 
                    render(){
                       return <div>{this.state.count}</div>
                     }
                }
      ```

    - 操作State

      -- this.setState({要修改的数据})

      ​    **setState的作用是: 1|修改state的值, 2|并更新UI**

      -- **注意** :不要直接修改state中的值, 应该使用组件实例的setState方法, 修改state的值

      ```js
         class Hello extends React.Component{
                     constructor(){
                       super()
                       this.state= {count:1}
                     }
                     
                     render(){
                     
                       return <div onClick={()={
                         this.setState({
                           count:this.state.count + 1
                         })
                       }}>{this.state.count}</div>
                     }
                }
      ```

  - **事件绑定的this指向问题**

     - **出现问题的原因** : 为了提高代码的阅读性,我们推荐把事件处理函数定义在结构的外面,但是这种情况下带来了this指向的问题

       ```js
       问题代码:
       class Hello extends React.Component {
           constructor() {
               super() 
               this.state = { count: 0, num: 100 }// 初始化state
           }

          //定义的普通函数,(babel编译jsx,采用的是严格模式,普通函数this就指向undefined)
           handle() {
               //这里this指向undefined
               this.setState({
                   count: this.state.count + 1
               })
           }

           render() {
             //这边直接将this.handle函数作为了事件函数
               return <div onClick={this.handle}>{this.state.count}</div>
           }
       }

       ```

     - 解决方法1 (解决this的指向)----箭头函数

       ```js
       class Hello extends React.Component {
           constructor() {
               super() 
               this.state = { count: 0, num: 100 }// 初始化state
           }

           handle() {
               //这里this指向类组件实例对象
               this.setState({
                   count: this.state.count + 1
               })
           }

           render() {
             //此时将箭头函数作为了事件函数,此时的handle是通过this类对象调用的，故handle中的this指向类实例对象
             return <div onClick={()=>{
                                  this.handle();
                                 }}>{this.state.count}</div>
           }
       }
       ```

     - 解决方法2 (解决this指向) ----bind

       ```js
       class Hello extends React.Component {
           constructor() {
               super() 
               this.state = { count: 0, num: 100 }// 初始化state
             //使用bind方法,利用bind的特性，赋值运算先算右边的，就是说将this.handle函数的this改为指向实例化对象,将返回的函数赋值给当前实例对象上的handle属性
               this.handle = this.handle.bind(this)
           }

        
           handle() {
               //通过bind方法改变了this指向,此时这里面的this指向组件实例
               this.setState({
                   count: this.state.count + 1
               })
           }

           render() {
             //这边直接将this.handle函数作为了事件函数
               return <div onClick={this.handle}>{this.state.count}</div>
           }
       }

       ```

     - 解决方法3 (解决this指向) ---- 类的实例方法

       ```js
       class Hello extends React.Component {
          // es7草案中,提案,如果要给当前类实例添加属性,就不需要写constructor了.应该使用下面的语法
           state = {
              count: 1
            }
          // 这样定义函数.这个函数,直接添加到了当前组件的实例身上
         // 注意:虽然是es7草案的语法.但是因为脚手架中使用了babel.所以可以放心使用
           handle = () => {
               //这是箭头函数本身没有this,所以会依次向父级函数寻找,此时这里this指向组件实例
               this.setState({
                   count: this.state.count + 1
               })
           }

           render() {
               return <div onClick={this.handle}>{this.state.count}</div>
           }
       }
       ```

  - **组件的props** 

     --- 组件是封闭的,要接收外部数据应该通过props来实现

    - **作用** : 接收外部传递给组件的数据

    - **传递数据方式** :给组件标签添加属性

    - **接收数据方式** : 函数组件通过props接收数据,类组件通过this.props接收数据

      ```js
      //在其他的组件中,使用了Header
      <Header name='jack' age={19} />

      //函数组件
      function Hello(props) { 
        console.log(props) 
        return ( 
          <div>接收到数据：{props.name}</div> 
        ) 
      } 

      //类组件
      class Hello extends React.Component { 
        render() { 
          return ( 
            <div>接收到的数据：{this.props.age}</div> 
          ) 
        } 
      } 
      ```

    - **props特点** 

      - **props是只读的对象,只能读取属性的值,不能修改props(修改报错)**。(主要记住)

      - 可以给组件传递任意类型的数据

      - **使用类组件,若写了constructor,应该将props传递给super(), 否则,无法在构造函数中获取到props** 

        ```js
        class Hello extends React.Component{
           constructor(props){
             //推荐将props传递给父类构造函数
             super(props)
           }
           
           render(){
             return <div>{this.props.age}</div>
           }
        }
        ```

    - **props校验** (函数组件和类组件都有校验规则)

      - **作用** : 使因为props导致的错误,可以给出明确的错误提示,增加组件的健壮性。

      - **实现方式**

        -- 导入prop-types包

        -- 给组件添加静态属性propTypes来给组件的props添加校验规则

        ```js
        /*常见类型：array、bool、func、number、object、string还有element(react元素)
          也会传组件*/
        // 必填项：isRequired 
        // 特定结构的对象：shape({  }) 

        //1、导包
        import PropTypes from 'prop-types'
        class Header extends React.Component{
          render(){
            return <div>{'测试props校验'}</div>
          }
        }
        //2、添加校验规则
        Header.propTypes = {
          //属性名:属性值得类型
          name:PropTypes.string ,// name属性,值为字符串类型
          list: PropTypes.array.isRequired,//传过来的list是一个数组且必填
          gender: PropTypes.oneOf(['男', '女']),//传过来的gender在两个中选择其中一个值
          obj: PropTypes.shape({
                color: PropTypes.string.isRequired,//这个参数为必传
                fontSize: PropTypes.number
              })//传进来一个特定结构的数据
          
        }
        ```

      - **props默认值** 

        -- 作用：给props设置默认值, 在未传入props时生效

        ```js
        //添加默认属性
        Header.defaultProps = {
            name:'lili'//这边定义默认属性,若有传值，则会覆盖这个属性，没有传值则显示这个属性，通过也可以通过props校验
        }
        ```

- ####表单处理

  - **受控组件(重要)** 

    -- js中的表单项的值被组件中的状态所控制,我们就叫受控组件。

    - 实现方式

      -- 在state中添加一个状态,作为表单元素的value值(有时复选框的checked也会依赖于state中的某个值)。(**控制表单元素值的来源**) 

      -- 给表单元素绑定change事件, 将表单元素的值 设置为state的值(**控制表单元素值得变化**)s

      ```js
      补充: 文本框、文本域、下拉框 操作value属性, 复选框 操作checked属性
      //类组件
      import React from 'react';

      export default class Header extends React.Component {
      	state = {
      		text: '',
      		area: '',
      		select: '',
      		checkbox: false,
      		radio: ''
      	};

       //柯里化函数实现

      柯里化（Currying），又称部分求值（Partial Evaluation），是把接受多个参数的函数变换成接受一个单一参数（最初函数的第一个参数）的函数，并且返回接受余下的参数而且返回结果的新函数的技术。
      	//这些方法的处理都是类似的，可以写成一个函数
        	handle = (name)=>{
        //接收传过来的参数,用于修改state中的值，定义时调用了这个函数，一般事件函数是要在触发后执行
        //这边要return一个函数作为事件函数
                return (e)=>{
        			const value = name ==='checkbox'?!this.state.checkbox:e.target.value;
        		    this.setState({
        			  [name]: value
        		    });

        		}
        	}
        /* 	//输入框
        	handleChange = (e) => {
        		//    console.log(e.target.value)
        		const value = e.target.value;
        		this.setState({
        			text: value
        		});
        	};
        	//文本框
        	handleArea = (e) => {
        		const value = e.target.value;
        		this.setState({
        			area: value
        		});
        		// console.log(value)
        	};

        	//下拉框
        	handleSelect = (e) => {
        		const value = e.target.value;
        		// console.log(value)
        		this.setState({
        			select: value
        		});
        	};
        	//复选框
        	handleCheckbox = (e) => {
        		let { checkbox } = this.state;
        		console.log(checkbox);
        		this.setState({
        			checkbox: !checkbox
        		});
        	};

        	//单选框
        	handleRadio = (e) => {
        		const value = e.target.value;
        		// console.log(value)
        		this.setState({
        			radio: value
        		});
        	}; */

        	render() {
        		//接收函数组件传过来的数据
        		// console.log(this.props.msg)
        		return (
        			<div>
        				{/* 输入框 */}
        				<input type="text" value={this.state.text} onChange={this.handle('text')} />
        				<br />
        				<br />
        				{/* 文本框 */}
        				<textarea value={this.state.area} onChange={this.handle('area')} />
        				<br />
        				<br />
        				{/* 下拉框 */}
        				<select value={this.state.select} onChange={this.handle('select')}>
        					<option value="1">吃饭</option>
        					<option value="2">睡觉</option>
        					<option value="3">打麻将</option>
        					<option value="4">喝酒</option>
        					<option value="5">抽烟</option>
        					<option value="6">推牌九</option>
        				</select>

        				<br />
        				<br />
        				{/* 复选框 */}
        				<label>
        					<input type="checkbox" checked={this.state.checkbox} onChange={this.handle('checkbox')} />
        				</label>

        				{/* 单选框 */}
        				<input type="radio" name="gender" onChange={this.handle('radio')} value="男" />
        				<input type="radio" name="gender" onChange={this.handle('radio')} value="女" />
        				<div>展示单选的值--------{this.state.radio}</div>
        			</div>
        		);
        	}
        }
      ```


  - 非受控组件(非受控组件直接操作dom)

     -- 借助于ref(ref的作用：获取DOM或组件), 使用原生DOM方式来获取表单元素值。

    - 实现方式

      -- 1、调用React.createRef() 方法创建一个ref对象

      ```js
      constructor() { 
        super() 
        //创建一个ref对象，存放在当前组件实例对象上
        this.txtRef = React.createRef() 
      }
      ```

      -- 2、将创建好的ref对象添加到文本框中

      ```js
      //将react元素与ref进行关联
      <input type="text" ref={this.txtRef} />
      ```

      -- 3、通过ref对象获取到文本框的值

      ```js
      //在哪里使用，就直接通过this.textRef.current获取当前dom,在通过value获取值
      Console.log(this.txtRef.current.value)
      ```

-  ####组件的生命周期

     [生命周期图谱](https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/)

     - **三大阶段**

       - 挂载阶段(创建阶段)（constructor => render => componentDidMount）

         - constructor : 创建组件时,最先执行,一般用于1|初始化state,2|为事件处理程序绑定this(bind)
         - render：(返回结构),组件渲染时都会触发。**注意：不能在render中调用setState(),容易出现死循环**
         - componentDidMount : 组件挂载(即组件创建完成),完成DOM渲染后。一般用于：1|发送网络请求(订阅pubsub),2|DOM操作

       - 更新阶段（render => componentDidUpdate）

         - render(props修改或者执行setState,都会导致更新)

         - componentDidUpdate : 组件更新(完成DOM渲染后),有两个参数,1|发送网络请求，2|DOM操作.**注意** : 如果一定要在这里面写setState(),就一定要有个if条件，出口。

           ```js
             componentDidUpdate(preProps,preState){
                   //更新阶段有两个参数表示的上一次的props和state的值
                   console.log(preProps,preState)
                   console.log('Test------state更新啦~~~')
               }
           ```

       - 卸载阶段（componentDidUnMount）

         - componentDidUnMount : 组件卸载(从页面上消失,就是页面上没有这个组件,跟display和visitibled不一样),执行(清除操作-->清除定时器,取消订阅)


- ####组件通讯

  --`react组件通讯有三种方式 :props,context,pubsub  `

  - props (单向数据流 (由上自下): 父传子)  App -> Far -> Son

  - context (单向数据流 (由上自下),一般传递比较简单的数据() : 跨级传递(爷爷可以直接传递给孙子(可以隔很多代)) )

    - 实现方法一

      ```js
      //通过案例说明
      APP.js,Far.js,Son.js,APP要给Son传递数据（加入这三个文件在一个目录）
      //1、创建context对象解构出Provider(提供数据),Consumer(消费数据)两个组件
      //但是因为APP传数据用到Provider,Son接收数据用到Consumer,是在不同的页面，但是两个页面用到的context对象要是一个,所以需要另外创建context.js来写创建context对象的代码
      ---------------------------------------------------------------------
      context.js
        import React from 'react'
        export default React.createContext()
      //注意的是：创建context对象的时候可以写默认值,如下
       //React.createContext('默认值') 
      // 注意: 默认值是在没有提供provider的时候生效,而不是没有写value的时候生效 
      ---------------------------------------------------------------------
      App.js
        import React from 'react'
        import Son from './son'
        import context from './context'//获取创建的context对象
        const {Provider,Consumer} =  context
        export default class App extends React.Component{
          render(){
              return (
                /*value值传的就是App需要给Son传递的数据*/
                 <Provider value='123'>
                  <div>
                      <Far/>
                  </div>
                 </Provider>
              )    
          }
       }
       ---------------------------------------------------------------------
        Son.js
          import React from 'react'
          import context from './context'//获取创建的context对象
          const {Provider,Consumer} =  context
          export default class Son extends Component {
       
               render() {
                    return           
                      //方法一的写法,有点麻烦,Consumer标签中是一个箭头函数，箭头函数返回的是son中react元素，此时data形参就是App传过来的值
                      <Consumer>{data=> <div>
                           Son组件,{data}
                         </div>} </Consumer> 
                   }
              }
      ```

    - 实现方法二

      -- 实现方法二与上面在接收数据的时候会有些差别主要是Son.js文件的差别

      ```js
       Son.js
          import React from 'react'
          import context from './context'//获取创建的context对象
          export default class Son extends Component {
            //方式二:在组件内部定义一个静态属性为导入进来的context对象
            //获取数据，直接在需要获取数据的地方通过this.context获取即可
            static contextType = context
               render() {
                    return (
                  <div>
                      //在这边获取App传过来的数据
                      Son组件{this.context}
                  </div>) 
                   }
              }
      ```

  - pubsub (发布订阅机制) : 是一个用JavaScript编写的库

     -- **利用发布订阅模式** ：比如说A发布了一个公众号,B,C,D都关注/订阅了,那么A要有内容变化/更新,会推送到公众号上,此时BCD由于关注/订阅了,就会收到信息数据。同时A不仅可以发布信息,也有权去关注/订阅其他人发布的公众号,有数据更新,A同样也会收到。若B取消关注A,那之后A的更新B就收不到了。B不仅可以取消关注A,也可以将自己所有关注/订阅的都取消掉。

    - 实现方式

      - 安装(需要下载依赖包)

        ```js
        在项目根目录下: npm install pubsub-js / yarn add pubsub-js
        ```

      - 引入

        ```js
        import PubSub from "pubsub-js" // 导入的PubSub是一个对象.提供了发布/订阅的功能
        ```

      - pubsubjs提供的方法

        ```js
        //下面的this指的是组件实例,因为token与页面渲染无关，所以不需要写在state中

        //发布信息 
        PubSub.publish('topic(话题)','hello Son(发布的数据)')

        //订阅信息--一般是在组件挂载成功后进行订阅，返回的token相当于标识,取消订阅时可以用到
        this.token = PubSub.subscribe('topic(话题)',(msg,data)=>{
          //是一个回调函数，当订阅的话题,发布信息后,回调函数被触发
          console.log(msg(话题),data(数据))
        })

        //取消订阅 --- 取消指定的订阅
        PubSub.unsubscribe(this.token)

        //取消话题的所有订阅
        PubSub.unsubscribe('topic');

        //****下面是不怎么用的,或者是慎用的****
        // 发布的同步使用方法
        // 慎用!!!! 因为会阻塞发布者的代码执行
        PubSub.publishSync(TOPIC, 'hello world!');

        // 取消所有订阅
        PubSub.clearAllSubscriptions();

        ```


- #### Fragment（包裹结构,但不会渲染根标签）

   -- react组件中只能有一个根组件,之前使用div包裹的方式会给html结构增加很多无用的层级,为了解决这个问题,可以使用React.Fragment


     ```js
     //以前的页面
     function Hello(){
         return (
           // 渲染到页面之后,这个div就是一个多余的
           <div>
             <h1>fragment</h1>
             <p>hello react</p>
           </div>
         ) 
     }

     //通过React.Fragment
     function Hello(){
         return (
           // 这样就只会渲染h1和p
           <React.Fragment>
             <h1>fragment</h1>
             <p>hello react</p>
           </React.Fragment>
         ) 
     }

     //上面的React.Fragment写法麻烦，下面是简写
     function Hello(){
         return (
           // 这是React.Fragment的简写形式
           <>
             <h1>fragment</h1>
             <p>hello react</p>
           </>
         ) 
     }
     ```

- ####React性能优化

     - 方式一(减轻state)
       - 减轻state. 跟视图渲染无关的数据,不要写在state里面, 可以直接加到this(组件实例)上面(比如定时器的id),**可以减少页面重新渲染次数** 

     - 方式二(shouldComponentUpdate -- 减轻不必要的重新渲染)(**只有在更新阶段才会调用**) 

       - **组件的更新机制** : 父组件重新渲染时,也会重新渲染子组件

       - **问题 : 为了避免不必要的重新渲染** --->**解决方式** :   使用钩子函数  shouldComponentUpdate(nextProps, nextState) , 可以通过这个钩子函数的返回值决定是否重新渲染,返回true表示重新渲染,false表示不重新渲染。

       - 钩子函数的**触发时机**: 更新阶段的钩子函数，组件重新渲染前执行 （shouldComponentUpdate => render） 

         ```js
         //直接在组件中写shouldComponentUpdate函数
         import React, { Component } from 'react'
         export default class Son extends Component {
         //此钩子函数只有在props或者state更新时才会被触发
         //钩子函数有两个参数，第一个参数是最新的props,第二个参数是最新的state
         shouldComponentUpdate(nextProps,nextState){
            state = {
             msg: 'xxx',
              //我们在修改数据时,不要在原先的基础上去改,尤其是基本数据类型以外的值
             obj: {
               name: 'zs'
             }
           }
           //在这边比较props或者state是否有变化
           //对比了props和state中的每条数据是否有变化，其中有一条变化都返回true
           if (nextProps.count !== this.props.count ||
                  nextState.msg !== this.state.msg) {
                  return true //返回true后，会继续执行render
                }
               return false//返回false，不会向下执行render
           }
           
            render() {
             console.log('son组件更新了')
             return (
               <>
                 <h1>{this.state.obj.name}</h1>
                 <div>Son组件,{this.props.count}</div>
                 <div>{this.state.msg}</div>
               </>
             )
           }
           
           
         }
         ```

       - **钩子函数(shouldComponentUpdate)有个缺点**：如果props和state中有很多的数据,就要在函数中挨个判断,会比较麻烦。**解决方式**-->使用纯组件(PureComponent).纯组件内部实现了shouldComponentUpdate ,我们直接使用纯组件就可以,省去了使用钩子函数也不需要挨个判断。

     - 方式三(pureComponet -- 纯组件)

       - PureComponent 与 React.Component功能是类似的,可以直接替换(就是原先我们创建的组件都是继承的React.Component,现在直接继承PureComponent即可)

         - **PureComponent 与 React.Component区别**： PureComponent 内部自动实现了 shouldComponentUpdate 钩子，不需要手动比较 

       - **PureComponent 原理**: 纯组件内部通过分别 对比 前后两次 props 和 state 的值，来决定是否重新渲染组件 

       - **值得注意的是**: 纯组件内部的对比是 shallow compare（浅层对比）,如果比较基本数据类型,比较的是值。如果比较复杂数据类型,比较的是地址值。**所以**, state 或 props 中属性值为引用类型时，应该创建新数据，不要直接修改原数据(基本类型时也尽量不要修改原数据)

         ```js
         import React, { Component, PureComponent } from 'react'

         // 纯组件的功能和component的功能几乎是一样,但是,纯组件将shouldComponentUpdate增强了,自动判断当前的props和state的值是否发生了变化
         // 注意: 纯组件,在底层对比state和props的时候,使用的是浅层对比
         // 如果比较基本数据类型,比较的是值
         // 如果比较复杂数据类型,比较的是地址值
         export default class Son extends PureComponent {
           state = {
             msg: 'xxx',
             obj: {
               name: 'zs'
             }
           }
           render() {
             console.log('son组件更新了')
             return (
               <>
                 <h1>{this.state.obj.name}</h1>
                 <div>Son组件,{this.props.count}</div>
                 <div>{this.state.msg}</div>
               </>
             )
           }
         }
         ```

- ####提供复用代码的逻辑

     - ####高阶组件(HOC higherOrderComponent)

       -- 高阶组件（HOC）是 React 中用于复用组件逻辑的一种**高级技巧**。HOC 自身不是 React API 的一部分，它是一种**基于 React 的组合特性而形成的设计模式** (**简单理解的话:** 一个拥有复用逻辑的函数,这个函数需要传入一个组件,然后返回一个增强的组件,并渲染这个增强组件)

       - 具体实现

         ```js
         //案例说明:登录注册页面(有相同的逻辑，获取用户名和密码的值)
         -----------------------------------------------------------------
         //复用逻辑的js，是一个函数,接收参数,并返回一个组件(组件中渲染的是传进来参数对应的组件)
         withForm.js
         import React from 'react'
         //返回一个函数，参数就是传进来接收数据的组件
         export default function withForm(WrappedComponent){
             //返回一个组件，名字不重要，可以忽略，调用函数时，用变量接收，变量就是返回的组件
             return class extends React.Component{
                 //在这边写传进来的组件公有的属性和方法
                 /* 
                    高阶组件问题2：使用了react-dev-tool插件后，可以在浏览器调试页面看到对应信息，但是由于返回的组件没有设置组件名所以调试页面看到的都是_temp,但是如果设置为Demo，那么看到的就全都是Demo,若使用高阶组件比较多，嵌套的也比较多的时候，找组件很麻烦（要见名知意）,可以给每个使用高阶组件设置不同的名字
                 */
                //解决问题2:--->在静态属性上设置displayName,WrappedComponent.name可以获取组件的名字
                static displayName = 'with'+WrappedComponent.name

           //state和handleChange是复用,公有的
                 state ={
                     username:'',
                     password:''
                 }
                 handleChange = name => e => {
                       this.setState({
                         [name]: e.target.value
                       })
                     }
                   
                 render(){
                     /* 高阶组件问题1：若App想给login传递数据,要通过withLogin(因为App->withLogin->Login,App与Login是爷孙关系)
                         解决-->  在withLogin组件中给Login传递数据，但是如果数据比较多的时候会比较麻烦,所以可以像state一样进行解构传值*/
                     return (
                     <WrappedComponent handleChange={this.handleChange} {...this.state} {...this.props} ></WrappedComponent>)
                 }
             }
         }
                       
         ---------------------------------------------------------------------              
         APP.js
         import React, { Component } from 'react'
         import Login from './pages/Login'
         import Register from './pages/Register'

         import withForm from './withForm'

         //调用withForm,将组件传进去,返回一个新的组件WithLogin/WithRegister
         const WithLogin = withForm(Login)
         const WithRegister = withForm(Register)

         export default class App extends Component {
           
           render() {
             return (
               <div><h1>高阶组件</h1>
                 {/* <Login></Login> */}
               //在需要使用Login的地方,调用返回的组件，返回的组件里面渲染的就是你传进去的那个组件(看上面的withForm.jsrender中return的)
                 <WithLogin test={'app传给login的值'}/>
                 <WithRegister/>
               </div>
             )
           }
         }

         ---------------------------------------------------------------------
         //登录注册组件
         Login.js
         import React, { Component } from 'react'
         export default class Login extends Component {
           render() {
             const {username,password,handleChange} = this.props
             console.log(this.props)
             return (    
               <div>
                 <br/>
                 <h1>登录</h1>
                 <form>
                   用户名:
                   <input type='text' value={username}
                     onChange={handleChange('username')}/>
                   <br />
                   密码:
                   <input type='password' value={password}
                     onChange={handleChange('password')} />
                   <br />
                   <input type='button' value='登录' />
                 </form>
               </div>
             )
           }
         }
         ---------------------------------------------------------------------
         Register.js
         import React, { Component } from 'react'
         export default class Register extends Component {
           render() {
             const {username,password,repassword,handleChange} = this.props
             return (
               <div>
                 <br/>
                 <h1>注册</h1>
                 <form>
                   用户名:
                   <input type='text' value={username}
                     onChange={handleChange('username')}/>
                   <br />
                   密码:
                   <input type='password' value={password}
                     onChange={handleChange('password')} />
                    <br />
                   确认密码:
                   <input type='password' value={repassword}
                     onChange={handleChange('repassword')}/>
                   <br />
                   <input type='button' value='登录' />
                 </form>
               </div>
             )
           }
         }
         ```

       - **高阶组件有两个问题**, 问题和解决方法看上面具体实现中的withForm.js中有写到。

     - ####render props

       -- render props ---> 将复用的状态和逻辑代码封装到一个组件中 。

       - 具体实现

         ```js
         //案例说明：猫捉老鼠

              //1、定义一个组件Position,来写公有的代码
         Position.js
              import React,{Component} from 'react'
              export default class Position extends Component{
                //公有状态
                 state = {
                     x:'',
                     y:''
                   }
                //公有方法
                 handle = (e)=>{
                   //设置鼠标的位置
                   this.setState({
                     x:e.clientX,
                     y:e.clientY
                   })
                 }
                 //在创建完成阶段绑定事件
                 componentDidMount(){
                   window.addEventListener('mousemove',this.handle)
                 }
                 //在卸载阶段清除事件
                 componentWillUnmount(){
                    window.removeEventListener('mousemove',this.handle)
                 }
              //渲染阶段，返回结构
                render(){
                    //方法一
                    return this.props.render(this.state)
                    //方法二 通过this.props.children可以获取到组件中的子节点，这里子节点为函数
                    return this.props.children(this.state)
                }

              }

         ----------------------------------------------------------------
         App.js

              import React, { Component } from 'react'
              import Cat from './components/Cat'
              import Mouse from './components/Mouse'

              import Position from './Position'

              export default class App extends Component {
                render() {
                  return (
                    <div>
                      <h1>猫捉老鼠</h1>
                    {/* 使用第一种写法 render是一个函数,把真正要渲染的组件当作这个函数的返回值，Position组件调用render时传入了状态数据,返回值(真正要渲染的组件)接收了这个状态数据 */}
                    {/* <Position render={state=><Mouse state={state}></Mouse>}></Position>
                    <Position render={state=><Cat state={state}></Cat>}></Position> */}
                     {/* 使用第二种写法  类似consumer,组件Position获取到子节点(函数)，调用的时传入了状态数据,返回值(真正要渲染的组件)接收了状态数据 */}
                  <Position>{state=><Mouse state={state}></Mouse>}</Position>
                  <Position>{state=><Cat state={state}></Cat>}</Position>
                    </div>
                  )
                }
              }

         ----------------------------------------------------------------  
         //猫和老鼠的代码
          Cat.js

              import React, { Component } from 'react'
              import CatUrl from '../assets/cat.gif'

              export default class Cat extends Component {
                render() {
                  let { x, y } = this.props.state
                  x += 200
                  y -= 50
                  return (
                    <div>
                      <img src={CatUrl} alt='' style={{ position: 'absolute', left: x, top: y }} />
                    </div>
                  )
                }
              }
         ----------------------------------------------------------------------
          Mouse.js

              import React, { Component } from 'react'
              import MouseUrl from '../assets/mouse.gif'

              export default class Mouse extends Component {
                render() {
                  let { x, y } = this.props.state
                  return (
                    <div>
                      <img src={MouseUrl} alt='' style={{ position: 'absolute', left: x, top: y, width: 100 }}/>
                    </div>
                  )
                }
              }
         ```


- ####Hooks（钩子,本质是函数）

     - **作用** :本质就是函数,可以**定义状态和生命周期钩子函数**。

     - **Hook出现原因**: 因为组件复用状态逻辑比较难,写起来也比较麻烦,Hook可以在无需修改组件的情况下复用状态逻辑。因为函数中没有this,所以**也解决了this指向的问题**

     - Hook/React开发团队的梦想:在未来逐渐取代class

     - **常用的Hooks**

       - userState()（在函数组件中定义状态）

         - 使用方法

           (1)导入（import React ,{userState} from 'react'）

           (2)使用 （const [状态的属性,操作状态属性的方法] = useState(属性默认值)）

           ```js
           //useState使用的代码演示(可以使用多次)

           import React ,{userState} from 'react'
           export default function Header(){
             //定义状态,state中有一个属性count默认值为0,若想修改count的值,可以调用setCount(传进来要给count赋值的数据)
             const [count,setCount] = useState(0)
             //若state中有多个属性，那就继续接着写即可
             const [msg,setMsg] = userState('默认值')
             
             return 
                (<div>
                   <div>{count}</div>
                   <button onClick={()=>{
                       //直接调用,修改count的值为count+1
                       setCount(count+1)
                    }}>按钮</button>
                 </div>)
           }
           ```

           ​

       - userEffect()（在函数组件中模拟生命周期钩子函数）

         - 使用方法

           (1)导入（import React ,{userEffect} from 'react'）

           (2) 使用 useEffect(()=>{},[])

           ```js
           //useEffect使用的代码演示
           import React ,{userEffect} from 'react'
           export default function Header(){
             //定义状态
             const [count,setCount] = useState(0)
             
             //模拟生命周期钩子函数
             useEffect(()=>{
               //没有传第二个参数的时候,这边相当于componentDidMount和componentDidUpdate
               return ()=>{
                 // 相当于componentWillUnmount,完成清除操作
               }
             },[count,props.msg])
             /*解析:userEffect函数调用时
              1、若只有一个参数(回调函数),这个第一个参数中的回调函数在创建完成和更新阶段都会调用(相当于componentDidMount和componentDidUpdate)
              2、第一个参数回调函数有一个返回值，也是一个回调函数,这个回调函数在组件销毁的时候会调用(相当于componentWillUnmount)
              3、若useEffect传入第二个参数为一个空数组时,则useEffect只相当于componentDidMount,更新阶段就不会执行
              4、若useEffect传入第二个参数为数组(不是空数组),数组中可以传入state或props数据,传入数组的数据就是被监听的数据,只要这些数据中有一个值发生变化,第一个参数的回调函数才会重新执行(执行之前会先执行卸载)*/
             
               return 
                (<div>
                   <div>{count}</div>
                   <button onClick={()=>{
                       //直接调用,修改count的值为count+1
                       setCount(count+1)
                    }}>按钮</button>
                 </div>)
           }
           ```

     - **Hooks使用的规则**（不管是react提供的hooks,还是第三方的hooks,还是自定义的hooks. 都遵守下面的规则） 

         (1) hooks只能在函数组件和自定义hooks中使用。

         (2) 在函数组件和自定义hooks中使用其他hooks的时候,要求写在顶级作用域,千万不要写在if/循环/嵌套的普通函数中

     - **自定义Hooks函数** (就是以useXXX命名的普通函数,在这个函数中实现定义状态和模拟生命周期)

- ​

- ​

- ​

- ### 脚手架初始化项目

     - 创建脚手架方式

       - 方式一

         npm i create-react-app -g //全局安装

         create-react-app 项目名称

       - 方式二

          npx create-react-app 项目名称（项目名称不能有大写）

          这一行代码都做了哪些事情

         ​      1.先下载create-react-app 这个脚手架工具

         ​      2.使用脚手架工具下载项目(已经配置好的项目)

         ​      3.删除create-react-app这个脚手架工具

     - 项目的整体技术架构 :   react + webpack4 + es6 + eslint + babel

         -- publilc文件夹中必须要有一个index.html

         -- src文件夹里面必须要有一个index.js(入口文件)

     - 启动项目

       - yarn start

         --若之前有安装过yarn的话,就使用,一旦使用yarn后,在使用npm的话,那么这个项目之前下载过的依赖会在通过npm下载一遍。所以用了yarn后尽量不要npm。若没有yarn的话，可以一直使用npm。

     - ​

- ​

     ​

     ​