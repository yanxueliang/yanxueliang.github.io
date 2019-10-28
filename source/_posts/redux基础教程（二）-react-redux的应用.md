---
title: redux基础教程（二）-react-redux的应用
date: 2018-09-28 16:47:24
tags: [react,redux,react-redux]
categories: redux
copyright: true
cover: 2018/09/28/redux基础教程-redux简单应用/2.png
top_img: /img/2.jpg
---

## react-redux简介

React 和 Redux 其实是两个独立的框架，都可以单独的使用。但在react中使用redux的话，就没有理由不使用 react-redux了，因为它可以大大的简化redux的代码书写。

## react-redux核心

react-redux有两个最主要的核心功能

> + connect : 用来链接容器组件和UI组件
> + Provider: 提供包含 store的context

## connect()

connect方法，用于从UI组件生产容器组件，将两个组件连接起来。

```react
import React, { Component } from 'react'
import { connect } from 'react-redux'

class Home extends Component {
    render() {
        return (
        	...
        )
    }
}
            
export default connect(null,null)(Home);
```

connect方法有两个参数：`mapStateToProps` 和`mapDispatchToProps`。通过这两个单词我们也可以了解到前者是将`state`映射到UI的`props`,后者是将UI组件中的一些操作传递给action

### mapStateToProps()

`mapStateToProps`是一个函数，是connect的第一个参数，它接收一个参数`state`,返回一个对象，返回的对象实际就是我们需要获取到的值。

```react
//UI组件代码
// 使用时直接{ this.props.input_value }就可以了。
...
const mapStateToProps = (state) => {
    return {
        input_value: state.default_value
    }
}
            
export default connect(mapStateToProps,null)(Home);
```

### mapDispatchToProps()

`mapDispatchToProps`可以是函数也可以是对象，如果是函数就会有`dispatch`和`ownProps`两个参数，并且return一个对象。

```react
//UI组件代码
import React, { Component } from 'react'
import { connect } from 'react-redux'

class Home extends Component {
    
    handleClick = () => {
       this.props.change();
    }
    render() {
        return (
        	<div>
            	<button onClick={this.handleClick}>点我</button>
            </div>
        )
    }
}

const mapDispatchToProps = (dispatch,ownProps) => {
  return {
    change: () => {
      dispatch({
        type: "VALUE_CHANGE",
        value: ownProps.input_value
      });
    }
  }
}         
export default connect(mapStateToProps,null)(Home);
```

## Provider 组件

`connect`方法生成容器组件以后，需要让容器组件拿到`state`对象，才能生成 UI 组件的参数。

一种解决方法是将`state`对象作为参数，传入容器组件。但是，这样做比较麻烦，尤其是容器组件可能在很深的层级，一级级将`state`传下去就很麻烦。

另外一种就是利用 React-Redux提供的`Provider`组件，在最外层包一层`Provider`组件，这样一来，`App`的所有子组件就默认都可以拿到`state`了。 

```react
import React from 'react';
import ReactDOM from 'react-dom';
import App from './app';
import registerServiceWorker from './registerServiceWorker';
import store from './store'
import { Provider } from 'react-redux';

const Main = (
    <Provider store={store}>
        <App />
    </Provider>
)

ReactDOM.render(Main, document.getElementById('root'));
registerServiceWorker();

```

React-Redux虽然看着有些麻烦，但方便了我们的操作，只要多写几遍这个流程，明白了其中的原理，就能很愉快的使用它了。

参考：[阮一峰的网络日志](http://www.ruanyifeng.com/blog/2016/09/redux_tutorial_part_three_react-redux.html)

