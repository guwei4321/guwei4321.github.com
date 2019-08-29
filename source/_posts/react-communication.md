title: react-communication
date: 2019-08-15 10:06:44
tags:
---


* 父子组件传通信
    - 父组件通过props传递信息
    - 子组件通过回调函数传递信息
* 跨级组件通信
    - 层层传递props
    - context
* 兄弟组件通信
    - 利用自定义事件

`React`组件式开发的最大好处就是可组合性。所以也会经常遇到组件之间通信的问题，以下内容我们从 父子组件通信 、跨级传递 、兄弟组件通信三个方面来举例说明。

## 父子组件传通信
在`react`中数据流是单向的，自上往下流动的，所以不能从下往上获取数据。所以解决父子组件的双向通信问题的话，我们只能通过以下方法：

1. 父组件通过`props`传递给子组件，子组件就能通过`props`获取父组件传递过来的数据。
2. `props`可以传递的`number、bool、object、string、function`等多个类型的，所以子组件通过调用父组件通过`props`传递过来的`function`类型的回调函数去改变父组件的值。
3. `react`提供了`ref`来获取组件实例，所以结合`props`和`ref`，父组件获取子组件实例的属性和方法。

### 例子
````
// Child.js
import React, { Component } from 'react'

export default class Child extends Component {
    constructor(props){
        super(props);
        this.state = {}
    }
    changeChildTxt = e => {
        this.props.changeChildTxt(e.target.value)
    }
    sayHi () {
        console.log('Hi')
    }
    render(){
        return(
            <div>
                <h1> {this.props.title} </h1>
                <input type="text" 
                onChange={this.changeChildTxt} // 调用父组件传进来的方法
                />
            </div>
        )
    }
}
// Parent.js
import React, { Component } from 'react'
import Child from './Child'

export default class Parent extends Component {
    constructor(){
        super()
        this.state = {
            txt: ''
        }
    }
    changeChildTxt = txt => {
        this.setState({
            txt: txt
        })
    }
    render(){
        return (
            <div>
                <button onClick={()=>{
                    this.ChildRef.sayHi()
                }}>通过ref获取子组件的方法</button>
                <Child 
                    title="1" // 将父组件的属性传递给子组件
                    changeChildTxt={this.changeChildTxt} // 将父组件的方法传递给子组件
                    ref={(ref)=>{
                        this.ChildRef = ref // 将子组件实例赋值给ChildRef
                    }}
                ></Child>
                <p>子组件传的值：{this.state.txt}</p>
            </div>
        )
    }
}
````


## 跨级组件通信
以上我们通过`props`解决父子级组件，也可以通过`props`层层传递来解决跨级通信的问题，但是组件层级深一点，就很难维护，所以我们使用`context`。

### 例子
````
````

`context`是一个全局变量，在任何一个地方都能访问到。但是react官方不建议大量使用`context`，尽管他可以解决`props`层层传递的问题，但是一旦组件复杂，可能很难知道`context`是从哪里传过来的，跟全局变量一样要慎用。


## 兄弟组件通信