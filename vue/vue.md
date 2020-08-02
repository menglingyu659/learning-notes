## vue

### 挂载权重：`render > template > el`

### 常用指令

#### `v-html`

#### `v-model`

#### `v-bind:`或`:`

#### `v-for`

#### `v-if`和`v-else-if`和`v-else`

#### `v-show`

#### `v-on:`或`@`

#### `v-slot`

### 常用属性

#### `$parent`

#### `$children`

#### `$root`

#### `$refs`

#### `provide/inject`

#### `Vue.extend`

#### `mixins`

### 非prop特性

#### `$attrs`

#### `$listeners`

### computed会将计算的结果缓存，如果依赖不变会取缓存结果，不会再执行计算函数

```jsx
const vm = new Vue({
    data: {
        a:'a'
    },
	computed: {
		name() {     //只读的计算属性不可通过this.name='mly'来进行赋值
            return this.a
        },
		name1: {    //可读可写的计算属性可通过this.name='mly'来进行赋值
			get(){
                return this.a
            },
            set(val){
                this.a = val
            }
		}
	}
})
```

### 校验库`async-validator`

#### 安装

- #### `yarn add async-validator`

### 全局弹窗组件  ——  可以用`extend`代替下面的`create函数`

```jsx
export function create(Component, props) {
    const vm = new Vue({
        render(h) {
            return h(Component, {props})
        }
    })
    vm.$mount() // 这步骤是为了让vm示例的render函数产生的VNode变成真实的dom节点
    document.body.appendChild(vm.$el)   //通过$el获取真实的dom节点，并将其添加到body中
}
```

### `Extend`方式：

```jsx
export function create(Component, props) {
    const Ctor = Vue.extend(Component);
    const comp = new Ctor({propsData: props})
    comp.$mount() // 这步骤是为了让vm示例的render函数产生的VNode变成真实的dom节点
    document.body.appendChild(comp.$el)   //通过$el获取真实的dom节点，并将其添加到body中
}
```



### 解决问题

#### 设计组件时（`Form组件`）访问不受组件嵌套限制的父组件属性

```tsx
function dipatch(componentName, eventName, props:[]) {
    let parent = this.$parent
    if (!parent) return
    let name = parent.$options.componentName;
    while(parent && (!name || name !== componentName)) {
        parent = parent.$parent;
        if (parent) {
            name = parent.$options.componentName;
        }
    }
    if (parent) {
        parent.$emit(eventName, ...props)
    }
}
```

