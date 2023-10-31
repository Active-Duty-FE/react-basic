react脚手架  
####  第一步
全局安装 npm install create-react-app -g
####  第二步
切换到想创建项目的目录，使用命令 create-react-app project-name
####  第三步
进入项目文件夹 cd project-name
####  第四步
启动项目 npm start or yarn start

### 变化 *
## React 18.* render的新方法
as-is
```js
ReactDOM.render(
    <React.StrictMode>
        <App />
    </React.StrictMode>,
    document.getElementById('root')
)
```
to-be
```js
ReactDOM.createRoot(document.getElementById('root')).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
)
```
or
```js
const container = document.getElementById('root')
const root = ReactDOM.createRoot(container)
root.render(
    <React.StrictMode>
        <App />
    </React.StrictMode>
)
```

## Proxy 变化 *
setupProxy.js
```js
const {createProxyMiddleware} = require('http-proxy-middleware')

module.exports = function(app){
  app.use(
    createProxyMiddleware('/api1',{
      target:'http://localhost:5000',
      changeOrigin:true,
      pathRewrite:{'^/api1':''}
    }),
    createProxyMiddleware('/api2',{
      target:'http://localhost:5001', 
    })
  )
}
```

## Router
* 安装  
```console
npm install --save react-router-dom
```
* 引入并适用
#### 基本用法
```js
// 适用 BrowserRouter 或 HashRouter
import { BrowserRouter, Link, Route } from 'react-router-dom'
<BrowserLink>
  <Link to="/home">Home</Link>
  <Link to="/about">About</Link>
  <Route path="/home" component={Home} />
  <Route path="/about" component={About} />
</BrowserLink>
```
#### Link 高亮


#### 基本用法 Link
```js
// 适用 BrowserRouter 或 HashRouter
import { BrowserRouter, Link, Route, Routes } from 'react-router-dom'
<BrowserLink>
  <Link to="/home">Home</Link>
  <Link to="/about">About</Link>
  <Routes>
    <Route path="/home" component={Home} />
    <Route path="/about" component={About} />
  </Routes>
</BrowserLink>
```
#### 高亮 NavLink
```js
// 适用 BrowserRouter 或 HashRouter
import { BrowserRouter, NavLink, Route, Routes } from 'react-router-dom'
<BrowserLink>
  <NavLink to="/home">Home</NavLink>
  <NavLink to="/about">About</NavLink>
  <Routes>
    <Route path="/home" component={Home} />
    <Route path="/about" component={About} />
  </Routes>
</BrowserLink>
```
### router 传递参数 Router V5
#### App 代码
```js
state = [
  {id:01, title:'aaa'},
  {id:02, title:'bbb'},
  {id:02, title:'ccc'}
]
const messageArr = this.state
return (
  {
    messageArr.map((msgObj) => {
      return (
        <li key={msgObj.id}>
          /* 向路由组件传递 params 参数 */
          /* <Link to={`/home/message/detail${msgObj.id}/${msgObj.titel}`}>{msgObj.title}</Link> */
          /* 向路由组件传递 search 参数 */
          /* <Link to={`/home/message/detail?id=${msgObj.id}&title=${msgObj.titel}`}>{msgObj.title}</Link> */
          /* 向路由组件传递 state 参数 */
          /* <Link to={{pathname:'/home/message/detail',state:{id:msgObj.id,title:msgObj.title}}}>{msgObj.title}</Link> */
      )
    })
  }
  /* 生命接收 params 参数 */
  /*<Route path="/home/message/detail/:id/:title" component={Detail} />*/
  /* search 参数无需接收，正常注册路由即可 */
  /*<Route path="/home/message/detail" component={Detail} />*/
  /* state 参数无需接收，正常注册路由即可 */
  /*<Route path="/home/message/detail" component={Detail} />*/
)
```
#### Detail 代码
```js
/* 接收 params 参数 */
/* const {id,title} =  this.props.match.params */
/* 接收 search 参数 */
/* 
  const {search} =  this.props.locattion.params 
  const {id,title} = qs.parse(search.slice(1))
*/
/* 接收 state 参数 */
/* const {id,title} =  this.props.location.state */
```
#### 总结
1. params 参数
    路由链接（携带参数）：  
    `<Link to='/demo/test/tom/18'>详情</Link>`  
    注册路由：  
    `<Route path='/demo/test/:name/:age' component={Test} />`  
    接收参数：  
    `this.props.match.params`
2. seach 参数
    路由链接（携带参数）：  
    `<Link to='/demo/test/?name=tom&age=18'>详情</Link>`  
    注册路由：  
    `<Route path='/demo/test' component={Test} />`  
    接收参数：  
    `this.props.location.search`  
    备注：获取到 search 是 urlencode 编码字符串，需要借助 qeurystring 解析
3. state 参数
    路由链接（携带参数）：  
    `<Link to={{pathname:'/demo/test',state:{name:'tom',age:18}}}>详情</Link>`  
    注册路由：  
    `<Route path='/demo/test' component={Test} />`  
    接收参数：  
    `this.props.location.state`  
    备注：刷新也可以保留参数
## react UI
### material-ui（国外）
官网：http://www.material-ui.com  
github：https://github.com/callemall/material-ui  
### ant-design（蚂蚁金服）
官网：https://ant.design/index-cn  
github：https://github.com/ant-design/ant-design  
`npm install antd`  
想看详细的配置： 官网右上角 - 3.X版本 - 左侧 create-react-app 中使用  
不能按一下方法暴露
样式按需引入 -> 配置脚手架 -> 暴露脚手架配置
`npm eject` Y

### element UI （饿了吗）
### vantUI （有赞） - vue 移动端设计

npm 设置淘宝镜像
```
npm get registry
npm config set registry https://registry.npm.taobao.org
npm config set registry https://registry.npmjs.org
```
## redux 求和案例 - 精简版
1. 去除 Count 组件自身的状态
2. src 下建立：  
    -src  
      >-redux  
        >> -store.js  
        -count_reducer.js
3. store.js  
3.1 引入 redux 中的 createStore 函数，创建一个 store  
3.2 createStore 调用时要传入一个为其服务的 reducer  
3.3 记得暴露 store 对象
4. count_reducer.js  
4.1 reducer 本质是一个函数，接收 preState, action 返回加工后状态  
4.2 reducer 有两个作用：初始化状态，加工状态
4.3 reducer 被第一次调用时，是 store 自动触发的，传递的 preState 是 undefined
5. 在 index.js 中监测 store 状态的改变，一旦发生改变重新渲染 `<App />`  
```js
store.subscribe(() => {
  root.render(
    <React.StrictMode>
      <App />
    </React.StrictMode>
  )
})
```
  或者 
  ```js
    // 监测 redux 中状态的变化，只要有变化，就调用 render
    store.subscribe(() => {
      this.setState({})
    })
  }
  ```
  备注: redux 只负责管理状态，至于状态的改变驱动着页面的展示，要靠自己写。
## redux 求和案例 - 完整版 
新增文件：  
1. count_action.js 专门用于创建 action 对象
2. constant.js 防止容易写错的 type 值
## redux 求和案例 - redux 异步 action 版
1. 明确：延迟的动作不想交给组件自身，想给 action
2. 何时需要异步 action：想要对状态进行操作，但是具体的数据靠异步任务返回。
3. 具体编码：  
3.1 npm install redux-thunk, 并配置在 store 中  
3.2 创建 action 的函数不再返回一般对象，而是一个函数，该函数中写异步任务。  
3.3 异步任务有结果后，分发一个同步的 action 去真正操作数据。
4. 备注：异步 action 不是必须要写的，完全可以自己等待异步任务的结果后再去分发同步 action
## redux 求和案例 - react-redux 基本使用
1. 明确两个概念  
1.1 UI组件：不能使用任何 redux 的 api，只负责页面的呈现，交互等  
1.2 容器组件：负责和 redux 通信，讲结果交给 UI 组件。
2. 如何创建一个容器组件 - 靠 react-redux 的 connect 函数
`connect(mapStateToProps,mapDispatchToProps)(UI 组件)`  
2.1 mapStateToProps：映射状态，返回值是一个对象  
2.2 mapDispatchToProps：映射操作状态的方法，返回值是一个对象 
3.   
3.1 备注：容器组件中的 store 是靠 props 传进去的，而不是在容器组件中直接引入
3.2 备注：mapDispatchToProps，也可以是一个对象

## redux 求和案例 - react-redux 优化
1. 容器组件和 UI 组件混成一个文件
2. 无需自己给容器组件传递 store, 给 <App /> 包裹一个 <Provider store={store}></Provider>即可
3. 使用了 react-redux 后也不用再自己监测 redux 中状态的改变，容器组件可以自动完成这个工作
4. mapDispatchToProps 也可以简写成一个对象
5. 一个组件要和 redux 打交道，要经过哪几步？  
5.1 定义好 UI 组件 -- 不暴露  
5.2 引入 connect 生成一个容器组件，并暴露，写法如下：
```js
connect(
  state => ({key:value}), // 映射状态
  {key:xxxxAction} // 映射操作状态方法
)(UI 组件)
```
5.3 在 UI 组件中通过 this.props.xxx 读取和操作状态
## redux 求和案例 - react-redux 数据共享版
1. 定义一个 Person 组件，和 Count 组件通过 redux 共享数据
2. 为 Person 组件编写：reducer, action 配置 constant 常量
3. 重点：Person 的reducer 和 Count 的 Reducer 要使用 combineReducers 进行合并。合并后的总状态是一个对象！
4. 交给 store 的是总 reducer， 最后注意在组件中读取状态的时候，记得“取到位”。

## redux 求和案例 - react-redux 开发者工具的使用
1. npm install redux-devtools-extension
2. store 中进行配置
```js
import {composeWithDevTools} from 'redux-devtools-extention'
const store = createStore(allReducer,composeWithDevTools(applyMiddleWare(thunks)))
```
## redux 求和案例 - react-redux 最终版
1. 所有变量名字要规范，尽量触发对象的简写方式
2. reducer 文件夹中，编写 index.js 专门用于汇总并暴露所有的 reducer

# 纯函数
1. 一类特别的函数：只要是同样的输入（实参），必定的同样的输出（返回）
2. 必须遵守以下约束：
2.1 不得改写参数数据
2.2 不会产生任何副作用，例如网络请求，输入和输出设备
2.3 不能调用 Date.now() 或者 Math.random() 等不纯的方法
3. redux 的 reducer 函数必须是一个纯函数
## 本地服务器
`npm i serve -g`
`serve build`
## 拓展
#### setState 更新状态的 2 种写法
1. setState(stateChange,[callback]) ------ 对象式的 setState
1.1 stateChange 为状态改变对象（该对象可以体现出状态的更改）
1.2 callback 是可选的回调函数，他在状态更新完毕、界面也更新后（render调用后）才被调用
2. setState(updater,[callback]) ------ 函数式的 setState
2.1 updater 为返回 stateChange 对象的函数
2.2 updater 可以接收到 state 和 props
2.3 callback 是可选的回调函数，他在状态更新，界面更新后（render调用后）才被调用
##### 总结
1. 对象式的 setState 是函数式的 setState 的简写方式（语法糖）
2. 使用原则：
2.1 如果新状态不依赖于原状态 => 使用对象方式
2.2 如果新状态依赖于原状态 => 使用函数式
2.3 如果需要在 setState() 执行后获取最新的状态数据，要在第二个 callback 函数中读取

# Hooks
### 1. React Hook/Hooks 是什么？
1.  Hook 是 React 16.8.0 版本增加的新特特性/新语法
2. 可以让你再函数组件中使用 state 以及其他的 React 特性
### 2. 三个常用的 Hook
1. State Hook: React.useState()
2. Effect Hook: React.useEffect()
3. Ref Hook: React.useRef()
### 3. State Hook
1. State Hook 让函数组件也可以有 state 状态，并进行状态数据的读写操作
2. 语法：const [xxx,setXxx] = React.useState(initValue)
3. useState() 说明：
  参数：第一次初始化指定的值在内部做缓存
  返回值：包含 2 个元素的数组，第一个为内部当前状态值，第二个为更新状态的函数
4. setXxx() 两种写法：
  setXxx(newValue): 参数为非函数值，直接指定新的状态值，内部用其覆盖运来的状态值
  setXxx(value => newValue): 参数为函数，接收原本的状态值，返回新的状态值，内部用其覆盖原来的状态值

### 4. Effect Hook
1. Effect Hook 可以让你在函数组件中执行副作用操作（用于模拟类组件中的生命周期钩子）
2. React 中的副作用操作：
发 ajax 请求数据获取
设置订阅 / 启动定时器
手动更改真是DOM
3. 语法说明：
```js
useEffect(() => {
  // 在此可以执行任何带副作用的操作
  return () => {
    // 在组件卸载前执行，做一些收尾工作，比如清除定时器 / 取消订阅等
  }
},[stateValue]) // 如果指定的是 []，回调函数只会在第一次 render() 后执行
```
4. 可以把 useEffect Hook 看做如下三个函数的组合
componentDidMount()
componentDidUpdate()
componentWillUnmount()

### 5. Ref Hook
1. Ref Hook 可以在函数组件中存储/查找组件内的标签或任意其他数据
2. 语法：const refContaner = useRef()
3. 作用：保存标签对象，功能与 React.createRef() 一样
### 6. context
### 7. render props
如何想组件内部动态传入带内容的结构（标签）？    
vue 中：  
使用 slot 技术，也就是通过组件标签体传入结构 <A><B /></A>  
React 中：  
使用 children props：通过组件标签传入结构  
使用 render props：通过组件标签属性传入结构，一般用 render 函数属性
##### children props
```js
<A>
 <B>xxx</B>
</A>
{this.props.children}
// 问题：如果 B 组件需要 A 组件内的数据 ==> 做不到
```
##### render props
`<A render={(data) => <C data={data} />}></A>`  
A 组件：{this.props.render(内部 state 数据)}  
B 组件：读取 A 组件传入的数据显示 {this.props.data}

### 9. 组件通信方式总结
#### 组件间的关系：
父子组件、兄弟组件（非嵌套组件）、祖孙组件（跨级组件）
#### 几种通信方式
1. props  
1.1 children props  
1.2 render props  
2. 消息订阅 - 发布  
pubs-sub, event 等等  
3. 集中式管理：  
redux, dva 等等  
4. conText：  
生产者 - 消费者  
#### 比较好的搭配方式
父子组件：props  
<<<<<<< HEAD
兄弟组件：消息订阅 - 发布、集中式管理  
祖孙组件（跨级组件）：消息订阅 - 发布、集中式装填管理、conText（开发用的少、封装插件用的多）


=======
兄弟组件：消息订阅 - 发布、集中式状态管理  
祖孙组件（跨级组件）：消息订阅 - 发布、集中式状态管理、conText（开发用的少、封装插件用的多）
>>>>>>> 45a334bebd8e327cb1a83a37555e38fe26540928

### 变化 *
 The unmountComponentAtNode() method has been replaced with root.unmount() in React 18.
```js
index.js
// Before
unmountComponentAtNode(container);

// After
root.unmount();
<<<<<<< HEAD
```

### react-router 6
一、\<BrowserRouter>
1. 说明：\<BrowserRouter> 用于包裹整个应用
2. 示例代码：
```js
import React from 'react'
import ReactDOM from 'react-dom'
import {BrowserRouter} from 'react-router-ddom'

ReactDOM.render(
  <BrowserRouter>
    <App />
  </BrowserRouter>
)
```
二、\<HashRouter>
1. 说明：作用与\<BrowserRouter>一样\<HashRouter>修改的是地址栏的 hash 值
2. 备注：6.x 版本中，\<HashRouter>、\<BrowserRouter>的用法与 5.x 相同  

三、\<Routes/> 与 \<Route>  
1. v6 版本中移除了先前的 \<Switch>，引入了新的替代者：\<Routes>
2. \<Routes> 和 \<Route> 要配合使用，且必须要用 \<Routes> 包裹 \<Route>
3. \<Route> 相当于一个 if 语句，如果其路径与当前 URL 匹配，则呈现其对应的组件
4. \<Route caseSensitive> 属性用于指定：匹配时是否区分大小写（默认为 false）
5. 当 URL 发生变化时，\<Routes> 都会查看其所有子 \<Route> 元素已找到最佳匹配并呈现组件
6. \<Route> 也可以嵌套使用，且可配合 useRoutes() 配置“路由表”，但需要通过\<Outlet> 组件来渲染其子路由
7. 示例代码
```js
<Routes>
  /* path 属性用于定义路径，element 属性用于定义当前路径所对应的组件 */
  <Route path="login" element={<Login />}></Route>

  /* 用于定义嵌套路由，home 是一级路由，对应的路径 /home */
  <Route path="home" element={<Home />}>
    /* test1 和 test2 是二级路由，对应的路径是 /home/test1 或 /home/test2 */
    <Route path="test1" element={<Test1 />}></Route>
    <Route path="test2" element={<Test2 />}></Route>
  </Route>
  // Route 也可以不写 element 属性，这时就是用于展示嵌套的路由，所对应的路径是 /user/xxx
  <Route path="users">
    <Route path="xxx" element={<Demo />}>
  </Route>
</Routes>
```
四、\<Link>
1. 作用：修改 URL，且不发送网络请求（路由链接）
2. 注意：外侧须有用<BrowserRouter></BrowserRouter>包裹
3. 示例代码：
```js
  import { Link } from 'react-router-dom'
  
  function Test() {
    return (
      <div>
        <Link to="/路径"></Link>
      </div>
    )
  }
```
五、\<NavLink>
1. 作用：与\<Link>组件类似，且可实现导航的“高亮”效果
2. 示例代码：
```js
// 注意：NavLink 默认类名是 active，下面是指定自定义的 class
// 自定义样式
<NavLink
  to="login"
  className={({isActive}) => {
    console.log('home',isActive)
    return isActive ? 'baseClassName activeName' : 'baseClassName'
  }}

>login</NavLink>
/*
  默认情况下，当 Home 的子组件匹配成功，Home 的导航也会高亮，
  当 NavLink 添加了 end 属性后，若 Home 的子组件匹配成功，则 Home 的导航没有“高亮”效果
*/
<NavLink to="home" end>home</NavLink>
```
六、\<Navigate>
1. 作用：只要\<Navigate>组件被渲染，就会修改路径、切换视图
2. replace 属性用于控制跳转模式，（push 或 replace，默认是 push）
3. 示例代码：
```js
import React，{ useState } from 'react'
import { Navigate } from 'react-router-dom'

export default function Home() {
  const [sum,setSum] = useState(1)
  return (
    <div>
      <h3>我是Home的内容</h3>
      {/* 根据 sum 的值决定是否切换视图 */}
      {sum === 1 ? <h4>sum 的值为 {sum}</h4> ：<Navigate to="/about" replace={true}></Navigate>}
      <button>点我将 sum 变为 2</button>
    </div>
  )
}
```
七、\<Outlet>
1. 当\<Route>产生嵌套时，渲染其对应的后续子路由
2. 示例代码：
```js
// 根据路由表生成对应的路由规则
const element = useRoutes([
  {
    path:'/about',
    element:<About/>
  },
  {
    path:'/home',
    element:<Home/>,
    children:[
      {
        path:'news',
        element:<News/>
      },
      {
        path:'message',
        element:<Message/>
      }
    ]
  }
])

// Home.jsx
import React form 'react'
import { NavLink,Outlet } from 'react-router-dom'
export default function Home() {
  return (
    <div>
      <h2>我是Home</h2>
      <ul className="nav nav-tabs">
        <li>
          <NavLink className="list-group-item" to="news">News</NavLink>
        </li>
        <li>
          <NavLink className="list-group-item" to="message">Message</NavLink>
        </li>
      </ul>
      {/* 指定路由组件呈现的位置 */}
      <Outlet />
    </div>
  )
}
``` 
### 路由的 Hooks
1. useRoutes()
```js
// 路由表配置：src/routes/index.js
import {Navigate} from 'react-router-dom'
import About from '../pages/About'
import Home from '../pages/Home'

const routes = [
  {
    path:'/about',
    element:<About />
  },
  {
    path:'/home',
    element:<Home />
  },
  {
    path:'/',
    element:<Navigate to="/about" />
  }
]
export default routes

// App.jsx
import React from 'react'
import {NavLink,Navigate,useRoutes} from 'react-router-dom'
import routes from './routes'

export default function App() {
  // 根据路由表生成对应的路由规则
  const element =  useRoutes(routes)
  return (
    <div>
      <NavLink className="list-group-item" to="/about">About</NavLink>
      <NavLink className="list-group-item" to="/home">Home</NavLink>
      
      {element}
    </div>
  )
}
```
2. useNavigate()  
2.1 作用：返回一个函数用来实现编程式导航  
2.2 示例代码
```js
import React from 'react'
import { useNavigate } from 'react-router-dom'

export default function Demo() {
  const navigate = useNavigate()
  const handle = () => {
    navigate('/login',{
      replace: false,
      state: {a:1,b:2}
    })
    navigate(-1)
  }
  return (
    <div>
      <button onClick={handle}>按钮</button>
    </div>
  )
}
```
3. useParams()  
3.1 作用：返回当前陆游的 params 参数，类似于 5.x 中的 match.params
3.2 示例代码：
```js
import React from 'react'
import { Routes, Route, useParams } from 'react-router-dom'
import User from './pages/User.jsx'

function ProfilePage() {
  // 获取 URL 中携带过来的 params 参数
  let { id } = useParams() 
}
function App() {
  return (
    <Routes>
      <Route path="users/:id" element={<User />}></Route>
    </Routes>
  )
}
```
4. useSearchParams()  
4.1 作用：用于读取和修改当前位置的 URL 中的查询字符串
4.2 返回一个包含两个值的数组，内容非别为：当前的 search 参数、更新 search 的函数
4.3 示例代码：
```js
import React from 'react'
import { useSearchParams } from 'react-router-dom'

export default function Detail() {
  const [search,setSearch] = useSearchParams()
  const id = search.get('id')
  const title = search.get('title')
  const content = search.get('content')
  return (
    <ul>
      <li>
        <button onClick={() => setSearch('id=008&title=哈哈&content=嘻嘻')}>点我更新一下收到的 search 参数</button>
      </li>
      <li>{id}</li>
      <li>{title}</li>
      <li>{content}</li>
    </ul>
  )
}
```
5. useLocation()  
5.1 作用：获取当前 location 信息，对标 5.x 中的路由组件的 location 属性
5.2 示例代码：
```js
import React from 'react'
import { useLocation } from 'react-router-dom'

export default function Detail() {
  const x = useLocation()
  // x 就是 location 对象：
  /*
    {
      hash:"",
      key:"ah9nv6sz",
      pathname:"/login",
      search:"?name=zs&age=18",
      state:{a:1,b:2}
    }
  */
  return (
    <ul>
      <li>{id}</li>
      <li>{title}</li>
      <li>{content}</li>
    </ul>
  )
}
=======
>>>>>>> 45a334bebd8e327cb1a83a37555e38fe26540928
```