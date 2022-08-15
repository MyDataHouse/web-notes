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
  const {provider, consumer} = creatContext('默认值可选类型要和将来传递的值一致')
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
7. 修改状态的值的类型根据初始值的类型要一致

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

### 10.2 useState-使用规则

**目标：**能够为函数组件提供多个状态

**内容：**

+ 如何为函数组件提供多个状态？

  + 调用 `useState` Hook 多次即可，每调用一次 useState Hook 可以提供一个状态
  + `useState Hook` 多次调用返回的 [state, setState]，相互之间，互不影响

+ useState 等 Hook 的使用规则：

  + **React Hooks 只能直接出现在 函数组件 中**
  + **React Hooks不能嵌套在 if/for/其他函数 中**
  + 原理：React 是按照 Hooks 的调用顺序来识别每一个 Hook，如果每次调用的顺序不同，导致 React 无法知道是哪一个 Hook

## 11. useEffect

### 11.1 useEffect-副作用介绍

- 理解：
  - 副作用是相对于主作用来说的，一个功能（比如，函数）除了主作用，其他的作用就是副作用
  - 对于 React 组件来说，==**主作用就是根据数据（state/props）渲染 UI**，**除此之外都是副作用（比如，手动修改 DOM）**==
- 常见的副作用（side effect）：数据（Ajax）请求、手动修改 DOM、localStorage、console.log 操作等

### 11.2 useEffect-基本使用

语法：

- 参数：回调函数（称为 **effect**），就是**在该函数中写副作用代码**

- ==执行时机==：该 effect 会在组件**第一次渲染**以及**每次组件更新**后执行

- 相当于 componentDidMount + componentDidUpdate

```javascript
import { useEffect } from 'react'

useEffect(function effect() {
  document.title = `当前已点击 ${count} 次`
})

useEffect(() => {
  document.title = `当前已点击 ${count} 次`
})
```

### 11.3 useEffect-依赖项

- 默认情况：只要状态发生更新 useEffect 的 effect 回调就会执行

- 性能优化：**跳过不必要的执行，只在 count 变化时，才执行相应的 effect**

- 语法：
  - 第二个参数：可选，也可以传一个数组，数组中的元素可以成为依赖项（deps : depends , 依赖） 
  - 该示例中表示：**只有当 count 改变时，才会重新执行该 effect**

```javascript
 useEffect(() => {
    document.title = `当前已点击 ${count} 次`
  }, [count])
```

### 11.4 useEffect-不要对依赖项撒谎

**目标：**能够理解不正确使用依赖项的后果

**内容：**

- useEffect 回调函数（effect）中用到的数据（比如，count）就是依赖数据，就应该出现在依赖项数组中
- 如果 useEffect 回调函数中用到了某个数据，但是，没有出现在依赖项数组中，就会导致一些 Bug 出现！
- 所以，不要对 useEffect 的依赖撒谎 

### 11.5 useEffect-依赖是一个空数组

**目标：**能够设置useEffect的依赖，让组件只有在第一次渲染后会执行

**内容**：

- useEffect 的第二个参数，还可以是一个**空数组（[]）**，表示只在组件第一次渲染后执行 effect
- ==使用场景==：**1 事件绑定  2 发送请求获取数据 等**
- 语法：
  - 该 effect 只会在组件第一次渲染后执行，因此，可以执行像事件绑定等只需要执行一次的操作
  - 此时，相当于 class 组件的 componentDidMount 钩子函数的作用

### 11.6 useEffect-清理工作

**目标：**能够在组件卸载的时候清除注册的事件

**内容：**

- effect 的**返回值**是可选的，可省略。也可以返回一个**清理函数**，用来执行事件解绑等清理操作
- ==清理函数的执行时机==：
  - **清理函数**会在==组件卸载==时以及==下一次副作用回调函数调用的时候==执行，用于清除上一次的副作用。
  - 如果依赖项为空数组，那么会在组件卸载时会执行。相当于组件的`componetWillUnmount`

### 5.7 useEffect-语法总结 ***

**目标：**能够说出useEffect的 4 种使用使用方式

**内容**：

```js
// 1
// 触发时机：1 第一次渲染会执行 2 每次组件重新渲染都会再次执行
// componentDidMount + ComponentDidUpdate
useEffect(() => {})

// 2（使用频率最高）
// 触发时机：只在组件第一次渲染时执行
// componentDidMount
useEffect(() => {}, [])

// 3（使用频率最高）
// 触发时机：1 第一次渲染会执行 2 当 count 变化时会再次执行
// componentDidMount + componentDidUpdate（判断 count 有没有改变）
useEffect(() => {}, [count])

// 4
useEffect(() => {
  // 返回值函数的执行时机：组件卸载时
  // 在返回的函数中，清理工作
  return () => {
  	// 相当于 componentWillUnmount
  }
}, [])

useEffect(() => {
  
  // 返回值函数的执行时机：1 组件卸载时 2 count 变化时
  // 在返回的函数中，清理工作
  return () => {}
}, [count])
```

## 12. useContext 亲戚组件之间的传值

1. 创建一个单独的的context文件导出创建的context对象

   ```javascript
   import {createContext} from 'react'
   export const Count = createContext()
   ```

2. 引入context文件对子组件传值

   ```javascript
   import {Count} from './cont-Context'
   <Count.Provider value={cehsi}><Count.Provider>
   ```

3. 子组件使用值

   ```javascript
   import {useContext} from 'react'
   import {count} from '../../cont-Context'
   const childrden = () => {
       cosnt cehsi = useContext(count)
       render(){}
   }
   ```

   

## 13. useRef

1. 导入useRef

   ```javascript
   import {useRef} from 'react'
   ```

2. 创建ref对象,使用

   ```javascript
   cosnt inputRef = useRef(null)
   const children = () => {
       handler(){
           inputRef.current.innerHTML
       }
       render(){
           return <h1 ref={inputRef}></h1>
       }
   }
   ```

   

## 14. redux

Redux 是 React 中最常用的**状态管理工具**（状态容器）其他js框架也可以使用

- 与Vue的Vuex是类似的
- redux : */*ˈriːdʌks*/* , 被带回的 , 复活的 , 网络翻译: 状态管理

**内容：**

为了让**代码各部分职责清晰、明确**，Redux 代码被分为三个核心概念：action/reducer/store

- action -> reducer -> store
- **action**（动作）：描述要做的事情
- **reducer**（函数）：更新状态
  - */*rɪˈdjuːsə(r)*/* , 还原剂

- **store**（仓库）：整合 action 和 reducer

### action

特点：

- **只描述做什么**
- **action是一个JS 对象，必须带有 `type` 属性**，用于区分动作的类型
- 根据功能的不同，**可以携带额外的数据**（比如，`payload` 有效载荷，也就是附带的额外的数据），配合该数据来完成相应功能

```javascript
const action = (payload)=> ({type:'increment', payload})
```

### reducer

**特点**：

- 函数签名为：`(prevState, action) => newState`
  - prevState: previous state : 上一个状态
  - 函数签名  , 就是函数的参数是啥 , 返回值是啥 , 其实可以**理解**为**函数的语法**
- 接收上一次的状态和 action 作为参数，根据 action 的类型，执行不同操作，最终返回新的状态
- 注意：**该函数一定要有返回值**，即使状态没有改变也要返回上一次的状态
- 约定：reducer 是一个**纯函数**，并且不能包含 side effect 副作用(比如，不能修改函数参数、不能修改函数外部数据、不能进行异步操作等)
- 纯函数：**相同的输入总是得到相同的输出**
- 对于 reducer 来说，为了保证 reducer 是一个纯函数，需要做到以下不要：
  1. 不要直接修改参数 state 的值（也就是：不要直接修改当前状态，而是根据当前状态值创建新的状态值）
  2. 不要使用 Math.random() / new Date() / Date.now() / ajax 请求等不纯的操作
     - 因为这些函数每次会得到不同的结果
  3. 不要让 reducer 执行副作用（side effect）

```javascript
const reducer = (prevState, action)=> {
    switch (action.type){
        case 'increment':
            return prevState + action.payload
        default :
            return prevState
    }
}
```

### store

**内容：**

- store：仓库，Redux 的核心，整合 action 和 reducer

- 特点：
  - **一个应用只有一个 store**
  - 维护应用的状态，获取状态：`store.getState()`
  - 发起状态更新时，需要分发 action：`store.dispatch(action)`
  - 创建 store 时**接收 reducer 作为参数**：`const store = createStore(reducer)`
- 其他 API，
  - 订阅(监听)状态变化：`const unSubscribe = store.subscribe(() => {}) `
    - subscribe : */səbˈskraɪb*/*  , 订阅
  - 取消订阅状态变化： `unSubscribe()`

```javascript
import { createStore } from 'redux'

const action = (payload)=> ({type:'increment', payload}) //声明了一个action对象
const reducer = (prevState = 0, action)=> { //声明了一个改变action对象的纯函数默认值为0
    switch (action.type){
        case 'increment':
            return prevState + action.payload
        default :
            return prevState
    }
}

const store = createStore(reducer)  //PS: 这一步整合了reducer
store.dispatch(action(2)) //将要改变的action对象传入store
```

### React-Redux-基本使用-全局配置

1. 安装 react-redux：`yarn add react-redux`
2. 从 react-redux 中导入 Provider 组件
   - react-redux中也有Provider , 其实react-redux也是通过context实现的数据共享
3. 导入创建好的 redux 仓库
4. 使用 Provider 包裹整个应用
5. 将导入的 store 设置为 Provider 的 store 属性值

#### 创建redux文件

```javascript
//创建reducer
export const reducer = (state = 0, action) => {
    switch (action.type){
        case 'ceshi':
            return state + 1
            default ：
            return state
    }
}

//在reducers/index.js中
import {reducer} from './reducer'
import {combinReducers} from 'redux'
export default combinReducers({reducer}) //合并多个reducer
```

#### 全局配置redux

安装react-redux 包


```javascript
//在src/index.js中
import ReactDom from 'react-dom/client'
//2.从 react-redux 中导入 Provider 组件
import { Provider } from 'react-redux'
const root = ReactDom.createRoot(document.querySelector('#root'))
//3. 导入创建好的 redux 仓库
import store from './store'
import App from './App'

root.render(
  //4. 使用 Provider 包裹整个应用
    // 全局配置 react-redux，配置后，项目中任何一个组件中，
	// 都可以直接接入 redux 来进行状态管理了
  <Provider store={store}> //5. 将导入的 store 设置为 Provider 的 store 属性值
    <App />
  </Provider>
)
```

#### React-Redux-获取状态useSelector

- `useSelector`：获取 Redux 提供的状态数据
- 参数：selector 函数，用于从 Redux 状态中筛选出需要的状态数据并返回
- 返回值：筛选出的状态

```javascript
import { useSelector } from 'react-redux'

// 计数器案例中，Redux 中的状态是数值，所以，可以直接返回 state 本身
const count = useSelector(state => state)

// 比如，Redux 中的状态是个对象，就可以：
const list = useSelector(state => state.list)

// 在组件中使用
const App = () => {
  const count = useSelector(state => state) //拿到redux中state的默认值
  
  return (
  	<div>
    	<h1>计数器：{count}</h1>
      <button>数值增加</button>
			<button>数值减少</button>
    </div>
  )
}
```

#### React-Redux-分发动作useDispatch

- `useDispatch`：拿到 dispatch 函数，分发 action，修改 redux 中的状态数据
- 语法：

```javascript
import { useDispatch } from 'react-redux'

// 调用 useDispatch hook，拿到 dispatch 函数
const dispatch = useDispatch()

// 调用 dispatch 传入 action，来分发动作
dispatch( action )
```

在子组件中

```javascript
import { useSelector, useDispatch } from 'react-redux'

// actions：
// action creator：函数，通过函数来创建 action 对象
//  1 计数器数值增加，增加多少不确定
const increment = payload => ({ type: 'increment', payload })
//  2 计数器数值减少，减少多少不确定
const decrement = payload => {
  return { type: 'decrement', payload }
}

const App = () => {
  // 调用 useSelector hook 来获取 redux 状态
  const count = useSelector(state => state)
  // 导入 dispatch 函数
  // 此处的 dispatch 函数就相当于 store.dispatch
  // 所以，要想修改状态，只需要 分发 action 即可
  const dispatch = useDispatch()

  return (
    <div>
      <h1>计数器：{count}</h1>
      <button onClick={() => dispatch(increment(2))}>数值增加</button>
      <button onClick={() => dispatch(decrement(5))}>数值减少</button>
    </div>
  )
}

export default App
```

### Redux中间件

默认情况下，Redux 自身只能处理同步数据流。但是在实际项目开发中，状态的更新、获取，通常是使用异步操作来实现。

- 问题：如何在 Redux 中进行异步操作呢? 
- 回答：通过 Redux 中间件机制来实现

#### 中间件原理

```javascript
// 简化写法：
// store 表示：redux 的 store
// next 表示：下一个中间件，如果只使用一个中间，那么 next 就是 store.dispatch（redux 自己的 dispatch 函数）
// action 表示：要分发的动作
const logger = store => next => action => {
  console.log('prev state:', store.getState()) // 更新前的状态
  // 记录日志代码
  console.log('dispatching', action)
  // 如果只使用了一个中间件：
  // 那么，next 就表示原始的 dispatch
  // 也就是：logger中间件包装了 store.dispatch
  let result = next(action)
  // 上面 next 代码执行后，redux 状态就已经更新了，所以，再 getState() 拿到的就是更新后的最新状态值
  // 记录日志代码
  console.log('next state', store.getState()) // 更新后的状态
  return result
}

// 完整写法：
const logger = store => {
  return next => {
    return action => {
      // 中间件代码写在这个位置：
    }
  }
}
```

#### redux-thunk中间件

**目标**：能够使用redux-thunk中间件处理异步操作

**内容**：

`redux-thunk` 中间件可以处理`函数形式的 action`。因此，在函数形式的 action 中就可以执行异步操作

语法：

- thunk action 是一个函数
- 函数包含两个参数：1 dispatch 2 getState

```javascript
// 函数形式的 action
 const thunkAction = () => {
  return (dispatch, getState) => {}
}

// 解释：
export const thunkAction = () => {
  // 注意：此处返回的是一个函数，返回的函数有两个参数：
	// 第一个参数：dispatch 函数，用来分发 action
  // 第二个参数：getState 函数，用来获取 redux 状态
  return (dispatch, getState) => {
    setTimeout(() => {
      // 执行异步操作
      // 在异步操作成功后，可以继续分发对象形式的 action 来更新状态
    }, 1000)
  }
}
```

原理

```javascript
function createThunkMiddleware(extraArgument) {
  // Redux 中间件的写法：const myMiddleware = store => next => action => { /* 此处写 中间件 的代码 */ }
  return ({ dispatch, getState }) => (next) => (action) => {
    // redux-thunk 的核心代码：
    // 判断 action 的类型是不是函数
    // 如果是函数，就调用该函数（action），并且传入了 dispatch 和 getState
    if (typeof action === 'function') {
      return action(dispatch, getState, extraArgument);
    }
    // 如果不是函数，就调用下一个中间件（next），将 action 传递过去
    // 如果没有其他中间件，那么，此处的 next 指的就是：Redux 自己的 dispatch 方法
    return next(action);
  };
}

```

#### @redux-devtools/extension 中间件

**目标**：能够使用chrome开发者工具调试跟踪redux状态

**步骤**：

1. 安装： `yarn add @redux-devtools/extension`
2. 从该中间件中导入 composeWithDevTools 函数
3. 调用该函数，将 applyMiddleware() 作为参数传入

```javascript
import thunk from 'redux-thunk'
import { composeWithDevTools } from '@redux-devtools/extension'
import reducers from './reducers'
const store = createStore(reducer, composeWithDevTools(applyMiddleware(thunk)))

export default store
```

