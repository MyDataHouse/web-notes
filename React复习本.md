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

  

