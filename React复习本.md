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