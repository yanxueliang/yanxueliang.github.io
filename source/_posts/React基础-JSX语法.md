---
title: React基础(一)-JSX语法
date: 2018-08-31 15:43:11
tags: [react,JSX]
categories: react
copyright: true
---

## 前言

学习和使用React有一段时间了，最近时间比较充裕，所以想把最近这段时间学习和使用react的心得和经验总结一下，从react的基础用法，到react-router、redux、immutable.js、styled-components等与react配套使用的一系列插件的使用都进行一个总结，写出来给大家分享下，本文只涉及基础用法，对于底层的原理和编译我另外再写文章表述。

## JSX简介

JSX是Facebook专门为React发明的一种新的类似于XML格式的语言，它是JavaScript的语法拓展。它使用XML标记的方式去直接声明界面 ，然后再利用编译器转换成JS语言。

<!--more-->

JSX有一下有点

> + JSX在渲染的时候输出的虚拟 dom，所以 JSX 执行更快
> + 类型安全，在编译过程中就能发现错误
> + 使用 JSX 编写模板更加简单快速

## JSX的基本使用

要使用 JSX 则必须要引入React,React的每一个组件都有render函数，render函数返回的就是JSX渲染的内容，JSX的基本使用个HTML标签没有什么区别。

````jsx
import React, { Component } from 'react';

class App extends Component {
  render() {
    return (
        <div>Hello word!</div>  
    );
  }
}
export default App;

````

## JSX中使用js表达式

我们可以在 JSX 中使用 JavaScript 表达式。表达式写在花括号 **{}** 中。实例如下：

````jsx
class App extends Component {
  render() {
    return (
      <div>
          <div>{1+1}</div>
          <div>{i == 1 ? 'True!' : 'False'}</div>
      </div>  
    );
  }
}
````

## JSX绑定属性值

````jsx
class App extends Component {
  render() {
    return (
     	 <img src={imgUrl}></img>
    );
  }
}

````

## JSX绑定style属性

````jsx
class App extends Component {
  render() {
    return (
     	 <div style={{color:'red', margin:'0 auto'}}></div>
    );
  }
}
````



## JSX注释

JSX的注释要写在花括号中

````jsx
class App extends Component {
  render() {
    return (
        <div>
             {/*这是一条注释...*/}
        	<div>{1+1}</div>
            <div>{i == 1 ? 'True!' : 'False'}</div>
        </div>  
    );
  }
}
````

## JSX插入HTML内容

JSX默认对所有的插入内容都进行文本转义，这样可以有效避免xss攻击 。某些情况下我们需要将不经转义的HTML片段插入到文档流中，这就需要用到JSX的一个属性：dangerouslySetInnerHTML 

````jsx
class App extends Component {
  render() {
    let v = "<h3>这是一段html内容</h3>"
    return (
        <div dangerouslySetInnerHTML={{__html: v}}></div>  
    );
  }
}
````

## JSX占位符：Fragement

在JSX中返回的内容都必须包含在一个大的标签内，所以我们不得不在最外边套一层div，如果我们不希望页面渲染这层div的话就需要使用占位符-Fragement

````jsx
import React, { Component, Fragement} from 'react';

class App extends Component {
  render() {
    return (
        <Fragement>
        	<div>Hello word!</div>
            <div>Hello React!</div>
        </Fragement>  
    );
  }
}
````

## 注意项

### 使用 JSX一定要引入 react

````
import React from 'react';
````

### 组件的首字母一定要大写

为了区别html固定的标签名称，在react中自定义的组件的组件首字母一定要大写，否则会报错。如：

````react
<Header/>   
<Conent></Content>
````

### JSX中的class名称

在JSX中给标签起class名称要用 **className**

````jsx
import React, { Component, Fragement} from 'react';

class App extends Component {
  render() {
    return (
        <Fragement>
        	<div className="title">Hello word!</div>
            <div>Hello React!</div>
        </Fragement>  
    );
  }
}
````



