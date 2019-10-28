---
title: react-router4 路由的嵌套和动态传值
date: 2018-09-28 10:24:23
tags: [react,react-router]
categories: react
copyright: true

---

![img](react-router4-路由的嵌套和动态传值/timg.jpg)

## 前言

react-router升级4以后有许多的更改，在web端使用的router改为了 react-router-dom,其整个设计思想也更加符合组件化的思想，一切都是组件！所以路由的嵌套也和之前有了较大的变化，react-router-dom的路由嵌套有两种方式。

<!--more-->

## 路由嵌套

### 1.在组件中需要嵌套路由的地方直接使用Route标签书写路由

```react
/*router.js*/

import React, { Component } from 'react'
import { BrowserRouter  as Router, Route, Switch} from 'react-router-dom'
import Home from './pages/home';

export default class Routers extends Component {
  render() {
    return (
      <Router>
        <App>
          <Switch>
            <Route path="/home" exact component={Home}></Route>
          </Switch>
        </App>
      </Router>
    )
  }
}
```

```react
/*Home组件*/

import React, { Component } from 'react'
import { Route, Switch} from 'react-router-dom'
import Menu from './pages/Menu';

export default class Routers extends Component {
  render() {
    return (
      <div>
        <Route path="/home/menu" component={Menu}></Route>
      </div>
    )
  }
}
```

### 2.使用render函数直接嵌套路由

```react
/*router.js*/

import React, { Component } from 'react'
import { BrowserRouter  as Router, Route, Switch} from 'react-router-dom'
import Home from './pages/home';
import Menu from './pages/Menu';

export default class Routers extends Component {
  render() {
    return (
      <Router>
        <App>
          <Switch>
            <Route path="/home" render={()=>
                <Menu>
                    <Route path="/home/menu" component={Menu}></Route>
                <Menu/>
            }></Route>
          </Switch>
        </App>
      </Router>
    )
  }
}
```

## 路由动态传值

路由的传值是通过 react-router中的match对象获取的，具体方法如下：

``` react
/*router.js*/

import React, { Component } from 'react'
import { BrowserRouter  as Router, Route, Switch} from 'react-router-dom'
import Menu from './pages/menu';
import Item1 from './pages/menu/item1';
import Item2 from './pages/menu/item2';

export default class Routers extends Component {
  render() {
    return (
      <Router>
        <App>
          <Switch>
            <Route path="/menu" render={()=>
                <Menu>
                    <Route path="/menu/item1/:id" component={Item1}></Route>
               		<Route path="/menu/item2/:id" component={Item2}></Route>
                <Menu/>
            }></Route>
          </Switch>
        </App>
      </Router>
    )
  }
}
```

```react
/* Menu组件 */
import React, { Component } from 'react'
import { Link } from 'react-router-dom'

export default class Menu extends Component {
  render() {
    return (
      <div>
        <Link to="/menu/item1/123">item1</Link> <br/>
        <Link to="/menu/item2/456">item2</Link>
        <hr/>
        {this.props.children}
      </div>
    )
  }
}

```

```react
/* Item1组件 */
import React, { Component } from 'react'

export default class Item1 extends Component {
  render() {
    return (
      <div>
        this is Item1 page;
        {this.props.match.params.id}  
      </div>
    )
  }
}

```



