react router V4.0跳转问题
============================

从1.x升级到4.x的时候 遇到路由无法变更页面无法发生跳转问题即：地址栏地址变了页面没有发生跳转需要手动刷新才能生效；
经排查原来是withRouter Switch搞得鬼。
例如：
```js
import React, { Component } from 'react'
import { Router, Route, Switch, withRouter } from 'react-router-dom'
import createHashHistory from 'history/createHashHistory'
import withAuth, { logout, goHomeIfLoggedIn } from 'utils/routes'

import Root from 'root.jsx'
import Home from 'modules/home/components'
import Dashboard from 'modules/home/components/dashboard'
import Login from 'modules/login/components'
import Access from 'modules/_global/error/403/components'
import NotFind from 'modules/_global/error/404/components'
// import Tasks from './tasks'
const RootT = withRouter(Root)
const routes = (
  <RootT>
    <Switch>
      <Route path='/login' component={Login}/>
      <Route path='/logout' onEnter={logout} />
      {/* <Route exact path='/' component={Home} /> */}
      <Home>
        <Route exact path='/' component={Dashboard}/>
        <Route path='/index' component={Dashboard}/>
        <Route path='/403' component={Access}/>
        <Route path='/404' component={NotFind}/>
      </Home>
    </Switch>
  </RootT>
)
export default class extends Component {
  render () {
    return (
      <Router history={createHashHistory()}>
        {routes}
      </Router>
    )
  }
}
// 出现此问题的条件： 
// 1.使用了react-reduct 的connect 
// 2.Router与Root 之间有 有layout布局 元素 
// 解决办法：withRouter 包裹组件 
// 原因：react-reduct shouldComponentUpdate 没有接收到任何属性改变，因此不再重新绘制页面
```
----------------------
withRouter：可以包装任何自定义组件，将react-router 的 history,location,match 三个对象传入。 
无需一级级传递react-router 的属性，当需要用的router 属性的时候，将组件包一层withRouter，就可以拿到需要的路由信息
例如：

```js
import Root from 'root.jsx'
const RootT = withRouter(Root)
const routes = (
  <RootT>
    <Switch>
      <Route path='/login' component={Login}/>
      <Route path='/logout' onEnter={logout} />
      {/* <Route exact path='/' component={Home} /> */}
      <Home>
        <Route exact path='/' component={Dashboard}/>
        <Route path='/index' component={Dashboard}/>
        <Route path='/403' component={Access}/>
        <Route path='/404' component={NotFind}/>
      </Home>
    </Switch>
  </RootT>
)
export default class extends Component {
  render () {
    return (
      <Router history={createHashHistory()}>
        {routes}
      </Router>
    )
  }
}

// 由于Root是当做路由节点顾无法取到路由信息，即在root文件中和其子节点的组件都是获取不到location信息，如果要获取到的话需要给Root包装一层withRouter
```