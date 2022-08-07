## 1.环境搭建

1. 安装创建react脚手架工具

   ```shell
   npx create-react-app react-basic
   # 或
   npm i create-react-app
   npm init react-app react-basic
   ```

2. 启动项目

   ```shell
   cd ./react-basic #跳转到项目根目录
   npm start #启动项目
   ```

   

## 2.声明式,命令式

- 声明式

  ```javascript
  import ReactDOM from 'react-dom/client'
  
  const container = document.getElementById('root')
  const root = ReactDOM.createRoot(container)
  
  const h1 = (
    <> //跟vue一样要有一个根节点,不希望有的话可以使用空标签
      <h3 className='top'>我的世界</h3>
      <span>物品的</span>
      <div></div>
      <label htmlFor='name'>测试</label>
      <input type='text' id='name' />
    </>
  )
  root.render(h1)
  ```

- 命令式

  ```javascript
  import React from 'react'
  import ReactDOM from 'react-dom/client'
  
  const container = document.getElementById('root')
  const root = ReactDOM.createRoot(container)
  
  const h1 = React.createElement(
    'h1',
    {
      text: '张三'
    },
    '测试'
  )
  root.render(h1)
  ```

## 3.使用注意事项

1. react@18版本弃用了,render方法,新版使用,createRoot方法

   ```javascript
   //before
   import React from 'react'
   import ReactDOM from 'react-dom'
   
   const title = React.creatElement('h1',null,'我是神')
   ReactDOM.render(title,document.getElementById('root'))
   
   //after
   import React from 'react'
   import ReactDOM from 'react-dom/client'
   
   const container = document.getElementById('root')
   const root = ReactDOM.createRoot(container)
   
   const h1 = React.createElement('h1',null,'我是神')
   root.render(h1)
   ```

2. JSX中语法更接近与JavaScript，**属性名采用驼峰命名法**

+ `class` ===> `className`

+ `for` ===>  `htmlFor`

  

3. 通过单花括号来使用表达式(vue使用双花括号)
   - 有值的,可以计算结果并返回的,都可以说是表达式
   - 如果要输出函数本身可以使用{函数名 + ''}的方式

## 4.组件

1. 函数式组件

   - 在不使用hooks的情况下,函数组件是无状态的,数据不能驱动视图
   - 负责页面展示,性能高

   ```javascript
   const Hellow = () => {return <h1>hellow word </h1>}
   root.render(<Hellow />)
   ```

   <mark>组件名必须以大写英文开头</mark>

2. class组件

   - 类组件是有状态组件,数据驱动视图
   - 有自己的数据,比函数组件性能低
   - 修改state中的状态,需要使用setState方法
   - react核心理念,状态不可变(不建议直接修改state中的值)
   
   ```javascript
   class Hellow extends React.Component {
       state:{
           name: '张三'
       }
       ceshi = () => {
           setState(){
               this.state.name = '李四'
           }
       }
       render(){
           return <h1 onClick={this.ceshi}>hellow {this.state.name} </h1>
       }
   }
   
   root.render(<Hellow />)
   ```
   
   

### 4.1 组件分离

```javascript
//在单独的js文件中 ,也可以按需导入,导出
export default const Hellow = () => {return <h1>hellow word </h1>}
                      
//在index.js中
import Hellow from './hellow'
root.render(<Hellow />)                                   
```

### 4.2 表单处理-受控组件

**内容**：

+ HTML中表单元素是可输入的，即表单元素维护着自己的可变状态（value）
+ 但是在react中，可变状态通常是保存在`state`中的，并且要求状态只能通过`setState`进行修改
+ React中将state中的数据与表单元素的value值绑定到了一起，**由state的值来控制表单元素的值**
+ 受控组件：**value值受到了react状态控制的表单元素** 
  - 原生dom中的change事件是在 , 文本框里的内容改变之后 , 失去焦点之后才会触发
  - 但是再react中的change事件 , 是文本框的内容改变之后 , 立马就会触发 (与原生input事件一致)

```javascript
class App extends React.Component {
  state = {
    msg: 'hello react'
  }

  handleChange = (e) => {
    this.setState({
      msg: e.target.value //e.target是下方的input , value就可以获取到input的值
    })
  }

  render() {
    return (
      <div>
        <input type="text" value={this.state.msg} onChange={this.handleChange}/>
      </div>
    )
  }
}
//ps:
//受控组件:
```

### 4.3 非受控组件-ref

**内容**

- 受控组件是通过 React 组件的状态来控制表单元素的值

- 非受控组件是通过**手动操作 DOM 的**方式来控制

- 此时，需要用到一个新的概念：`ref`

- ref：用来在 React 中获取 DOM 元素

  ```javascript
  // 1 导入 createRef 函数（ 用来创建 ref 对象 ）
  import { createRef } from 'react'
  
  class Hello extends Component {
    // 2 调用 createRef 函数来创建一个 ref 对象
    //   ref 对象的名称（txtRef）可以是任意值
    //   命名要规范： txt（DOM 元素的自己标识） + Ref
    txtRef = createRef()
  
    handleClick = () => {
      // 文本框对应的 DOM 元素
      // console.log(this.txtRef.current) //ps:这里的current是固定的属性 , 就是用于获取当前txtRef绑定的dom对象
  
      // 4 获取文本框的值：
      console.log(this.txtRef.current.value)
    }
  
    render() {
      return (
        <div>
          {/*
          	3 将创建好的 ref 对象，设置为 input 标签的 ref 属性值
          		作用：将 ref 和 input DOM 绑定到一起，将来在通过 this.txtRef 拿到的就是当前 DOM 对象
          */}
          <input ref={this.txtRef} />
          <button onClick={this.handleClick}>获取文本框的值</button>
        </div>
      )
    }
  }
  ```

  

## 5.样式处理

```javascript
 <div className={`title ${item.id === 1? 'like' : ''}`}>style样式</div>
 <div className={item.id === 1 ? 'title like' : 'title'}>style样式</div>
 <div className={['title', item.id===1 ? 'like' : ''].join(' ')}>style样式</div>
 import ClassNames from 'classnames'
 <div className={ClassNames('title', {like : item.id === 1})}>style样式</div>
```



## 6. 组件传参

### 6.1父传子

- 要传递数字,使用{}

函数组件

```javascript
// 父组件传递,也可以直接使用{...this.state}传递,react内部会处理成name={this.state.name}
root.render(<Hellow name='张三' tag={18}></Hellow>)
//子组件接收
function Hellow (props){
    console.log(props.name)
}
```



class组件

```javascript
// 父组件传递
root.render(<Hellow name='张三' tag={18}></Hellow>)
//子组件接收
class Hellow extends Component {
    this.props.name
}
```

### 6.2子传父通过回调函数

函数组件

```javascript
// 父组件传递
root.render(<Hellow callback={this.callback}></Hellow>)
//子组件接收
function Hellow (props){
    console.log(props.callback(2))
}
```

class组件

```javascript
// 父组件传递
root.render(<Hellow name='张三' callback={this.callback}></Hellow>)
//子组件接收
class Hellow extends Component {
    this.props.callback(2)
}
```

### 6.3兄弟组件传值

- 将兄弟的共享状态放在父组件中使用

### 6.4 亲戚组件传值context

- 导入creatContext对象

  ```javascript
  import {creatContext, Componet} from 'react'
  ```

  

- 解构出provider,consumer组件

  ```javascript
  const {provider, consumer} = creatContext()
  ```

  

- 传递数据使用provider组件包裹住接收数据的组件,使用value属性传递数据

  ```javascript
  <provider value = {this.state.name}>
  	<Hellow />
  </provider>
  ```

  

- 接收数据者在要使用数据的地方使用consumer组件包裹使用函数接收数据渲染

  ```javascript
  <Hellow>
      <consumer>
        {name => <h1>我的名字叫做{name}</h1>}
      </consumer>
  </Hellow>
  ```

  

### 6.5 props校验(不使用typescript时使用)

1. 安装 prop-types 包

   ```shell
   npm i prop-types
   ```

2. 在需要校验的组件中导入

   ```javascript
   import PropTypes form 'prop-types'
   ```

3. 使用

   ```javascript
   //方式一: List是组件名,params是要传递过来的参数名
   List.propTypes = {
       params: PropTypes.arry
   }
   
   //方式二: 在类组件中添加静态成员,静态成员可以通过类直接访问,而动态成员需要实例化组件才能访问
   class List extends Componet {
       static propTypes = {
           params: PropTypes.arry
       }
   }
   ```

4. 设置默认值

   ```javascript
   List.defaultProps = {
       params: []
   }
   
   //函数组件可以使用参数设置默认值的方式
   List = ({params = 20}) =>{
       return <h1> {params} </h1>
   }
   ```

   

5. 常用规则

   1. 常见类型: array, bool ,func , number , object , string
   2. React元素类型: element
   3. 必填项: isRequired
   4. 特定结构: shape({})

   **核心代码**

   ```jsx
   // 常见类型
   optionalFunc: PropTypes.func,
   // 必选
   requiredFunc: PropTypes.func.isRequired, //方法类型并且必传
   // 特定结构的对象
   optionalObjectWithShape: PropTypes.shape({  //这个对象中必须有color和fontSize属性
   	color: PropTypes.string,
   	fontSize: PropTypes.number
   })
   ```


## 7. 插槽

`通过props的childer属性来实现插槽`

### 7.1 匿名插槽

```javascript
//父组件
class parent extends Component {
    render(){
        return <Hellow>我的世界</Hellow>
    }
}

//子组件
class Hellow extends Component {
    render(){
        return <h1> {this.props.children} </h1>
    }
}
```

### 7.2 作用域插槽

```javascript
//父组件
class parent extends Component {
    handler = (params) => { console.log(params)}
    render(){
        return <Hellow> {this.handler} </Hellow>
    }
}

//子组件
class Hellow extends Component {
    render(){
        return <h1> {this.props.children('参数')} </h1>
    }
}
```

## 8. 生命周期钩子函数(函数组件没有生命周期)

| 钩子函数                                           | 触发时机                | 作用                                                         |
| -------------------------------------------------- | ----------------------- | ------------------------------------------------------------ |
| constructor                                        | 创建组件时,最先执行     | 1. 初始化state  2. 创建 Ref 3. 使用 bind 解决 this 指向问题等 |
| render                                             | 每次组件渲染都会执行    | 渲染UI（**注意： 不能调用setState()** ）                     |
| componentDidMount                                  | 组件挂载(DOM完成渲染)   | 发送网络请求,DOM操作                                         |
| componentDidUpdate(prevProps, prevState, snapshot) | 组件更新(DOM完成渲染后) | DOM操作，可以获取到更新后的DOM内容，**不要直接调用setState** |
| componentWillUnmount                               | 组件卸载(从页面消失)    | 执行清理工作（比如：清理定时器等)                            |

### 组件渲染的时机

1. setState() 更新state数据 ,
2.  forceUpdate() 强制组件更新 , 
3. 组件收到新的props(父向子传递的属性变化)(实际上只要父组件更新,子组件就会重新渲染[只是走一遍代码,通过diff算法判断要不要更新DOM])

## 9. setState进阶

### 11.1 setState更新数据的说明

**内容：**

+ setState 方法更新状态是同步的，但是表现为延迟更新状态（==注意==：**非异步更新状态！！！**）

+ 延迟更新状态的说明：

  - 调用 setState 时，将要更新的状态对象，放到一个更新队列中暂存起来（没有立即更新）

  - 如果多次调用 setState 更新状态，**状态会进行合并，后面覆盖前面**

  - 等到所有的操作都执行完毕，React 会拿到最终的状态，然后触发组件更新

+ 优势：多次调用 setState() ，只会触发一次重新渲染，提升性能

```javascript
state = { count: 1 }

this.setState({
	count: this.state.count + 1
})
console.log(this.state.count) // 1
```

### 11.2 setState 进阶语法

**目标：**能够掌握setState箭头函数的语法，解决多次调用依赖的问题

**内容:**

+  推荐：使用 `setState((prevState) => {})` 语法
   +  previous : /*ˈpriːviəs*/ , 以前的

+  参数 prevState：表示上一次 `setState` 更新后的状态

```javascript
this.setState((prevState) => {
  return {
    count: prevState.count + 1
  }
})
```

### 11.3. setState-第二个参数 (了解,用得少)

**目标：**能够使用setState的回调函数，操作渲染后的DOM

**内容：**

+ 场景：在状态更新（页面完成重新渲染）后立即执行某个操作
+ 语法：`setState(updater[, callback])`

```js
this.setState(
	(state) => ({}),
	() => {console.log('这个回调函数会在状态更新后并且 DOM 更新后执行')}
)
```

### 11.4. setState-同步or异步

**目标：**能够说出setState到底是同步的还是异步

**内容：**

+ setState本身并不是一个异步方法，其之所以会表现出一种“异步”的形式，是因为react框架本身的一个性能优化机制
+ React会将多个setState的调用合并为一个来执行，也就是说，当执行setState的时候，state中的数据并不会马上更新
+ 情况1: setState如果是在react的生命周期中或者是事件处理函数中，表现出来为：延迟合并更新（“异步更新”）--> 假的异步
+ 情况2: setState如果是在setTimeout/setInterval或者原生事件中，表现出来是：立即更新（“同步更新”）

## 10. Hooks

**内容**：

- `Hooks`：钩子、钓钩、钩住
- `Hooks` 是 **React v16.8** 中的新增功能 
- 作用：为**函数组件**提供状态、生命周期等原本 class 组件中提供的 React 功能
  - 可以理解为通过 Hooks 为函数组件钩入 class 组件的特性
- 注意：**Hooks 只能在函数组件中使用**，自此，函数组件成为 React 的新宠儿

### 10.1 useState-基本使用

- 参数：**状态初始值**。比如，传入 0 表示该状态的初始值为 0

- 注意：此处的状态可以是任意值（比如，数值、字符串等），而 class 组件中的 state 必须是对象

**步骤**：

1. 导入 `useState` hook
2. 调用 `useState` 函数，并传入状态的初始值
3. 从 `useState` 函数的返回值中，拿到状态和修改状态的函数
4. 在 JSX 中展示状态
5. 在按钮的点击事件中调用修改状态的函数，来更新状态(使用新的状态值`替换`旧值)
6. 修改状态的时候，一定要**使用新的状态替换旧的状态**

```javascript
// useState hook 的基本使用
import { useState } from 'react'

const App = () => {
  // 调用 useState hook 创建状态,解构数组,数组第一项是初始值,第二项是修改初始值的方法
  const [state,setState] = useState(10)
  return (
    <div>
      <h1>计数器：{state}</h1>
      <button onClick={() => setState(state + 1)}>+1</button>
    </div>
  )
}

export default App
```

