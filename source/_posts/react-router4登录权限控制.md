---
title: react-router4登录权限控制
date: 2018-09-28 09:25:12
tags: [react,react-router]
categories: react
copyright: true

---

## 前言

我们在做系统的时候都会遇到登录的问题，那么react框架怎么实现登录权限的控制呢？本文利用react-router-dom和sessionStorage来进行登录权限的控制。

<!--more-->

## 安装react-router-dom

```node.js
yarn add react-router-dom
```

## 入口文件 - index.js 

```react
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import Routers from './router';
import registerServiceWorker from './registerServiceWorker';

ReactDOM.render(<Routers />, document.getElementById('root'));
registerServiceWorker();
```

## 创建容器组件 App

```react
 render() {
    return (
      <div className="App">
        { this.props.children }
      </div>
    );
  }
```

## 创建权限控制组件 AuthRouter组件

**注意：** 要使用 withRouter 强制更新路由信息，否则可能会出现路由地址改变但页面没有相应改变的bug 。

```react
import React, { Component } from 'react'
import { withRouter } from 'react-router'
import { Route, Redirect } from 'react-router-dom'

class AuthRouter extends Component {
    render() {  
        const { component: Component, ...rest } = this.props
        const isLogged = sessionStorage.getItem("isLogin") === "1" ? true : false;
      
        return (
            <Route {...rest} render={props => {
              return isLogged
                  ? <Component {...props} />
                  : <Redirect to="/login" />
            }} />
        )
      }
}

export default withRouter(AuthRouter); 
```

## 创建路由文件-router.js

```react
render(){
      return (
      <Router>
        <App>
          <Switch>
            <Route path="/" exact component={Login}></Route>
            <Route path="/login" component={Login}></Route>
            {/*登录权限控制组件*/}
            <AuthRouter path='/main' component={Home}></AuthRouter>
          </Switch>
        </App>
      </Router>
    )
}
```

## 登录页面组件

引用的antd的UI框架，可以忽略

```react
import React, { Component } from 'react'
import './index.less'
import { Form, Icon, Input, Button } from 'antd'


const FormItem =  Form.Item;
class Login extends Component {
  render() {
    const { getFieldDecorator } = this.props.form;
    return (
      <div className="content">
        <Form className="login-form">
          <FormItem>
            {
              getFieldDecorator('userName',{
                rules: [
                  {
                    required: true,
                    message: '请填写用户名！'
                  }
                ]
              })(
                <Input prefix={
                  <Icon type='user' style={{color:'rgba(0,0,0,.25)'}}/>
                } placeholder='userName'></Input>
              )
            }
          </FormItem>
          <FormItem>
            {
              getFieldDecorator('password',{
                rules: [{required: true, message: "请填写密码！"}]
              })(
                <Input prefix={
                  <Icon type="lock" style={{color:'rgba(0,0,0,.25)'}}/> } placeholder="password"></Input>
              )
            }
          </FormItem>
          <FormItem>
            <Button type="primary" htmlType="submit" className={"btn"} onClick={this.login}>登录</Button>
          </FormItem>
        </Form> 
      </div>
      
    )
  }

  login = (e) =>{
    e.preventDefault();
    this.props.form.validateFields((err,values) => {
      if(!err){
		sessionStorage.setItem("isLogin","1");
      	this.props.history.push('./main');
      }else {
        console.log(err);
      }
    })
  
  }
}

export default Form.create()(Login);
```

至此，我们比较完整的实现了登录权限的问题，也了解了路由控制权限的方法，如果小伙伴们有更好的实现方法，可以互相交流学习 ^_^