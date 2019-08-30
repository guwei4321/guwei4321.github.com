title: react-communication
date: 2019-08-15 10:06:44
categories:
- React
tags:
- React
- React实操
---
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
// Bank.js
import React, {Component} from 'react'
import PropTypes from 'prop-types'

export default class Bank extends Component {
    constructor() {
        super();
    }
    // 子组件必须申明使用的context
    static contextTypes = {
        title: PropTypes.string,
        list: PropTypes.array
    }
    render() {
        const {list} = this.props;
        return (
            <div>
                // 通过this.context获取
                <h3>{this.context.title}</h3> 
                <ul>
                    {
                        this.context.list.map((item, index) =>
                            <li key={`list-${index}`}>{item.name}</li>
                        )     
                    }
                </ul>
            </div>
        )
    }
}
// Transfer.js
import React, { Component } from 'react'
import Bank from './Bank'

export default class extends Component {
    render(){
        return(
            <div>
                <Bank />
            </div>
        )
    }
}
// List.js
import React, {Component} from 'react'
import PropTypes from 'prop-types'
import Transfer from './Transfer'

export default class List extends Component {
    constructor(props) {
        super(props);
    }
    // 申明必须支持的context
    static childContextTypes = {
        title: PropTypes.string,
        list: PropTypes.array
    }
    // 返回context对象
    getChildContext() {
        return {
            title: '银行列表',
            list: this.props.list
        }
    }
    render(){
        return (
            <div>
                <Transfer />
            </div>
        )
    }
}
````
除了通过属性`this.context`获取context，React以下方法访问父组件提供的Context：
````
// 构造函数
constructor(props, context)

//比如生命周期：
componentWillReceiveProps(nextProps, nextContext)
shouldComponentUpdate(nextProps, nextState, nextContext)
componetWillUpdate(nextProps, nextState, nextContext)

// 无状态组件
const StatelessComponent = (props, context) => (
  ......
)
````

`context`是父组件链上所有的节点组件都能访问到的属性，只要在顶级组件中定义了`context`，子孙组件都能访问到，可以理解为组件的作用域。但是不建议大量使用`context`，尽管他可以解决`props`层层传递的问题，但是一旦组件复杂，某个组件脱离了中间组件，就获取不到`context`，可用性跟复用性严重降低了，所以要慎用`context`。


## 没有嵌套关系的组件通信
兄弟组件通信，我们先让一个子组件传递数据到父组件，父组件再传值到另一个子组件，父组件相当于充当中转站的角色。但是这样子组件多的话，最外层的根组件可能会充当了很多组件的中转站角色。
所以我们不如直接使用自定义事件，使用发布/订阅模式来实现通信。
### 例子
````
// ChildrenA.js
import React, { Component } from 'react'
import eventBus from './eventBus'


export default class ChildrenA extends Component {
    constructor(){
        super()
        this.state = {
            message: 'message'
        }
    }
    componentDidMount() {
        this.eventBus = eventBus.addListener('changeMessage', (message) => {
            this.setState({
                message
            })
        })
    }
    componentWillUnmount(){
        eventBus.removeListener(this.eventBus)
    }
    render(){
        return(
            <div>
                {this.state.message}
            </div>
        )
    }
}

// ChildrenB.js
import React, { Component } from 'react'
import eventBus from './eventBus'


export default class ChildrenB extends Component {
    constructor () {
        super()
    }
    handleClick = (message) => {
        eventBus.emit('changeMessage', message)
    }
    render (){
        return (
            <div>
                <button onClick={() => this.handleClick('变变变')} >点击我改变ChildrenB的值</button>
            </div>
        )
    }
}
// ParentA.js
import React, { Component } from 'react'
import ChildrenA from './ChildrenA'
import ChildrenB from './ChildrenB'

export default class ParentA extends Component {
    constructor() {
        super()
    }
    render(){
        return (
            <div>
                <ChildrenA />
                <ChildrenB />
            </div>
        )
    }
}
````

##总结
* 父子组件传通信
    - 父组件通过props传递信息
    - 子组件通过回调函数传递信息
* 跨级组件通信
    - 层层传递props
    - context
* 兄弟组件通信
    - 利用自定义事件

或者引入redux，以上所有关系的组件通信问题就都能解决