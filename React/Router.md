# React-Router 路由

先来一个简单的配置

```javascript
import React, { Component } from 'react';
import { BrowserRouter, Route, Switch, Link } from 'react-router-dom';

// 申明三个组件
const Home = () => (
  <div>我是主页</div>
)
const Content = () => (
  <div>我是内容</div>
)
const Log = () => (
  <div>我是日志</div>
)


// Link 标签相当于Vue-router中的 router-link
const Nav = () => (
  <div>
    <Link to='/home'>
    主页
    </Link>
    <Link to='/content'>
    内容
    </Link>
    <Link to='/log'>
    日志
    </Link>
  </div>
)
class App extends Component {
  render() {
    return (
      <div className="App">
        <BrowserRouter> // 表示使用browserRouter
          <Switch> // 表示只显示匹配到的第一个route
            <Route component={Nav} path='/' exact />
            <Route component={Home} path='/home' />
            <Route component={Content} path='/content' />
            <Route component={Log} path='/log' />
          </Switch>
        </BrowserRouter>
      </div>
    );
  }
}

export default App;
```