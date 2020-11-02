### redux

 -- Redux是一个状态管理器,是一个js库,用于集中管理状态。

 -- 特点：集中管理、可预测、易于测试、易于调试、强大的中间件机制。

**注意: redux是一个独立于react的库,可以配合任何的UI库/框架使用。 **

- ####Redux的三大核心概念

  - **action** (表示需求,并提供完成该需求需要的数据)

    - action是一个js对象,必须要有一个type属性,描述需求/动作类型,type的值使用大写字母的字符串。也可以提供其他属性,提供完成需求/动作需要的数据。
    - 简化操作：使用函数来创建action,将该函数叫做actionCreator

    ```js
    // todo app，添加任务的action：
    { type: 'ADD_TODO', text: '学习Redux' }

    //简化action creator:
    const increment = () => ({ type: 'INCREMENT' })
    // 创建action
    increment()
    ```

  - **reducer** (表示具体执行需求的角色(开发者))

    - reducer是一个纯函数(同样的输入，必定得到同样的输出),不要有修改参数、调用 Math.random() 等不纯的操作。（**reducer中不能写异步操作**） 

    - 实现方式

      ```js
      //作用接收两个参数 state(初始化state),action,完成此需求/行为动作,并返回state

      //给state设置默认值
      let initState = {
        count:0,
        msg:'默认值'
      }
      const counter = (state = initState, action) => {
        switch (action.type) {
          case 'INCREMENT':
            return {
              //为了防止state被覆盖，先将state中的数据解构出来，后面修改的会覆盖原先的数据
              ...state,
              //修改的数据最新属性数据,
              count:state.count+action.data
            }
          default:           // 必须得有！默认返回当前 state
            return state
        }
      }
      ```

    - reducer的执行时机(或者说什么时候被触发)

      (1)  reducer在创建store的时候会被调用一次,这次调用会走reducer函数中的default,用来初始化redux中的数据。

      (2)  每一次调用dispatch的时候会被触发,redux会自动将最新的数据,传递给state参数,将需求传递给action参数。

  - **store** (是Redux的仓库,一个redux只有一个store)

    - 作用：将action和reducer组合到一起

    - 职责：获取到action,触发dispatch将action传进去,触发reducer去执行。

    - 实现方式

      ```js
      //导入
      import {createStore} from 'redux'
      import reducer from './reducer.js'
      //接收reducer作为参数,创建store
      const store = createStore(reducer)
      ```

    - **若使用了中间件store的写法** 

      ```js
      //需要在redux中解构applyMiddleware
      import {createStore,applyMiddleware} from 'redux'
      import reducer from './reducer.js'

      const store = createStore(reducer,applyMiddleware(中间件1,中间件2...))
      ```

- #### redux的注意点

  - 一般分为四个文件去写

    (1) actions.js --> 在这里定义同步的action和异步的action

    (2) contants.js --> 定义一些常量(需求类型)

    (3) reducer,js --> 定义reducer函数

    (4) store.js --> 创建store

- #####redux的基本使用（都写在了一个js文件中）

  ```js
  //1 安装
  npm i redux
  //2 创建store
  import { createStore } from 'redux'
  //3. 创建store,将reducer传入到store中
  const store = createStore(reducer)

  //4 定义reducer函数,接收两个参数state,action
  //注意的是:在定义reducer的时候,通过es6语法设置默认值的方式,初始化state的值
  function reducer(state=0,action){
    switch(action.type){
        case 'INCREMENT'
          return state+1
        default:
          return state//一定要写一个默认情况,返回原来的state
    }
  }

  //触发reducer处理,dispatch传action
  store.dispatch({type:'INCREMENT'})
  ```

- redex的内部执行原理

- ####react-redux(Provider组件,connect函数)

  -- 若想要在react中使用redux,需要导入包**react-redux** ,使用react-redux包要有一个顶层思想--->**要有容器组件和展示组件** 

    **展示组件**（就是react组件）: 通过props去获取redux的数据和操作redux数据的方法。 （不跟redux关联的组件,要传入connect()()中的组件,**特点**--> 提供了组件的结构和样式）

    **容器组件**（就是使用connect函数得到的组件）: 职责-->将redux的数据和操作redux数据的方法传递给展示组件。（跟redux密切相关的组件,调用connect()()之后得到的组件,**特点**--> 专门与redux进行交互）

  - ####核心API 

    - **Provider组件**  --> 用来包裹整个React应用,接收store属性,为应用提供state和操作state的方法。

    - **connect函数** --> 连接Redux和React组件,为包裹的组件提供state和操作state的方法,组件通过props获取Redux store的内容。

      - connect函数的内部结构

        ```js
        //connect组件返回的是一个函数，函数里面返回的是组件
        function connect(){
          return function(){
            return class extends Componect{
              
            }
          }
        }
        ```

      - connect函数传参的方式(简写形式,原始写法可以看老师的md文件)

        - 第一个参数 (传入是一个函数 state=>({属性名: state.属性名})

          ```js
          //第一次connect函数执行的时候,这个函数会被调用,state接收的就是redux中最新的数据,return的对象,就是添加到展示组件props身上的数据
          //后面如果redux中的数据发生变化了,react-redux还会继续调用这个函数,将最新的数据传递给展示组件,所以在展示组件中,永远可以自动获取到最新的数据。
          ```

        - 第二个参数 (传入一个对象 {actionCreator,actionCreator})

          ```js
          //注意：在展示组件中,可以获取到相同名称的函数,但是在展示组件中拿到的函数不是真正的actionCreator,只是名字相同而已。

          //为什么名字相同？
           因为connect函数将传入的actionCreator进行了一次封装,比如actionCreator的名字叫inc,connect会根据这个inc新建一个函数,
             function inc(){//这个inc是传递给展示组件的函数
             dispatch(inc())//这个inc是actionCreator
           }
          ```


  - **具体使用**

    - 1、在App根组件中,导入Provider包裹App中结构并传入store对象
      ```js
      //导入
      import {Provider} from 'redux'
      export default class App extends Component {
        render() {
          return (
            //使用Provider包裹
            <Provider store={store}>
              <div className="container">
                <Search />
                {/* <List /> */}
                <WithList></WithList>
              </div>
            </Provider>
          );
        }
      }
      ```

    - 2、若哪一个组件中需要用到redux的数据,就针对这个组件,写一个容器组件,然后在这个容器组件中传递数据和操作数据的方法。

      ```js
      WithList.js

      import { connect } from 'react-redux' //导入connect
      import List from '../pages/List'  //导入展示组件
      import {getUsersAsync} from '../redux/actions' //导入操作redux数据的方法(异步action)


      //const 容器组件 = connect(传给展示组件的数据, 传给展示组件操作redux数据的方法)(展示组件)
      const WithList = connect(state=>({users: state.users}), {getUsersAsync})(List)
      export default WithList

      ```

- ####redux-thunk使用

  ```js
  为什么使用redux-thunk?
  ```

  ​

- 8

- 9

- ​