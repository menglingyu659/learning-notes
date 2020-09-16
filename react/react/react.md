# react



### antd

- #### antd按需加载

- #### antd主题配置



---



### React组件化

#### 资源

##### [create-react-app](https://www.html.cn/create-react-app/docs/getting-started/)

##### [HOC](https://reactjs.org/docs/higher-order-components.html)

##### [ant-design](https://ant.design/docs/react/use-with-create-react-app-cn)



---



#### 组件化优点

- ##### 增加代码复用性，提高开发效率

- ##### 简化调试步骤，提升整个项目的可维护性

- ##### 便于协同开发

- ##### 要注意降低耦合性



----



### 组件跨层级通讯-Context

组件之间可以将数据跨越多个层级进程传输，且可以不用props来传递。子组件指的是在一个组件中引入并应用某一个组件。



举个例子，

```jsx
//错的写法：这种写法的Child是Father的子元素而不是子组件，Father可以称作是Index的子组件
import Child form './Child'
import Father from './Father'
function Index() {
    return <div>
    	<Father>
        	<Child></Child>
        </Father>
    </div>
}
//对的写法
import Child from './Child'
function Father() {
    return <div>
    	<Child></Child>
    </div>
}
```



---



### Context API

#### `React.createContext`

#### `Context.Provider`

#### `Class.contextType`

#### `Context.Consumer`

#### `UseContext`

- #### class组件

  1. ##### 只能获取一个context：通过`static contextType = ThemeContext`获取context

  2. ##### 获取一个或多个context：Context.Consumer（）

- #### 函数组件

  1. ##### 获取一个或多个context：useContext(Context)

```jsx
//Context.jsx
import React, {Component} from 'react';
export const ThemeContext = React.createContext({color:'red'});//默认值
export const UserContext = React.createContext({name:'mly'});
```

```jsx
import React from 'react'
import {ThemeContext, UserContext} from './Context.jsx'
import UseContextTypePage from './UseContextTypePage.jsx'
function Index() {
    const [theme, setTheme] = React.useState({color:'blue'})
    const [user, setUser] = React.useState({name:'li'})
    return <div>
    	<ThemeContext.Provider value={theme}>
            <UserContext.Provider value={user}>
        		<UseContextTypePage></UseContextTypePage>
            </UserContext.Provider>
        </ThemeContext.Provider>
    </div>
}
```

```jsx
//class组件：UseContextTypePage.jsx   static contextType = ThemeContext 只能获取一个context
import React, {Component} from 'react';
import {ThemeContext} from './Context.jsx'
export default class UseContextTypePage extends Component {
    static contextType = ThemeContext  // 获取传递的context绑定到当前组件this.context上
    render() {
        const {themeColor} = this.context;
        console.log(themeColor) //值为UseContextPage.jsx中的'red'
        return (
        	<div>
                {themeColor}
            	<h1>UseContextTypePage</h1>
            </div>
        )
    }
}

//函数组件获取形式  useContext获取一个或者多个context都可以
import React, {useContext} from 'react';
import {ThemeContext, UserContext} from './Context.jsx'
export default function UseContextTypePage(props) {
    const {themeColor} = useContext(ThemeContext)// 获取传递的context作为useContext()执行的返回值
    const {name} = useContext(UserContext)
    console.log(themeColor) //值为UseContextPage.jsx中的'red'
    return (
    	<div>
            {themeColor}
            <h1>UseContextTypePage</h1>
        </div>
    )
}

//class组件：UseContextTypePage.jsx   Context.Consumer可以获取多个context
import React, {Component} from 'react';
import {ThemeContext, UserContext} from './Context.jsx'
export default class UseContextTypePage extends Component {
    render() {
        return (
        	<div>
                <ThemeContext.Consumer>
                	{theme => (
                    	<UserContext.Consumer>
                        	{user => (
                            	<div>{theme}+{user}</div>
                            )}
                        </UserContext.Consumer>
                    )}
                </ThemeContext.Consumer>
            </div>
        )
    }
}
```

```jsx
//UseContextPage.jsx   Context.provider给子组件传递一个或多个context
import React, {Component} from 'react';
import ContextTypePage from './UseContextTypePage.jsx'
import {ThemeContext, UserContext} from './Context.jsx'
export default class UseContextPage extends Component {
    constructor(props) {
        super(props)
        this.state = {
            theme: {
                themeColor: 'red'
            },
            user: {
                name:'孟令雨'
            }
        }
    }
    render() {
        const {theme, user} = this.state
        return (
            <div>
                <h1>ContextPage</h1>
                <ThemeContext.provider value={theme}>  {/*将value值传递给子组件(ContextTypePage)的this.context上,value值变化组件更新传递的context变化，使子组件更新，value值不变组件不更新（浅比较）*/}
                    <UserContext.provider value={user}>
                    	<UseContextTypePage></UseContextTypePage>  
                    </UserContext.provider>
                </ThemeContext.provider>
            </div>
        )
    }
}
```



---



### antd表单组件使用及实现

- ##### ==`antd3.x`的`form`表单局部变化会引起整体变化，`antd4.x`不会==

##### antd表单组件的使用

- ##### 函数组件使用通过自定义hooks引入表单：Form.useForm()

```jsx
import React, {useEffect} from "react";
import {Form, Input, Button} from "antd";
const FormItem = Form.Item;
function FormComponent(props) {
  const [form] = Form.useForm();
  const onFinish = (val) => {
    console.log("onFinish", val); //sy-log
  };
  const onFinishFailed = (val) => {
    console.log("onFinishFailed", val); //sy-log
  };
  const onReset = () => {
    form.resetFields();
  };
    
  useEffect(() => {
    form.setFieldsValue({ name: "default" });
  }, []);
  return (
    <Form form={form} onFinish={onFinish} onFinishFailed={onFinishFailed} onReset={onReset}>
      <FormItem label="姓名" name="name" rules={[nameRules]}>
        <Input placeholder="name input placeholder" />
      </FormItem>
      <FormItem label="密码" name="password" rules={[passworRules]}>
        <Input placeholder="password input placeholder" />
      </FormItem>
      <FormItem>
        <Button type="primary" size="large" htmlType="submit">
          Submit
        </Button>
      </FormItem>
      <FormItem>
        <Button type="default" size="large" htmlType="reset">
          Reset
        </Button>
      </FormItem>
    </Form>
  );
}

```

- ##### class组件使用Form表单：

```jsx
import React, { Component } from "react";
import { Form, Input, Button } from "antd";
const FormItem = Form.Item;
const nameRules = { required: true, message: "请输⼊姓名！" };
const passworRules = { required: true, message: "请输⼊密码！" };
export default class AntdFormPage extends Component {
  formRef = React.createRef();
  componentDidMount() {
    this.formRef.current.setFieldsValue({ name: "default" });
  }
  onReset = () => {
    this.formRef.current.resetFields();
  };
  onFinish = (val) => {
    console.log("onFinish", val); //sy-log
  };
  onFinishFailed = (val) => {
    console.log("onFinishFailed", val); //sy-log
  };
  render() {
    console.log("AntdFormPage render", this.formRef.current); //sy-log
    return (
      <div>
        <h3>AntdFormPage</h3>
        <Form ref={this.formRef} onFinish={this.onFinish} onFinishFailed={this.onFinishFailed} onReset={this.onReset}>
          <FormItem label="姓名" name="name" rules={[nameRules]}>
            <Input placeholder="name input placeholder" />
          </FormItem>
          <FormItem label="密码" name="password" rules={[passworRules]}>
            <Input placeholder="password input placeholder" />
          </FormItem>
          <FormItem>
            <Button type="primary" size="large" htmlType="submit">
              Submit
            </Button>
          </FormItem>
          <FormItem>
            <Button type="default" size="large" htmlType="reset">
              Reset
            </Button>
          </FormItem>
        </Form>
      </div>
    );
  }
}

```



---



### `rc-field-form`表单组件的使用

- ##### `rc-fifield-form`：是reactd提供的第三方表单组件库

  - ##### `antd4.x`的表单基于`rc-fifield-form`实现的，[github源码地址](https://github.com/react-component/field-form)。

  - ##### `antd3.x`的表单基于`rc-form`，`rc-form`是`HOC（高阶组件）`

  - ##### 安装rc-fifield-form，`yarn add rc-field-form`。

  - ##### 函数组件使用`rc-fifield-form`，（==useForm，仅限函数组件使用==）：
  
    ```jsx
  import React, { useEffect } from "react";
    // import Form, {Field} from "rc-field-form";
  import Form, { Field } from "../components/my-rc-field-form/";
    import Input from "../components/Input";
    const nameRules = { required: true, message: "请输⼊姓名！" };
    const passworRules = { required: true, message: "请输⼊密码！" };
    export default function MyRCFieldForm(props) {
      const [form] = Form.useForm();
      const onFinish = (val) => {
        console.log("onFinish", val); //sy-log
      };
      // 表单校验失败执⾏
      const onFinishFailed = (val) => {
        console.log("onFinishFailed", val); //sy-log
      };
      useEffect(() => {
        console.log("form", form); //sy-log
        form.setFieldsValue({ username: "default" });
      }, []);
      return (
        <div>
          <h3>MyRCFieldForm</h3>
          <Form form={form} onFinish={onFinish} onFinishFailed={onFinishFailed}>
            <Field name="username" rules={[nameRules]}>
              <Input placeholder="input UR Username" />
            </Field>
            <Field name="password" rules={[passworRules]}>
              <Input placeholder="input UR Password" />
            </Field>
            <button>Submit</button>
          </Form>
        </div>
      );
    }
    
    ```
  
  - ##### class组件使用`rc-fifield-form`：
  
    ```jsx
  import React, { Component } from "react";
    // import Form, {Field} from "rc-field-form";
  import Form, { Field } from "../components/my-rc-field-form/";
    import Input from "../components/Input";
    const nameRules = { required: true, message: "请输⼊姓名！" };
    const passworRules = { required: true, message: "请输⼊密码！" };
    export default class MyRCFieldForm extends Component {
      formRef = React.createRef();
      componentDidMount() {
        console.log("form", this.formRef.current); //sy-log
        this.formRef.current.setFieldsValue({ username: "default" });
      }
      onFinish = (val) => {
        console.log("onFinish", val); //sy-log
      };
      // 表单校验失败执⾏
      onFinishFailed = (val) => {
        console.log("onFinishFailed", val); //sy-log
      };
      render() {
        return (
          <div>
            <h3>MyRCFieldForm</h3>
            <Form ref={this.formRef} onFinish={this.onFinish} onFinishFailed={this.onFinishFailed}>
              <Field name="username" rules={[nameRules]}>
                <Input placeholder="Username" />
              </Field>
              <Field name="password" rules={[passworRules]}>
                <Input placeholder="Password" />
              </Field>
              <button>Submit</button>
            </Form>
          </div>
        );
      }
    }
    ```
  



---



### 掌握ref

#### HOC

- ##### 装饰器：修饰的必须是一个`class组件`，不能是`函数组件`

  - ##### 修饰顺序从下往上

- ##### 高阶组件注意事项

  - ##### 不要再`render`方法中使用（因为每次都会重新创建这个组件，浪费性能）



---



### 弹窗组件实现

- ##### 传送门(`createPortal`)：react v16之后出现的portal可以实现内容传送功能。将组件加到指定中（如加入`<body></body>中`）

  - ##### 第一个参数为内容，第二个参数为要去的地方

  ```jsx
  // Diallog.js
  import React, { Component } from "react";
  import { createPortal } from "react-dom";
  export default class Dialog extends Component {
    constructor(props) {
      super(props);
      const doc = window.document;
      this.node = doc.createElement("div");
      doc.body.appendChild(this.node);
    }
    componentWillUnmount() {
      window.document.body.removeChild(this.node);
    }
    render() {
      const { hideDialog } = this.props;
      return createPortal(
        <div className="dialog">
          {this.props.children}
          {typeof hideDialog === "function" && <button onClick={hideDialog}>关掉弹窗</button>}
        </div>,
        this.node
      );
    }
  }
  ```



---



### Redux

#### 使用redux

##### 普通使用redux

```jsx
import React, { Component } from "react";
import store from "../store/";
export default class ReduxPage extends Component {
  componentDidMount() {
    store.subscribe(() => {
      this.forceUpdate();
    });
  }
  add = () => {
    store.dispatch({ type: "ADD" });
  };
  minus = () => {
    store.dispatch({ type: "MINUS" });
  };
  render() {
    console.log("store", store); //sy-log
    return (
      <div>
        <h3>ReduxPage</h3>
        <p>{store.getState()}</p>
        <button onClick={this.add}>add</button>
        <button onClick={this.minus}>minus</button>
      </div>
    );
  }
}
```

##### 加入中间件使用redux

`redux-thunk`：实现异步操作(==dispatch的区别，平常传入`{}`，使用`redux-thunk`可传入`function(){}`实现异步==)

`redux-logger`：打印redux日志

```jsx
import { createStore, applyMiddleware } from "redux";
import logger from "redux-logger";
import thunk from "redux-thunk";
import counterReducer from "./counterReducer";
const store = createStore(counterReducer, applyMiddleware(thunk, logger));
```

```jsx
asyAdd = () => {
  store.dispatch((dispatch, getState) => {
    setTimeout(() => {
      // console.log("now ", getState()); //sy-log   //获取当前redux的state
      dispatch({ type: "ADD", payload: 1 });
    }, 1000);
  });
};
```



---



### 实现redux

```jsx
export default function createStore(reducer, enhancer) {
  if (enhancer) {
    return enhancer(createStore)(reducer);
  }
  let currentState;
  let currentListeners = [];
  function getState() {
    return currentState;
  }
  function dispatch(action) {
    currentState = reducer(currentState, action);
    currentListeners.forEach((listener) => listener());
    return action;
  }
  function subscribe(listener) {
    currentListeners.push(listener);
    return () => {
      currentListeners = [];
    };
  }
  dispatch({ type: "KKBREDUX/OOOO" });
  return {
    getState,
    dispatch,
    subscribe,
  };
}
```



---



### 实现中间件 —— `applyMiddleware`

```jsx
export function applyMiddleware(...middleWares) {
  return (createStore) => (reducer) => {
    let store = createStore(reducer);
    let dispatch = store.dispatch;
    store = {
      ...store,
      dispatch: (action) => dispatch(action),
    };
    middleWares = middleWares.map((fn) => fn(store));
    dispatch = compose(...middleWares)(dispatch);
    return {
      ...store,
      dispatch,
    };
  };
}

function compose(...funs) {
  if (funs.length === 0) {
    return (args) => args;
  }
  return funs.reduce((previous, current) => (...arg) => {
    return previous(current(...arg));
  });
}
```

##### 实现`redux-logger` 、`redux-thunk`、`redux-promise`

```jsx
const logger = ({ getState }) => {
  return (next) => (action) => {
    if (typeof action === "function") {
      return next(action);
    }
    console.group(`action ${action.type} @ ${new Date().toLocaleString()}`);
    console.log(`%c prev state `, "color: gray", getState());
    console.log(`%c action    `, "color: aqua", action);
    const returnAction = next(action);
    console.log(`%c next state `, "color: yellowgreen", getState());
    console.groupEnd();
    return returnAction;
  };
};

const thunk = ({ dispatch, getState }) => {
  return (next) => (action) => {
    if (typeof action === "function") {
      return action(dispatch, getState);
    }
    return next(action);
  };
};

import isPromise from "is-promise";  //判断是否是promise
import { isFSA } from "flux-standard-action";  //判断是否是{type:xxx}的格式

const reduxPromise = ({dispatch}) => {
  return (next) => (action) => {
    if (action instanceof Promise) {
      return action.then((act) => {
        return dispatch(act);
      });
    }
    return next(action);
  };
};
```



---



### 函数式编程

函数式编程最基本的运算：**合成（compose）**和**柯里化（Currying）**



#### 合成

如果一个值需要经过多个函数才能变成另外一个值，就可以把所有的中间函数合成一个函数，这个就叫做函数的合成

```js

```



#### 柯里化

把接受多个**参数**的**函数**变成接受一个单一参数（最初函数的第一个函数）的函数，并且返回接受余下的参数而且返回结果的新函数的技术（==多参数变单参数==）

```js
function add(x) {
  let sum = x;
  const inner = (y) => {
    if (y === undefined) {
      return sum;
    }
    sum += y;
    return inner;
  };
  return inner;
}

add(1)(2)(3)();
```





---



### `combineReducers`：聚合reducer

##### 使用

```jsx
import {combineReducers, createStore, applyMiddleware} from 'redux'
function reducer1(state = 0, { type, payload = 1 }) {
  switch (type) {
    case "add":
      return state + payload;
    case "minus":
      return state - payload;
    default:
      return state;
  }
}
const store = createStore(combineReducers({home: reducer1}), applyMiddleware(thunk, logger));

//页面中取值用
store.getState().home
```

##### 实现

```jsx
export function combineReducers(reducers = {}) {
  return (state = {}, action) => {
    for (const prop in reducers) {
      state[prop] = reducers[prop](state[prop], action);
    }
    return state;
  };
}
```



---



### `React-Redux`

#### 使用`react-redux`

#### 资源

##### [react-redux API](https://www.redux.org.cn/docs/react-redux/api.html)

##### [react-redux](https://github.com/reduxjs/react-redux)

```jsx
//Provider  将store传递给子组件
import store from "./store";
import { Provider } from "react-redux";
ReactDOM.render(
  <Provider store={store}>
      <App />
  </Provider>,
  document.getElementById("root")
);

//connect 连接store和子组件的props属性
import React, { Component } from "react";
import { connect } from "react-redux";
class ReactReduxPage extends Component {
  render() {
    const { num, add, minus, asyAdd } = this.props;
    return (
      <div>
        <h1>ReactReduxPage</h1>
        <p>{num}</p>
        <button onClick={add}>add</button>
        <button onClick={minus}>minus</button>
      </div>
    );
  }
}
//------------------------------------------------------------------mapStateToProps(Function)
const mapStateToProps = (state) => {     
  return {
    num: state,
  };
};
//------------------------------------------------------------------


// -----------------------------------------------------------------mapDispatchToProps(Function or Object)
const mapDispatchToProps = { 		    	//写法1
  add: () => ({ type: "add" }),
  minus: () => ({ type: "minus" }),
};
const mapDispatchToProps = (dispatch) => {  //写法2
  return {
    add: () => dispatch({ type: "add" }),
    minus: () => dispatch({ type: "minus" }),
  };
};
import { bindActionCreators } from "redux"; //写法3
const mapDispatchToProps = (dispatch) => {
  let creators = {
    add: () => ({ type: "add" }),
    minus: () => ({ type: "minus" }),
  };
  creators = bindActionCreators(creators, dispatch);
  return { ...creators };
};
//--------------------------------------------------------------------------

export default connect(
  mapStateToProps, //状态映射 mapStateToProps
  mapDispatchToProps //派发事件映射
)(ReactReduxPage);
```



---



### `react-redux`相关的`hook API`

##### `useSelector`：获取store state

##### `useDispatch`：获取store dispatch

- ##### 两种`Hooks`的使用

  ```jsx
  import React, { useCallback } from "react";
  import { useSelector, useDispatch } from "react-redux";
  
  export default function test(props) {
    const store = useSelector((state) => ({
      count: state.count,
    }));
    const dispatch = useDispatch();
    const add = useCallback(() => {
      dispatch({ type: "add" });
    });
    return (
      <div>
        <h3>ReactReduxHookPage</h3>
        <p>{store.count}</p>
        <button onClick={add}>add</button>
      </div>
    );
  }
  ```



---



### 实现`react-redux`

##### API：

- `<Provider></Provider>`
- `connect`
- Hooks：`useSelector` 和 `useDispatch`

```jsx
import React, { useContext, useReducer, useEffect } from "react";
const Context = React.createContext();

const bindActionCreators = (creaters = {}, dispatch) => {
  const returnCreaters = {};
  for (const prop in creaters) {
    returnCreaters[prop] = () => dispatch(creaters[prop]());
  }
  return returnCreaters;
};

export const Provider = ({ store, children }) => {
  return <Context.Provider value={store}>{children}</Context.Provider>;
};

export const connect = (mapStateToProps = () => ({}), mapDispatchToProps) => (Component) => (props) => {
  const { getState, dispatch, subscribe } = useContext(Context);
  const [, setUpdate] = useReducer((x) => x + 1, 0);
  const mutation = () => {
    if (mapDispatchToProps === undefined) return { dispatch };
    if (typeof mapDispatchToProps === "function") {
      return mapDispatchToProps(dispatch);
    } else {
      return bindActionCreators(mapDispatchToProps, dispatch);
    }
  };
  useEffect(() => {
    const unsubscribe = subscribe(() => setUpdate());
    return () => unsubscribe();
  }, [subscribe]);
  const combine = { ...mapStateToProps(getState()), ...mutation() };
  return <Component {...props} {...combine}></Component>;
};

export const useSelector = (selector) => {
  const { getState, subscribe } = useContext(Context);
  const [, setUpdate] = useReducer((x) => x + 1, 0);
  console.log("useDispatch", "dispatch");

  useEffect(() => {
    const unsubscribe = subscribe(() => setUpdate());
    return () => unsubscribe();
  }, [subscribe]);
  return selector(getState());
};

export const useDispatch = () => {
  const { dispatch } = useContext(Context);
  return dispatch;
};
```



---



### Hooks API

#### useReducer

```jsx
import React, { useReducer } from "react";

function reducer(state = 0, action) {
  switch (action.type) {
    case "add":
      return state + 1;
    case "minus":
      return state - 1;
    default:
      return state;
  }
}

function init(initVal) { //接受的参数为useReducer的第二个参数
  return initVal + 51;
}

export default function useReducerTest() {
  const [state, dispatch] = useReducer(reducer, 5, init); //第一个参数是reducer，第二个参数是初始值，第三个参数若存在return值会代替第二个参数作为初始值
  return (
    <div>
      <p>{state}</p>
      <button
        onClick={() => {
          dispatch({ type: "add" });
        }}
      >
        add
      </button>
      <button
        onClick={() => {
          dispatch({ type: "minus" });
        }}
      >
        minus
      </button>
    </div>
  );
}
```



---



### `React-Router`

### [react-router 这个英文文档很好](https://reacttraining.com/react-router/web/guides/quick-start)

#### 简介

- #### react-router包含3个库，react-router、react-router-dom和react-router-native。react-router提供最基本的路由功能，实际使⽤的时候我们不会直接安装react-router，⽽是根据应⽤运⾏的环境选择安装react-router-dom（在浏览器中使⽤）或react-router-native（在rn中使⽤）。react-router-dom和react-router-native都依赖react--router，所以在安装时，react-router也会⾃动安装，创建web应⽤，使用



#### 安装

```shell
yarn add react-router-dom
```



```jsx
<Router>
    <Route exact strict></Route> {/* exact strict共同使用代表不可匹配路径为"/login/"只可匹配"/login"   */}
    <Route sensitive></Route> {/* sensitive代表匹配路径严格按照大小写   */}
</Router>
```



#### 基本使用

- ##### react-router中奉⾏⼀切皆组件的思想，路由器-**Router**、链接-**Link**、路由-**Route**、独占-**Switch**、重定向-**Redirect**都以组件形式存在



---



### **Route**渲染内容的三种⽅式

- ##### Route渲染优先级：children>component>render。

- ##### 三者能接收到同样的[route props]，包括`match`、 `location` 和 `history`，但是当不匹配的时候，==`children`的`match`为null==。

- ##### 这三种⽅式互斥，你只能⽤⼀种，它们的不同之处可以参考下⽂：

  - >**component: component**
  	>
    >##### 只在当location匹配的时候渲染。
    >
    >```jsx
    >{/* 渲染component的时候会调⽤React.createElement，如果使⽤下⾯这种匿名函数的形式，每次都会⽣成⼀个新的匿名的函数，
    >导致⽣成的组件的type总是不相同，这个时候会产⽣重复的卸载和挂载 */}
    ><Route component={() => <TestComponent count= {count} />} />
    ><Route component={TestComponent} />
    >//内部大概原理
    >function Route(props) {
    >    return <props.component></props.component>
    >}
    >```
  
  - >**render**：**func**
    >
    >##### 当你⽤render的时候，内部仅仅是将这个函数调⽤的返回值渲染到页面。但是它和component⼀样，能访问到所有的[route props]。
    >
    >```jsx
    ><Route render={() => <TestComponent count= {count} />} />
    >//内部大概原理
    >function Route(props) {
    >    //每次type值，都是传进来函数的返回值组件（TestComponent）的type，所以每次type都相同不会发生重复卸载和挂载
    >    return {props.render()}
    >}
    >```
    >
    >
  
  - > **children**：**func**
    >
    > ##### 有时候，不管location是否匹配，你都需要渲染⼀些内容，这时候你可以⽤children。除了不管location是否匹配都会被渲染之外，其它⼯作⽅法与render完全⼀样
  
    

---



### 使用

**相关组件**

- BrowserRouter
- Switch
- Route
- Link

**相关API**

- withRouter

**相关Hooks**

- useHistory
- useLocation
- useParams
- useRouteMatch

```jsx
import React from "react";
// import {
//   BrowserRouter as Router,
//   Switch,
//   Route,
//   Link,
//   useRouteMatch,
//   useParams,
//   useLocation,
//   useHistory,
//   withRouter,
// } from "react-router-dom";
import {
  BrowserRouter as Router,
  Switch,
  Route,
  Link,
  useRouteMatch,
  useParams,
  useLocation,
  useHistory,
  withRouter,
} from "./component";

function Home(props) {
  console.log(props);
  return <span>home</span>;
}

const HomeR = withRouter(function (props) {
  // const history = useHistory();
  // const location = useLocation();
  // const match = useRouteMatch();
  // const params = useParams();
  // console.log(history);
  // console.log(location);
  // console.log(match);
  // console.log(params);
  console.log(props);
  return <span>HomeR</span>;
});

function Detail(props) {
  console.log(props);
  return <span>detail</span>;
}

function Info(props) {
  console.log(props);
  return <span>info</span>;
}

function No(props) {
  console.log(props);
  return <div>404</div>;
}

function App() {
  return (
    <div>
      <Router>
        <p>
          <Link to="/123">Home</Link>
        </p>
        <p>
          <Link to="/detail">Detail</Link>
        </p>
        <p>
          <Link to="/info">Info</Link>
        </p>
        <p>
          <Link to="/none">none</Link>
        </p>
        <br />
        <Switch>
          <Route exact path="/:id" children={() => <HomeR a="a"></HomeR>} component={Home}></Route>
          <Route path="/detail" component={Detail}></Route>
          <Route path="/info" component={Info}></Route>
          <Route component={No}></Route>
        </Switch>
      </Router>
    </div>
  );
}

export default App;
```



---



### 实现

git地址：[react-router-dom实现](https://github.com/menglingyu659/react-router)

**相关组件**

- BrowserRouter
- Switch
- Route
- Link

**相关API**

- withRouter

**相关Hooks**

- useHistory
- useLocation
- useParams
- useRouteMatch

**index页面**

```jsx
export { BrowserRouter } from "./BrowserRouter";
export { Link } from "./Link";
export { Route } from "./Route";
export { Switch } from "./Switch";
export { useRouteMatch, useParams, useLocation, useHistory } from "./hooks";
export { withRouter } from "./withRouter";
```



**BrowserRouter**

```jsx
//BrowserRouter.js
import React from "react";
import { createBrowserHistory } from "history";
import Router from "./Router";

export function BrowserRouter(props) {
  const history = createBrowserHistory();
  return <Router history={history} {...props}></Router>;
}

//Router.js
import React, { useState } from "react";
import { RouterContext } from "./context";

export default function ({ history, children }) {
  const [location, setLocation] = useState(history.location);
  history.listen(({ location }) => {
    setLocation(location);
  });
  const match = { path: "/", url: "/", params: {}, isExact: location.pathname === "/" };
  return <RouterContext.Provider value={{ location, history, match }}>{children}</RouterContext.Provider>;
}
```



**Link**

```jsx
import React, { useContext } from "react";
import { RouterContext } from "./context";

export function Link({ to, children }) {
  const { history } = useContext(RouterContext);
  const handleClick = (e) => {
    e.preventDefault();
    e.stopPropagation();
    history.push(to);
  };
  return (
    <a href={to} onClick={handleClick}>
      {children}
    </a>
  );
}
```



**Route**

```jsx
import React, { useContext } from "react";
import { RouterContext } from "./context";
import { matchPath } from "react-router-dom";

export function Route(props) {
  const { path, component, children, render, computerMatch } = props;
  const context = useContext(RouterContext);
  const match = computerMatch
    ? computerMatch
    : path
    ? matchPath(context.location.pathname, props)
    : context.match;
  props = {
    ...context,
    match,
  };
  return (
    <RouterContext.Provider value={props}>
      {match
        ? children
          ? typeof children === "function"
            ? children(props)
            : children
          : component
          ? React.createElement(component, props)
          : render
          ? render(props)
          : null
        : typeof children === "function"
        ? children(props)
        : null}
    </RouterContext.Provider>
  );
}
```



**Switch**

```jsx
import React, { useContext } from "react";
import { matchPath } from "react-router-dom";
import { RouterContext } from "./context";

export function Switch({ children }) {
  const context = useContext(RouterContext);
  let match, element;
  React.Children.forEach(children, (ele) => {
    if (element) return;
    match = ele.props.path ? matchPath(context.location.pathname, ele.props) : context.match;
    if (match) {
      element = ele;
    }
  });

  return match ? React.cloneElement(element, { computerMatch: match }) : null;
}
```



**Hooks**

```jsx
import { useContext } from "react";
import { RouterContext } from "./context";

export const useRouteMatch = () => {
  const context = useContext(RouterContext);
  return context.match;
};
export const useParams = () => {
  const match = useRouteMatch();
  return match.params;
};
export const useLocation = () => {
  const context = useContext(RouterContext);
  return context.location;
};
export const useHistory = () => {
  const context = useContext(RouterContext);
  return context.history;
};
```



**withRouter**

```jsx
import React, { useContext } from "react";
import { RouterContext } from "./context";

export const withRouter = (Component) => (props) => {
  const context = useContext(RouterContext);
  return React.createElement(Component, {
    ...props,
    ...context,
  });
};
```

























