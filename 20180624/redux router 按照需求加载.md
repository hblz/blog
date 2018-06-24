redux router 按照需求加载
============================

**说明**
页面数量较多，在部分的单页面的应用来说，首屏渲染速度就会很滞后，使工作效率变得很缓差，如果可以按时加载，就解决了当下的效率底下的问题，只有在切换到对应的页面时才去加载对应的js。
解决此问题只需一点小小的改变，方法很简单，react配合webpack进行安需加载，route的component替换成getComponent，组件的获取方式也进行一点改变使用require.ensure，同时在webpack中配置chunkFilename

----------
**解决办法**
```js
const choosmark = (location, items) => {
    require.ensure([], require => {
        items(null, require('../Component/choosmark').default)
    },'choosmark')
}

const tips = (location, items) => {
    require.ensure([], require => {
        items(null, require('../Component/tips').default)
    },'tips')
}

const  focused = (location, items) => {
    require.ensure([], require => {
        items(null, require('../Component/ focused').default)
    },' focused')
}

const container = (location, items) => {
    require.ensure([], require => {
        items(null, require('../Component/container').default)
    },'container')
}
const RouteConfig = (
    <Router history={history}>
        <Route path="/" component={Roots}>
            <IndexRoute component={index} />//首页
            <Route path="index" component={index} />
            <Route path="tips" getComponent={tips} />//提示部分
            <Route path=" focused" getComponent={ focused} />// focused
            <Route path="container" getComponent={container} />
            <Redirect from='*' to='/'  />
        </Route>
    </Router>
)
```