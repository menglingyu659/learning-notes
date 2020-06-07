## react

### antd

- #### antd按需加载

- #### antd主题配置

### React组件化

#### 资源

- #### [create-react-app](https://www.html.cn/create-react-app/docs/getting-started/)

- #### [HOC](https://reactjs.org/docs/higher-order-components.html)

- #### [ant-design](https://ant.design/docs/react/use-with-create-react-app-cn)

#### 组件化优点

- ##### 增加代码复用性，提高开发效率

- ##### 简化调试步骤，提升整个项目的可维护性

- ##### 便于协同开发

- ##### 要注意降低耦合性

#### 组件跨层级通信——context

- ##### class组件

  1. ##### 只能获取一个context：通过`static contextType = ThemeContext`获取context

  2. ##### 获取一个或多个context：Context.Consumer（）

- ##### 函数组件

  1. ##### 获取一个或多个context：useContext(context)

```jsx
//Context.jsx
import React, {Component} from 'react';
export const ThemeContext = React.createContext();
export const UserContext = React.createContext();
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



#### antd表单组件使用及实现

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

##### `rc-fifield-form`表单组件的使用

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
        console.log("form", this.formRef.current); //sy￾log
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
  

#### 掌握ref

#### HOC

- ##### 装饰器：修饰的必须是一个`class组件`，不能是`函数组件`

  - ##### 修饰顺序从下往上

- ##### 高阶组件注意事项

  - ##### 不要再`render`方法中使用

#### 弹窗组件实现

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

### Redux

#### 实现redux



