---
title: react学习总结20-32
abbrlink: 'conclusionReact20_32'
categories: react
tags: [react]
---

# react视频学习总结 20-32

视频链接：[戳这里](https://www.youtube.com/watch?v=aZGzwEjZrXc&list=PL4cUxeGkcC9gZD-Tvwfod2gaISzfRiP9d&index=21&ab_channel=TheNetNinja) 

视频20是一个组件封装 这里就不细说了

## react-router-dom

react中的路由管理工具：react-router-dom

下载一个稳定的版本，在视频中是version 5:  `npm install react-router-dom@5`

### 各种组件

视频中使用到的组件有 BrowserRouter, Route, Switch 以及 Link

```javascript
import Navbar from './Navbar';
import Home from './Home';
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';

function App(){
    return(
    	<Router>
        	<div className="App">
        		<Navbar/>
        		<div className="content">
        			<Switch>
        				<Route path="/">
        					<Home />
        				</Route>
        			</Switch>
        		</div>
        	</div>
        </Router>
    )
}
```

#### Router

Router(BrowserRouter) 在整个项目中只需要使用一次，在其内部包含Route, Switch 等组件

#### Route

Route的作用就是给对应的组件提供一个路由路径， route中的path参数用来指定路由地址 。route还有一个exact参数，用来设定这个路由是否需要精准匹配（只有在路由路径和path参数的内容完全一样才跳转到这个route）。

当Route没有设定精准匹配时，并且有多个Route在同一级下，react会根据输入的url对同级的Route们从上到下进行遍历，只要其中的Route的path内容被包含在了url中就会被渲染，举例：

```javascript
<Router>
    <Route path="/"> //<Route exact path="/">
        <Home />
    </Route>
        
    <Route path="/create">
        <Create />
    </Route>
        
    <Route path="/help">
        <Help />
    </Route>
</Router>
//当url为 “/create”，或者时"/createNewPage" 会渲染第一个以及第二个组件，也就是 <Home/> 以及<Create/>，因为 “/”  和 “/create” 都被 “/createNewPage” 包含
//当url为 “/help” 时， 会渲染第一个以及第三个组件
//如果你想在url为“/help”时只渲染第三个组件，那么可以给第一个组件添加exact属性
```

#### Switch

Switch放在Route的外部， Router的内部

Switch中一般会有2个或者多个Route组件，因为Switch的主要作用就是保证在同一时间内只会有一个符合路由的Route组件被渲染，举例：

```javascript
//用Route中的例子来说， 当有多个符合url的Route组件时，这些Route组件都会被渲染，但是如果在Route组件外用Switch包裹，那么只会渲染从上到下来看第一个符合url的Route组件

<Router>
    <Switch>
        <Route path="/"> //<Route exact path="/">
            <Home />
        </Route>

        <Route path="/create">
            <Create />
        </Route>

        <Route path="/help">
            <Help/>
        </Route>
    </Switch>
</Router>
//当url为“/create”时， 浏览器只渲染第一个组件，不渲染第二个组件，因为第一个组件已经满足了url的要求了
```

#### Link

Link的使用场景：不刷新页面进行路由跳转，加快请求速度。（如果使用a标签进行路由跳转会重新发送http请求，然后刷新页面）

```javascript
const Navbar = () => {
    return(
    	<nav className = "navbar">
        	<h1>Blogs</h1>
        	<div className = "links">
        		//<a href = "/"> Home </a>
        		<Link to = "/">Home</Link>
        		//<a href = "/creaet"> New Blog </a>
        		<Link to = "/create">New Blog </Link>
        	</div>
        </nav>
    );
}
//将a标签使用Link标签替换掉
//Link的本质上还是a标签
//Link中的to属性和 Route中的path属性 几乎一样。Link会根据to的参数内容取Router内部查找拥有对应path的Route组件，所以得先设有相应path的Route,才能设置有对应to路径的Link
```

#### Link使用的时候会遇到的问题

当你用Link进行快速跳转（故意为难Link，比如点一个按钮在他还没完成渲染的时候立刻点击另一个按钮）时，会遇到Warning类型的报错。

解决办法：创建一个clean up 函数, 放在useEffect钩子函数中

#### AbortController

一个用来中止web请求的容器对象

[介绍](https://developer.mozilla.org/zh-CN/docs/Web/API/AbortController) 

视频24有介绍   （的确解决了问题 不过感觉并不是所有主流浏览器都能使用 所以不细说）

### 带参路由

类似于根据不同的id进行不同页面跳转：  /id/123     /id/456

使用react-router-dom中的useParams进行带参路由的跳转（参数传递）

```javascript
<Router>
    <Switch>
        <Route path="/"> //<Route exact path="/">
            <Home />
        </Route>

        <Route path="/create">
            <Create />
        </Route>

        <Route path="/help">
            <Help/>
        </Route>

		<Route path="/blogs/:id"> //:id就是一个动态变量，作为parameter传递给 Route组件中包含的组件（这里是BlogDetails）
            <BlogDetails/>
        </Route>
    </Switch>
</Router>
```

```javascript
// BlogDetails.js
import {useParams } from "react-router-dom"

const BlogDetails = () => {
    const { id } = useParams();
    
    return (
    	<div className = "blog-details">
        	<h2>Blog details - { id }  </h2>
        </div>
    );
}

export default BlogDetails;
```

也可以在Link中的to中使用带参路由：

```javascript
<Link to={`/blogs/${blog.id}`}>
	<h2> { blog.title }</h2>
</Link>
```

### 表单

在react中使用表单的时候，想像vue一样将输入框的内容和某个变量的值绑定应该怎么做呢？

:no_entry: 错误做法 : 声明一个useState ，将input中的value和useState的参数名绑定 `value={title}`

正确做法：在绑定了value之后，使用onChange 调用 useState中的set函数：  `onChange ={(e) => setTitle(e.target.value)}` 

#### submit请求

在<form>标签内绑定onSubmit事件 并在onSubmit绑定的函数内部使用e.preventDefault阻止默认事件

#### post请求

可以使用axios

也可以使用fetch ，在fetch里面配置method,headers,body等参数

### useHistory

用来重定向网页的路由，跟vue中的$history.push相似的作用

```javascript
import { useHistroy } from 'react-router-dom';

const history = useHistory();
history.go(-1)//go back one through history, 使用数字来表述退回的页面个数
history.push('/')//redirect, 内容是Route组件中定义过的path参数
```





