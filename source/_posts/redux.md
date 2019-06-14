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
将多个reducer合并为一个reducer
`combineReducers.js` 源码分析如下：
````javascript
function getUndefinedStateErrorMessage(key, action) {
  const actionType = action && action.type
  const actionDescription =
    (actionType && `action "${String(actionType)}"`) || 'an action'

  return (
    `Given ${actionDescription}, reducer "${key}" returned undefined. ` +
    `To ignore an action, you must explicitly return the previous state. ` +
    `If you want this reducer to hold no value, you can return null instead of undefined.`
  )
}

function getUnexpectedStateShapeWarningMessage(
  inputState,
  reducers,
  action,
  unexpectedKeyCache
) {
  const reducerKeys = Object.keys(reducers)
  const argumentName =
    action && action.type === ActionTypes.INIT
      ? 'preloadedState argument passed to createStore'
      : 'previous state received by the reducer'

  if (reducerKeys.length === 0) {
    return (
      'Store does not have a valid reducer. Make sure the argument passed ' +
      'to combineReducers is an object whose values are reducers.'
    )
  }

  if (!isPlainObject(inputState)) {
    return (
      `The ${argumentName} has unexpected type of "` +
      {}.toString.call(inputState).match(/\s([a-z|A-Z]+)/)[1] +
      `". Expected argument to be an object with the following ` +
      `keys: "${reducerKeys.join('", "')}"`
    )
  }

  const unexpectedKeys = Object.keys(inputState).filter(
    key => !reducers.hasOwnProperty(key) && !unexpectedKeyCache[key]
  )

  unexpectedKeys.forEach(key => {
    unexpectedKeyCache[key] = true
  })

  if (action && action.type === ActionTypes.REPLACE) return

  if (unexpectedKeys.length > 0) {
    return (
      `Unexpected ${unexpectedKeys.length > 1 ? 'keys' : 'key'} ` +
      `"${unexpectedKeys.join('", "')}" found in ${argumentName}. ` +
      `Expected to find one of the known reducer keys instead: ` +
      `"${reducerKeys.join('", "')}". Unexpected keys will be ignored.`
    )
  }
}

// 检查reducers的state是否有默认返回值
function assertReducerShape(reducers) {
  Object.keys(reducers).forEach(key => {
    const reducer = reducers[key]
    // 以默认值来执行reducer 
    const initialState = reducer(undefined, { type: ActionTypes.INIT })

    if (typeof initialState === 'undefined') {
      throw new Error(
        `Reducer "${key}" returned undefined during initialization. ` +
          `If the state passed to the reducer is undefined, you must ` +
          `explicitly return the initial state. The initial state may ` +
          `not be undefined. If you don't want to set a value for this reducer, ` +
          `you can use null instead of undefined.`
      )
    }

    if (
      typeof reducer(undefined, {
        type: ActionTypes.PROBE_UNKNOWN_ACTION()
      }) === 'undefined'
    ) {
      throw new Error(
        `Reducer "${key}" returned undefined when probed with a random type. ` +
          `Don't try to handle ${
            ActionTypes.INIT
          } or other actions in "redux/*" ` +
          `namespace. They are considered private. Instead, you must return the ` +
          `current state for any unknown actions, unless it is undefined, ` +
          `in which case you must return the initial state, regardless of the ` +
          `action type. The initial state may not be undefined, but can be null.`
      )
    }
  })
}

 /**
 将传入的reducers转为，key为reducerName，value为reducer处理函数，形如
 {
   reducerA: funA
   reducerB: funB
 }
 并且生成新的state tree，形如：
 {
   reducerA: {
     key: 'value'
   },
   reducerB: {
     key: 'value'
   }
 }
 */
export default function combineReducers(reducers) {
  const reducerKeys = Object.keys(reducers)
  // 过滤掉reducers中不是function的键值对
  const finalReducers = {}
  for (let i = 0; i < reducerKeys.length; i++) {
    const key = reducerKeys[i]

    if (process.env.NODE_ENV !== 'production') {
      if (typeof reducers[key] === 'undefined') {
        warning(`No reducer provided for key "${key}"`)
      }
    }

    if (typeof reducers[key] === 'function') {
      finalReducers[key] = reducers[key]
    }
  }
  const finalReducerKeys = Object.keys(finalReducers)

  let unexpectedKeyCache
  if (process.env.NODE_ENV !== 'production') {
    unexpectedKeyCache = {}
  }

  let shapeAssertionError
  try {
    assertReducerShape(finalReducers)
  } catch (e) {
    shapeAssertionError = e
  }

  return function combination(state = {}, action) {
    if (shapeAssertionError) {
      throw shapeAssertionError
    }

    if (process.env.NODE_ENV !== 'production') {
      const warningMessage = getUnexpectedStateShapeWarningMessage(
        state,
        finalReducers,
        action,
        unexpectedKeyCache
      )
      if (warningMessage) {
        warning(warningMessage)
      }
    }

    let hasChanged = false
    // 存放最终的 state 树
    const nextState = {}
    for (let i = 0; i < finalReducerKeys.length; i++) {
      // 获取每个reducer的key名
      const key = finalReducerKeys[i]
      // 获取 reducer
      const reducer = finalReducers[key]
      // 获取传入的state树
      const previousStateForKey = state[key]
      // 执行该key的reducer函数，生成新state tree
      const nextStateForKey = reducer(previousStateForKey, action)
      if (typeof nextStateForKey === 'undefined') {
        const errorMessage = getUndefinedStateErrorMessage(key, action)
        throw new Error(errorMessage)
      }
      // 以各自的reducerName作为key名，将新生成的state作为value值，生成最终的state tree
      nextState[key] = nextStateForKey
      // 判断所有的state有没有变化
      hasChanged = hasChanged || nextStateForKey !== previousStateForKey
    }
    // 如果state tree 变化了，就返回新的；否则，返回旧的
    return hasChanged ? nextState : state
  }
}
````

`applyMiddleware.js` 源码分析如下：
回顾下`createStore`方法，`createStore(reducer, preloadedState, enhancer)`，`applyMiddleware`作为第三个参数`enhancer`传入，
````javascript
import compose from './compose'

 /**
 applyMiddleware(thunk)就是 createStore 中的enhancer，负责扩展store
 */
export default function applyMiddleware(...middlewares) {
  return createStore => (...args) => {
    const store = createStore(...args)
    let dispatch = () => {
      throw new Error(
        `Dispatching while constructing your middleware is not allowed. ` +
          `Other middleware would not be applied to this dispatch.`
      )
    }

    const middlewareAPI = {
      getState: store.getState,
      dispatch: (...args) => dispatch(...args)
    }
    // 将 middlewares 作为参数注入，函数科里化后返回新的函数链。
    const chain = middlewares.map(middleware => middleware(middlewareAPI))
    // 以 store.dispatch 来注入 
    dispatch = compose(...chain)(store.dispatch)

    return {
      ...store,
      dispatch
    }
  }
}

````

`bindActionCreators.js` 源码分析如下：
````javascript
function bindActionCreator(actionCreator, dispatch) {
  return function() {
    return dispatch(actionCreator.apply(this, arguments))
  }
}

/**
 将dispatch包装好来直接使用
 */
export default function bindActionCreators(actionCreators, dispatch) {
  if (typeof actionCreators === 'function') {
    return bindActionCreator(actionCreators, dispatch)
  }

  if (typeof actionCreators !== 'object' || actionCreators === null) {
    throw new Error(
      `bindActionCreators expected an object or a function, instead received ${
        actionCreators === null ? 'null' : typeof actionCreators
      }. ` +
        `Did you write "import ActionCreators from" instead of "import * as ActionCreators from"?`
    )
  }
  // 遍历 actionCreators 分别来执行 dispatch
  const keys = Object.keys(actionCreators)
  const boundActionCreators = {}
  for (let i = 0; i < keys.length; i++) {
    const key = keys[i]
    const actionCreator = actionCreators[key]
    if (typeof actionCreator === 'function') {
      boundActionCreators[key] = bindActionCreator(actionCreator, dispatch)
    }
  }
  return boundActionCreators
}

````

`compose.js` 源码分析如下：
````javascript
/**
 传入 Functions 作为参数，返回链式调用的形态。譬如，compose(f, g, h) 最终返回 (...args) => f(g(h(...args)))
*/
export default function compose(...funcs) {
  if (funcs.length === 0) {
    return arg => arg
  }

  if (funcs.length === 1) {
    return funcs[0]
  }

  return funcs.reduce((a, b) => (...args) => a(b(...args)))
}

````