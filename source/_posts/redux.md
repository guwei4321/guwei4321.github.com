title: Redux源码解析
date: 2018-05-24 13:53:51
tags:
- JavaScript
- React
categories:
- JavaScript
- React
---

版本号 redux@4.0.1

## createStore

`createStore.js` 源码分析如下：
````javascript
import $$observable from 'symbol-observable'

import ActionTypes from './utils/actionTypes'
import isPlainObject from './utils/isPlainObject'

/**
 * 创建一个Redux store 来存储所有state
 * 调用 dispatch 是唯一改变store的方法
 *
 * 应用只有一个单独的store. 通过action指定state tree做出不同的操作，
 * 通过combineReducers方法将将多个reducer合成一个reducer.
 *
 * @param {Function}  reducer 接受当前state和要处理的action，返回新的statetree
reducer 是一个函数,接收两个参数，分别是当前的 state 树和要处理的 action，返回新的 state 树

  [preloadedState]  初始化的state，可以设置store中的默认值 , 可以将服务端传来经过处理后的 state 传给它。
 * 如果使用  combineReducers 来制作root reducer，则必须是一个keys保持一致的普通对象
 
 * @param {Function} [enhancer] 高阶函数，增加返回的 store。与第三方的 middleware 相似，通过函数改变 store 接口。 
 *
 * @returns {Store} 返回一个对象，分别提供 dispatch, getState, subscribe, replaceReducer 四个方法,   
 */
export default function createStore(reducer, preloadedState, enhancer) {
  // 保证 传入的 preloadedState, enhancer 是非函数 
  if (
    (typeof preloadedState === 'function' && typeof enhancer === 'function') ||
    (typeof enhancer === 'function' && typeof arguments[3] === 'function')
  ) {
    throw new Error(
      'It looks like you are passing several store enhancers to ' +
        'createStore(). This is not supported. Instead, compose them ' +
        'together to a single function'
    )
  }
  // 如果第二个参数是函数，则将 preloadedState 赋给 enhancer 
  if (typeof preloadedState === 'function' && typeof enhancer === 'undefined') {
    enhancer = preloadedState
    preloadedState = undefined
  }
  // enhancer必须是函数
  if (typeof enhancer !== 'undefined') {
    if (typeof enhancer !== 'function') {
      throw new Error('Expected the enhancer to be a function.')
    }

    return enhancer(createStore)(reducer, preloadedState)
  }
  // reducer必须是函数
  if (typeof reducer !== 'function') {
    throw new Error('Expected the reducer to be a function.')
  }

  let currentReducer = reducer // 当前的reducer
  let currentState = preloadedState // 当前的 state
  let currentListeners = [] // 当前dispatch将会触发的更新函数数组
  let nextListeners = currentListeners // 下个dispatch将会触发的函数数组 
  let isDispatching = false // 变量开关，是否正在执行dispatch
  
  // 如果 nextListeners 和 currentListeners 是同一个引用，则拷贝一份
  function ensureCanMutateNextListeners() {
    if (nextListeners === currentListeners) {
      nextListeners = currentListeners.slice()
    }
  }

  // 如果正在执行 dispatch 中的函数时，则抛出错误；只有在执行结束后才返回新的state
  function getState() {
    if (isDispatching) {
      throw new Error(
        'You may not call store.getState() while the reducer is executing. ' +
          'The reducer has already received the state as an argument. ' +
          'Pass it down from the top reducer instead of reading it from the store.'
      )
    }

    return currentState
  }

  function subscribe(listener) {
    if (typeof listener !== 'function') {
      throw new Error('Expected the listener to be a function.')
    }
    // 因为执行 dispatch 时会调用 listener，所以在执行dispatch的时候，必须保证 listeners 数组中的订阅更新函数不变
    // 所以在dispatch()执行的时候，订阅还是在取消订阅的时候都不能更新 listeners数组
    if (isDispatching) {
      throw new Error(
        'You may not call store.subscribe() while the reducer is executing. ' +
          'If you would like to be notified after the store has been updated, subscribe from a ' +
          'component and invoke store.getState() in the callback to access the latest state. ' +
          'See https://redux.js.org/api-reference/store#subscribe(listener) for more details.'
      )
    }

    let isSubscribed = true

    ensureCanMutateNextListeners()
    // 将listener推入到nextListeners数组
    nextListeners.push(listener)

    return function unsubscribe() {
      if (!isSubscribed) {
        return
      }
      if (isDispatching) {
        throw new Error(
          'You may not unsubscribe from a store listener while the reducer is executing. ' +
            'See https://redux.js.org/api-reference/store#subscribe(listener) for more details.'
        )
      }

      isSubscribed = false
    // 将listener从nextListeners数组中删除      
      ensureCanMutateNextListeners()
      const index = nextListeners.indexOf(listener)
      nextListeners.splice(index, 1)
    }
  }

    /**
   * action 是对象，改变 state 的唯一方式
   * 返回值：要分发的action
   */
  function dispatch(action) {
    if (!isPlainObject(action)) {
      throw new Error(
        'Actions must be plain objects. ' +
          'Use custom middleware for async actions.'
      )
    }

    if (typeof action.type === 'undefined') {
      throw new Error(
        'Actions may not have an undefined "type" property. ' +
          'Have you misspelled a constant?'
      )
    }

    // 不能同时dispatch 多个 action 函数
    if (isDispatching) {
      throw new Error('Reducers may not dispatch actions.')
    }

    try {
      isDispatching = true
      // 通过reducer函数，获取当前的 state 
      currentState = currentReducer(currentState, action)
    } finally {
      isDispatching = false
    }

    // 遍历调用
    const listeners = (currentListeners = nextListeners)
    for (let i = 0; i < listeners.length; i++) {
      const listener = listeners[i]
      listener()
    }

    return action
  }

  // 替换计算 state的 reducer。
  function replaceReducer(nextReducer) {
    // 必须是个函数
    if (typeof nextReducer !== 'function') {
      throw new Error('Expected the nextReducer to be a function.')
    }
    // 将传入的 currentReducer 赋值给 currentReducer
    currentReducer = nextReducer
    dispatch({ type: ActionTypes.REPLACE })
  }

 
  // 改变 state最小的 observabl
  function observable() {
    const outerSubscribe = subscribe
    return {
      subscribe(observer) {
        if (typeof observer !== 'object' || observer === null) {
          throw new TypeError('Expected the observer to be an object.')
        }
        // 订阅state的更新函数
        function observeState() {
          if (observer.next) {
            observer.next(getState())
          }
        }
        // 取消订阅state的更新函数
        observeState()
        const unsubscribe = outerSubscribe(observeState)
        return { unsubscribe }
      },

      [$$observable]() {
        return this
      }
    }
  }
  // 初始化 默认的 store 里的 statetree
  dispatch({ type: ActionTypes.INIT })

  // 暴露出去的方法
  return {
    dispatch,
    subscribe,
    getState,
    replaceReducer,
    [$$observable]: observable
  }
}
````