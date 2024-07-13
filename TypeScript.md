## TypeScript特点

- typescript 会在编译时对代码进行严格的静态类型检查，可以在编码阶段就发现问题，而不是在上线运行时才发现
- typeScript 语法遵循 ES 规范，更细速度快，不断支持最新的 ECMAScript 新特性，如装饰器、public/private 修饰符
- typescript 支持 OOP（面向对象）的接口，抽象类，多态特性
- typescript 可以为 IDE 提供更好的代码补全、接口提示、跳转到定义
- 还有重要一点是众多科技公司已经采用 typeScript 进行开发，也是前端工程师需要掌握的就业技能

## 项目安装TS

1. 安装typescript

   ```shell
   npm install -D typescript
   ```

2. 查看版本

   ```shell
   npm tsc -v
   ```

   

## 编译TS

我们使用 tsc 命令可以将 ts 文件编译为 js 文件，如果在编译过程中有 ts 错误将在命令行报出

```shell
tsc hd.ts //会编译生成 hd.js
```

每次修改 ts 文件后再执行命令编译会过于繁琐，可以执行以下命令自动监听 ts 文件内容并自动生成 js 文件

```shell
tsc hd.ts -w
```

## 类型校验

### 指定类型校验

在变量后面使用 : 写明要指定的数据类型 或 指定的值

```javascript
function getTitle(name: 'houdunren' | 'hdcms') {
	return name === 'houdunren' ? '后盾人' : 'hdcms 系统';
}
```

### 类型推断

当没有明确设置类型时，系统会根据值推断变量的类型

下例中系统会根据值推断 hd 变量为 string，当将 hd 设置为 18 的 number 类型时编译时将报误

```javascript
let hd = 'ceshi';
hd = 18
```

```javascript
const user = {name:'后盾人',age:18,open:true,lessons:[
    {title:'linux'},
    {title:'TS'}
]}

// 推断结果
const user: {
    name: string;
    age: number;
    open: boolean;
    lessons: {
        title: string;
    }[];
}
```



### 指定类型校验格式

| 数据类型     | 校验写法                                                     |
| :----------- | ------------------------------------------------------------ |
| 基本数据类型 | let hd : string = 'wode'                                     |
| 数组类型     | let arr : (string \| number)[] = ['wode',12]                 |
| 对象类型     | let obj : {name : string, age : number, open?: boolen} = {name:'ceshi',age:12} |

## 配置文件

TS 支持对编译过程使用配置项自定义，因为下面要讲的有些类型在不同 TS 配置时有差异，所以我们要掌握 TS 配置文件的创建与使用。

### 初始化

```javascript
tsc --init
//项目中使用 npx tsc --init
```

然后执行以下命令使用配置项的定义进行监测

```javascript
tsc -w
//项目中使用 npx tsc -w
```

### 配置选项

|       配置       | 说明                                                   |
| :--------------: | ------------------------------------------------------ |
|  nolmplicitAny   | 禁止使用隐含的any类型, 如函数参数没有设置具体类型      |
| strictNullChecks | 开启时不允许将null, undefined 赋值给其他类型比如字符串 |
|      target      | 转换成 JS 的版本                                       |
|      strict      | 是否严格模式执行                                       |
|      module      | 使用的模块系统                                         |

## 基本类型

除了上面的类型自动推断外，更多的时候是明确设置变量类型

| 数据类型                                                     | 写法                                             |
| ------------------------------------------------------------ | ------------------------------------------------ |
| 字符串                                                       | let hd:string = 'ceshi'                          |
| 数值                                                         | let hd:number = 100                              |
| 布尔                                                         | let hd:boolean = true                            |
| 数组                                                         | let hd:string[] = ['ceshi']                      |
| 元组 (明确数组每个成员值类型的数组为元组)                    | let hd :[string, number, boolean]                |
| 对象                                                         | let hd:object                                    |
| any(设置为any等于使用纯js开发)                               | let hd:any                                       |
| unknown(跟any的区别会进行TS检查, 使用 as 类型断言来明确类型) | let hd:unknown                                   |
| void(void类型的值为 null 或 undefined,常用于对函数返回值类型的定义, 严格模式（tsconfig.json 配置中关闭`strict`）时，void 值只能是 undefined) | const a = (a : string):void =>{}                 |
| never (never没有任何子类型,所以任何类型都不可以赋值给never, 函数抛出异常或无限循环时返回值是 never) | function hd(): never {throw new Error('出错了')} |
| union 联合类型十多个类型的组合,使用 \| 进行连接类似javascript 中的 \|\| | let hd:string \| number = 'ceshi'                |

### 对象

下面是声明对象类型但不限制值类型

```javascript
let hd:object
hd ={name: '后盾人'}
hd = {} //使用字面量声明对象
hd = [] //数组是对象
hd = Object.prototype //原型对象
hd='houdunren' //报错，改变了类型为字符串
```

限定对象值类型

```javascript
let hd:{name: string,year:number}

hd={name:'后盾人',year:2010}
```

属性后面跟上`?` 用来指定 url 为可选值，这样的属性是非必填项

```javascript
let hd:{name: string,year:number,url?:string}
hd={name:'后盾人',year:2010}
```

### 索引签名

如果有明确的索引名称可以使用下面方式来定义签名

```javascript
type HOUDUNREN = {
  name: string
  city: string
}

let hd: HOUDUNREN = {
  name: 'houdunren',
  city: 'beijing'
}
```

如果定义任意属性的签名，可以使用索引签名完成

```javascript
type HOUDUNREN = {
  [key: string]: keyof any
}

let hd: HOUDUNREN = {
  name: 'houdunren'
}
```

下例中我们要求索引后有 Hd 的后缀，则可以使用模板字面量形式

```typescript
type HOUDUNREN = {
  [key: `${string}Hd`]: keyof any
}

let hd: HOUDUNREN = {
  nameHd: 'houdunren'
}
```

当然也可以使用 Record 工具类型来定义

```javascript
type HOUDUNREN = Record<string, string>

let hd: HOUDUNREN = {
  name: 'houdunren'
}
```

Record 可以使用联合类型定义索引

```javascript
type HOUDUNREN = Record<'name' | 'age' | 'city', string>

let hd: HOUDUNREN = {
  name: 'houdunren',
  age: '18',
  city: 'beijing'
}
```

### 交叉类型

交差类型是将 interface、object 等进行合并，组合出新的类型

- interface、object 进行属性合并
- 交叉时要保证类型是一致的，string 与 number 交叉将得到 never 类型

对象类型会进行属性合并

```javascript
interface A { name: string }
type B = { age: number }

let c: A & B = { name: '后盾人', age: 100 }
```

两个类型有相同属性，且类型不同时，返回类型为 never

```javascript
let a = { name: '后盾人' }
let b = { age: 10, name: true }

type HD = typeof a & typeof b

//报错 不能将类型“string”分配给类型“never”。
let c: HD = { age: 30, name: 'houdunren' }
```

上面的问题可以使用 Pick 类型工具移除 name 索引

```typescript
let a = { name: '后盾人' }
let b = { age: 10, name: true }

//通过Pick移除name索引
type HD = typeof a & Pick<typeof b, 'age'>

let c: HD = { age: 30, name: 'houdunren' }
```

通过交叉类型将 **User** 类型组合成新的 **Member** 类型

```typescript
type User = { name: string, age: number }
type Member = { avatar: string } & User

let member: Member = {
  name: 'houdunren', avatar: 'xj.png', age: 30
}
```

下面是属性合并函数的类型定义

```javascript
function merge<T, U>(a: T & U, b: U): T & U {
  for (const key in b) {
    // a[key as Extract<keyof U, string>] = b[key] as any
    a[key] = b[key] as any
  }

  return a;
}
```

string 和 number 因为类型不同，交叉计算后得到 never 类型

```javascript
type HD = string & number;

type HD2 = 'a' & 'b'
```

### 联合交叉类型

```typescript
type HD = ('a' | 'b') & ('a' )  // a  因为字符串'b'与右侧联合类型交叉后得到never，所以被过滤了

type HD2 = ('a' | 'b') & ('a' | string)  // a |b
```

### 剩余参数

下面的求合函数接收多个参数

```typescript
function sum(...args: any[]): number {
    return args.reduce((s, n) => s + n, 0);
}

console.log(sum(1, 2, 3, 4, 5));
```

下面通过第二个参数接收剩余参数，来实现数据追加的示例

```typescript
function push(arr: any[], ...args: any[]): any[] {
    arr.push(...args)
    return arr;
}

const hd: any[] = ['houdunren.com']

console.log(push(hd, '向军', '后盾人')); // [ 'houdunren.com', '向军', '后盾人' ]
```

### 函数重载

函数的参数类型或数量不同时，会有不同的返回值，函数重载就是定义这种不同情况的函数。

**重载签名**

重载签名是对函数多种调用方式的定义，定义不同的函数参数签名与返回值签名，但是没有函数体的实现。

- 使用函数时调用的是重载签名函数，在 vscode 代码跟踪时也会定位到重载签名
- 将从第一个重载签名尝试调用，向下查找是否有匹配的重载签名
- 定义重载签名可以在 idea、vscode 中拥有更好的代码提示

```typescript
function getId(id: string): string;
function getId(id: number): number;
```

**实现签名**

实现签名是是函数功能的实现，对参数与返回值要包扩符合函数签名的宽泛类型。

- 重载签名可以是多个，实现签名只能是一个
- 实现签名是最终执行的函数
- 用户在调用时调用的是重载签名
- 重载签名可被调用，实现签名不能被调用
- 实现签名要使用通用类型

```typescript
//重载签名
function getId(id: string): string;
function getId(id: number): number;

//实现签名
function getId(id: unknown): unknown {
	if (typeof id === 'string') {
		return id;
	}
	return id;
}

//function getId(id: string): string (+1 overload)
getId('后盾人');
```

实现签名要使用通用的类型

```typescript
function getId(id: string): string;
function getId(id: number): number;

//报错：因为实现签名不通用 「不能将类型“unknown”分配给类型“string”」
function getId(id: unknown): string {
	if (typeof id === 'string') {
		return id;
	}
	return id;
}

getId('后盾人');
```

## 断言使用

### Enums枚举

- 不设置值时，值以 0 开始递增

下面是使用枚举设置性别

```typescript
enum SexType {
    BOY, GIRL
}

const hd = {
    name: '后盾人',
    sex: SexType.GIRL
}
console.log(hd); //{ name: '后盾人', sex: 1 }
```

当某个字段设置数值类型的值后，后面的在它基础上递增

```typescript
enum SexType {
    BOY = 1, GIRL
}
...
console.log(hd); //{ name: '后盾人', sex: 2 }
```

可以将值设置为其他类型

```typescript
enum SexType {
    BOY = '男', GIRL = '女'
}
...
console.log(hd); //{ name: '后盾人', sex: '女' }
```

### as 断言

as 断言的意思就是用户断定这是什么类型，不使用系统推断的类型，说白了就是『我说是什么，就是什么』

```typescript
function hd(arg: number) {
  return arg ? 'houdunren' : 2030
}

let f = hd(1) //let f: string | number
```

我们可以由开发者来断定（断言）这就是字符串，这就是断言

```typescript
function hd(arg: number) {
  return arg ? 'houdunren' : 2030
}

let f = hd(1) as string //let f: string
```

也可以使用下面的断言语法

```typescript
function hd(arg: number) {
  return arg ? 'houdunren' : 2030
}

let f = <string>hd(1) //let f: stri
```

### const 断言

#### let & const 

- const 保证该字面量的严格类型
- let 为通用类型比如字符串类型

```typescript
const hd = 'houdunren' //const hd: "houdunren"
let xj = 'houdunren' //let xj: string
```

#### const

`const`断言告诉编译器为表达式推断出它能推断出的最窄或最特定的类型，而不是宽泛的类型

- 字符串、布尔类型转换为具体值
- 对象转换为只读属性
- 数组转换成为只读元组

下面限定 user 类型为最窄类型`houdunren.com`

```typescript
let user = '后盾人' as const
user = 'houdunren.com'

//与以下很相似
let user: 'houdunren' = 'houdunren'
user = 'houdunren'
```

对象转换为只读属性

```typescript
let user = { name: '后盾人' } as const
user.name = 'houdunren' //因为是只读属性，所以不允许设置值
```

当为变量时转换为变量类型，具体值是转为值类型

```typescript
let a = 'houdunren.com'
let b = 2030

let f = [a, b, 'houdurnen.com', true, 100] as const //readonly [string, number, "sina.com", true, 100]
let hd = f[0]
hd = '向军'
```

### 数组赋值

变量 f 得到的类型是数组的类型 string|number，所以只要值是这两个类型都可以

```typescript
let a = 'houdunren.com'
let b = 2039

let hd = [a, b] //let hd: (string | number)[]
let f = hd[1] //let f: string | number
f = '后盾人' //不报错，因为类型是 string | number
```

使用 const 后会得到值的具体类型，面不是数组的类型

```typescript
let a = 'houdunren.com'
let b = 2039

let hd = [a, b] as const //let hd: readonly [string, number]
let f = hd[1] //let f: number
f = '后盾人' //报错，只能是最窄类型即变量 b 的类型 number
```

也可以使用以下语法

```typescript
let a = 'houdunren.com'
let b = 2039

let hd = <const>[a, b] //let hd: readonly [string, number]
let f = hd[1] //let f: number
f = 199
```

### 解构

下面解构得到的变量类型不是具体类型，面是数组类型，比如变量 y 的类型是 *string* | (() => *void*)

这在写项目时是不安全的，因为可以将 y 随时修改为字符串，同时也不会有友好的代码提示

```typescript
function hd() {
  let a = 'houdunren.com'
  let b = (x: number, y: number): number => x + y
  return [a, b]
}
let [n, m] = hd() //变量 m 的类型为 string | (() => void)

m(1, 6) //报错：因为类型可能是字符串，所以不允许调用
```

可以断言 m 为函数然后调用

```typescript
function hd() {
  let a = 'houdunren.com'
  let b = (x: number, y: number): number => x + y
  return [a, b]
}
let [n, m] = hd()
console.log((m as Function)(1, 2))
//使用以下类型声明都是可以的
console.log((m as (x: number, y: number) => number)(1, 5)
```

可以在调用时对返回值断言类型

```typescript
function hd() {
  let a = 'houdunren.com'
  let b = (x: number, y: number): number => x + y
  return [a, b]
}

let [n, m] = hd() as [string, (x: number, y: number) => number]
console.log(m(9, 19))
```

也可以在函数体内声明返回类型

```typescript
function hd() {
  let a = 'houdunren.com'
  let b = (x: number, y: number): number => x + y
  return [a, b] as [typeof a, typeof b]
}

let [n, m] = hd()
console.log(m(9, 19))
```

使用 as const 就可以很高效的解决上面的问题，可以得到具体的类型，来得到更安全的代码，同时会有更好的代码提示

```typescript
function hd() {
  let a = '后盾人'
  let b = (): void => {}
  return [a, b] as const
}

const [x, y] = hd() //变量 y 的类型为 () => void
```

因为 const 是取值的类型，下面代码虽然不报错，但此时 b 的类型已经是 字符串或函数，所以像下面一样在函数调用时 as const 没有意义

```typescript
function hd() {
  let a = 'houdunren.com'
  let b = (x: number, y: number): number => x + y
  return [a, b]
}

const [n, m] = [...hd()] as const
```

### null/undefined

默认情况下 null 与 undefined 可以赋值给其他类型

```typescript
let hd: string = 'houdunren.com'
hd = null
hd = undefined
```

当我们修改 tsconfig.json 配置文件的 strictNullChecks 字段为 true（默认即为 true） 时，则不能将 null、undefined 赋值给其他类型

```typescript
"strictNullChecks": true
```

除非向下面一样明确指定类型

```typescript
let hd: string |undefiend|null = 'houdunren.com'
hd = null
hd = undefined
```

### 非空断言

下面的示例获取的值可能为 HTMLDivElement 或 null，所以直接分配类型“HTMLDivElement”将报错误

> 下例操作需要开启 tsconfig.json 的配置项 strictNullChecks 字段为 true

```typescript
const el: HTMLDivElement = document.querySelector('.hd')
console.log(el.id);
```

可以使用 as 断言类型

```typescript
const el: HTMLDivElement = document.querySelector('.hd') as HTMLDivElement
console.log(el.id);
```

在值后面使用 `!` 来声明值非 null

```typescript
const el: HTMLDivElement = document.querySelector('.hd')!
console.log(el.id);
```

### DOM

为了演示示例我们创建 html 文件如下

- 下面的操作需要开启 tsconfig.json 的配置项 strictNullChecks 字段为 true

```typescript
<!DOCTYPE html>
<html lang="en">
    <head>
        <title>后盾人</title>
        <script src="1.js" defer></script>
    </head>
    <body>
        <div class="hd">houdunren.com</div>
        <button id="bt">插入元素</button>
    </body>
</html>
```

### 类型推断

对于获取的标签对象可能是为 null 也可能是该标签类型

- body 等具体标签可以准确标签类型或 null
- 根据 class 等获取不能准确获取标签类型，推断的类型为 Element|null

```text
const body = document.querySelector('body') //const body: HTMLBodyElement|null
```

下面是根据 class 获取标签结果是 ELement 并不是具体的标签，因为根据 class 无法确定标签类型

```text
const el = document.querySelector('.hd') //const el: Element | null
```

### null 处理

针对于其他标签元素，返回值可能为 null，所以使用 as 断言或！处理

```typescript
let div = document.querySelector('div') as HTMLDivElement//const div: HTMLDivElement
//或使用
div = document.querySelector('div')! //非空断言
console.log(div.id);
```

### 断言处理

使用`as` 将类型声明为 `HTMLAnchorElement` 则 TS 会将其按 a 链接类型处理

- 现在所有的提示将是 a 链接属性或方法

```typescript
const el = document.querySelector('.hd') as HTMLAnchorElement //const el: HTMLAnchorElement
console.log(el.href);
```

下例中的 DOM 类型会报错，因为.hd 是 Element 类型，而构建函数参数 el 的类型是 `HTMLDivElement`

```typescript
class Hd {
    constructor(el: HTMLDivElement) {
    }
}
const el = document.querySelector('.hd'); //el: Element
new Hd(el)
```

这时可以使用 as 断言处理，明确告之获取的值类型是 `HTMLDivElement`

```typescript
class Hd {
    constructor(el: HTMLDivElement) {
    }
}
const el = document.querySelector('.hd') as HTMLDivElement;
new Hd(el)
```

### 事件处理

下面提取按钮元素并添加事件，实现插入元素的功能

```typescript
const body = document.querySelector('body')
const bt = document.querySelector('#bt') as HTMLButtonElement

bt.addEventListener('click', (e: Event) => {
    e.preventDefault(); //因为设置了 e 的类型，所以会有完善的提示
    body.insertAdjacentHTML('beforeend', "<div>后盾人-向军</div>")
})
```

## 类与接口

### 类的定义

使用TS约束属性并实例化对象

```typescript
class User {
    name: string
    age: number
    constructor(n: string, a: number){
        this.name = n
        this.age = a
    }
    
    info(): string {
        return `${this.name}的年龄是${this.age}`
    }
}
console.log(new User('张三', 12))
console.log(new User('李四', 22))
```

### 修饰符

#### public

- 默认情况下属性是 public (公开的) , 既可以在类的内部与外部修改和访问
- 不明确设置修饰符即为 public

```typescript
class User{
    public name: string
    public age: number
    constructor(n: string , a: number){
        this.name = n
        this.age = a
    }
    
    public info(): string{
        return `${this.name}的年龄是 ${this.age}`
    }
}

const hd = new User('张三', 12)
const xj = new User('李四', 22)

hd.name = '王五'

for( const key in hd){
    if(hd.hasOwnProperty(key)){
        console.log(key)
    }
}
```

#### protected

protected 修饰符指受保护的, 只允许在父类与子类中使用, 不允许在类的外部使用

```typescript
class Hd {
    protected name: string
    constructor(name: string) {
        this.name = name
    }
}
class User extends Hd {
    constructor(name: string) {
        super(name)
    }

    public info(): string {
        return `你好 ${this.name}`
    }
}

const hd = new User('后盾人');

console.log(hd.info());
console.log(hd.name); //属性是 protected 不允许访问
```

#### private

private 修饰符指私有的, 不允许在子类与类的外部使用

父类声名 private 属性或方法子类不允许覆盖

```typescript
class Hd {
    private name: string
    constructor(name: string) {
        this.name = name
    }
    private info(): void { }
}
class User extends Hd {
    constructor(name: string) {
        super(name)
    }

    public info(): void {

    }
}
```

子类不能访问父类的 private 属性或方法

```typescript
class Hd {
    private name: string
    constructor(name: string) {
        this.name = name
    }
}
class User extends Hd {
    constructor(name: string) {
        super(name)
    }

    public info(): string {
        return `你好 ${this.name}` //属性是 private 不允许子类访问
    }
}
```

子类更改父类方法或属性的访问修饰符有些限制的

- 父类的 private 不允许子类修改
- 父类的 protected 子类可以修改为 protected 或 public
- 父类的 public 子类只能设置为 public

```typescript
class Hd {
    private name: string
    constructor(name: string) {
        this.name = name
    }
    public info(): void { }
}
class User extends Hd {
    constructor(name: string) {
        super(name)
    }
    protected info(): string {
        return 'houdunren.com'
    }
}
```

#### readonly

redonly 将属性定义为只读, 不允许在类的内部与外部进行修改

- 类似于其他语言的 const 关键字

```typescript
class Axios {
    readonly site: string = 'https://houdunren.com/api'
    constructor(site?: string) {
        this.site = site || this.site
    }
    public get(url: string): any[] {
        console.log(`你正在请求 ${this.site + '/' + url}`)
        return []
    }
}

const instance = new Axios('https://www.houdunwang.com')
instance.get('users')
```

#### constructor

构造函数是初始化实例参数使用的，在 TS 中有些细节与其他程序不同

我们可以在构造函数 constructor 中定义属性，这样就不用在类中声明属性了，可以简化代码量

- 必须要在属性前加上 public、private、readonly 等修饰符才有效

```typescript
class User {
    constructor(
        public name: string,
        public age: number
    ) {}

    public info(): string {
        return `${this.name}的年龄是 ${this.age}`
    }
}

const hd = new User('后盾人', 12);

```

#### static

static 用于定义静态属性或方法，属性或方法是属于构造函数的

- 静态属性是属于构造函数的，不是对象独有的，所以是所有对象都可以共享的

##### 语法介绍

```typescript
class Site {
    static url: string = 'houdunren.com'

    static getSiteInfo() {
        return '我们不断更新视频教程在' + Site.url
    }
}
console.log(Site.getSiteInfo());
```

##### 单列模式

当把 construct 定义为非 public 修饰符后，就不能通过这个类实例化对象了。

```typescript
class User{
    protected constructor(){}
}
const hd = new User() //报错
```

我们可以利用这个特性再结合 static 即可实现单例模式，即只实例化一个对象

```typescript
class User {
    static instance: User | null = null;
    protected constructor() { }

    public static make(): User {
        if (User.instance == null) User.instance = new User;

        return User.instance;
    }
}

const hd = User.make();
console.log(hd);
```

#### get/set

使用 get 与 set 访问器可以动态设置和获取属性，类似于 vue 或 laravel 中的计算属性

```typescript
class User{
    private _name
    constructor(name: string){
        this._name = name
    }
    public get name(){
        return this._name
    }
    public set name(value){
        this._name = value
    }
}

const hd = new User('李四')
hd.name = '张三'
console.log(hd.name)
```

因为 get 与 set 是新特性所以编译时要指定 ES 版本

```typescript
tsc 1.ts -w -t es5
//或修改ts配置文件
```

#### abstract

抽象类定义使用 abstract 关键字，抽象类除了具有普通类的功能外，还可以定义抽象方法

- 抽象类可以不包含抽象方法，但抽象方法必须存在于抽象类中
- 抽象方法是对方法的定义，子类必须实现这个方法
- 抽象类不可以直接使用，只能被继承
- 抽象类类似于类的模板，实现规范的代码定义

```typescript
class Animation {
    protected getPos() {
        return { x: 100, y: 300 }
    }
}

class Tank extends Animation {
    public move(): void {

    }
}

class Player extends Animation {
    public move: void{

}
```

上例中的子类都有 move 方法，我们可以在抽象方法中对其进行规范定义

- 抽象方法只能定义，不能实现，即没有函数体
- 子类必须实现抽象方法

```typescript
abstract class Animation {
    abstract move(): void
    protected getPos() {
        return { x: 100, y: 300 }
    }
}

class Tank extends Animation {
    public move(): void {

    }
}

class Player extends Animation {
    public move(): void {

    }
}
```

子类必须实现抽象类定义的抽象属性

```typescript
abstract class Animation {
    abstract move(): void
    abstract name: string
    protected getPos() {
        return { x: 100, y: 300 }
    }
}
class Tank extends Animation {
    name: string = '坦克'
    public move(): void {

    }
}

class Player extends Animation {
    name: string = '玩家'

    public move(): void {

    }
}
```

抽象类不能被直接使用，只能被继承

```typescript
abstract class Animation {
    abstract move(): void
    protected getPos() {
        return { x: 100, y: 300 }
    }
}
const hd = new Animation(); //报错，不能通过抽象方法创建实例
```

#### interface

接口用于描述类和对象的结构

- 使项目中不同文件使用的对象保持统一的规范
- 使用接口也会有更好的代码提示

##### 抽象类

下面是抽象类与接口的结合使用

```typescript
interface AnimationInterface {
    name: string
    move(): void  //接口不能有具体的函数实现
}
abstract class Animation {
    protected getPos(): { x: number; y: number } {
        return { x: 100, y: 300 }
    }
}

class Tank extends Animation implements AnimationInterface {
    name: string = '敌方坦克'
    public move(): void {
        console.log(`${this.name}移动`)
    }
}

class Player extends Animation {
    name: string = '玩家'
    public move(): void {
        console.log(`${this.name}坦克移动`)
    }
}
const hd = new Tank()
const play = new Player()
hd.move()
play.move()
```

##### 对象

下面使用接口来约束对象

```typescript
interface UserInterface {
    name: string;
    age: number;
    isLock: boolean;
    info(other:string): string,
}

const hd: UserInterface = {
    name: '后盾人',
    age: 18,
    isLock: false,
    info(o:string) {
        return `${this.name}已经${this.age}岁了,${o}`
    },
}

console.log(hd.info());
```

如果尝试添加一个接口中不存在的函数将报错，移除接口的属性也将报错。

```typescript
const hd: UserInterface = {
		...
    houdurnen() { }  //“houdurnen”不在类型“UserInterface”中
}
```

如果有额外的属性，使用以下方式声明，这样就可以添加任意属性了

```typescript
interface UserInterface {
    name: string;
    age: number;
    isLock: boolean;
    [key:string]:any
}
```

##### 接口继承

下面定义游戏结束的接口 PlayEndInterface , AnimationInterface 接口可以使用 extends 来继承该接口

```typescript
interface PlayEndInterface {
    end(): void
}
interface AnimationInterface extends PlayEndInterface {
    name: string
    move(): void
}

class Animation {
    protected getPos(): { x: number; y: number } {
        return { x: 100, y: 300 }
    }
}

class Tank extends Animation implements AnimationInterface {
    name: string = '敌方坦克'
    public move(): void {
        console.log(`${this.name}移动`)
    }
    end() {
        console.log('游戏结束');
    }
}

class Player extends Animation implements AnimationInterface {
    name: string = '玩家'
    public move(): void {
        console.log(`${this.name}坦克移动`)
    }
    end() {
        console.log('游戏结束');
    }
}
const hd = new Tank()
const play = new Player()
hd.move()
play.move()
```

对象可以使用实现多个接口，多个接口用逗号连接

```typescript
interface PlayEndInterface {
    end(): void
}
interface AnimationInterface {
    name: string
    move(): void
}

class Animation {
    protected getPos(): { x: number; y: number } {
        return { x: 100, y: 300 }
    }
}

class Tank extends Animation implements AnimationInterface, PlayEndInterface {
    name: string = '敌方坦克'
    public move(): void {
        console.log(`${this.name}移动`)
    }
    end() {
        console.log('游戏结束');
    }
}

class Player extends Animation implements AnimationInterface, PlayEndInterface {
    name: string = '玩家'
    public move(): void {
        console.log(`${this.name}坦克移动`)
    }
    end() {
        console.log('游戏结束');
    }
}
const hd = new Tank()
const play = new Player()
hd.move()
play.move()
```

##### 函数

下面是用 UserInterface 接口约束函数的参数与返回值

- 会根据溃口规范提示代码提示
- 严格约束参数类型,维护代码安全

###### 函数参数

下面是对函数参数的类型约束

```typescript
interface UserInterface {
    name: string;
    age: number;
    isLock: boolean;
}

function lockUser(user: UserInterface, state: boolean): UserInterface {
    user.isLock = state;
    return user;
}

let user: UserInterface = {
    name: '后盾人', age: 18, isLock: false
}

lockUser(user, true);
console.log(user);
```

###### 函数声明

使用接口可以约束函数的定义

```typescript
interface Pay{
    (price: number): boolean
}
const getUserInfo: Pay = (price: number) => true
```

###### 构造函数

下面的代码我们发现需要在多个地方使用对 user 类型的定义

```typescript
class User{
    info: {name: string, age: nubmer}
    constructor(user: {name: string, age: number}){
        this.info = user 
    }
}
const hd = new User({name:'张三', age: 12})
console.log(hd)
```

使用 interface 可以优化代码,同时有良好的代码提示

```typescript
interface UserInterface {
    name: string,
    age: number
}
class User{
    info: UserIngerface
    constuctor(user: UserInterface){
        this.info = user
    }
}
const hd = new User({name: '张三', age: 12})
console.log(hd)
```

###### 数组

对数组类型使用接口进行约束

```typescript
interface UserInterface {
  name: string
  age: number
  isLock: boolean
}

const hd: UserInterface = {
    name: '张三',
    age: 12,
    isLock: false
}

const xj: UserInterface = {
    name: '李四',
    age:33,
    isLock: false
}

const users: UserInterface[]=[]
users.push(hd,xj)
console.log(users)
//[{ name: '张三', age: 12, isLock: false },{ name: '李四', age: 33, isLock: false }]
```

###### 枚举

下面是使用枚举设置性别

```typescript
enum SexType{
    BOY, GIRL
}

interface UserInterface{
    name: string,
    sex: SexType
}

const hd: UserInterface = {
    name: '张三'
    sex: SexType.BOY
}

console.log(hd) // { name: '后盾人', sex: 0 }
```

#### type

type 与 interface 非常相似都可以描述一个对象或者函数，使用 type 用于定义类型的别名，是非常灵活的类型定义方式。

- type 可以定义基本类型别名如联合类型，元组
- type 与 interface 都是可以进行扩展
- 使用 type 相比 interface 更灵活
- 如果你熟悉其他编程语言，使用 interface 会让你更亲切
- 使用类(class) 时建议使用接口，这可以与其他编程语言保持统一
- 决定使用哪个方式声明类型，最终还是看公司团队的规范

##### 基本使用

下面是使用 type 声名对象类型

```typescript
type User = {
    name: string,
    age: number
}
const hd: User = {name: '张三', age: 12}
```

上面已经讲解了使用 interface 声明函数，下面来看使用 type 声明函数的方式

```typescript
type Pay = (price: number) => boolean
cosnt wepay: Pay = (price: number) =>{
    console.log(`微信支付${price}`)
    return true
}

wepay(100)
```

##### 类型别名

type可以为 number, string, boolean, object 等基本类型定义别名,比如下列的IsAdmin

```typescript
//基本类型别名
type IsAdmin = boolean

//定义联合类型
type Sex = 'boy' | 'girl'

type User = {
    isAdmin: IsAdmin,
    sex: Sex
}
const hd: User = {
    isAdmin: true,
    sex: "boy"
}

//声明元组
const users: [User] = [hd]
```

##### 索引类型

type 与 interface 在索引类型上的声名是相同的

```typescript
interface User {
    [key: string]:any
}

type UserType = {
    [key: string] = any
}
```

##### 声名继承

typescript 会将同名接口声名进行合并

```typescript
interface User {
    name: string
}
interface User {
    age: number
}
const hd: User ={
    name:'张三',
    age:12
}
```

interface 可以 extends 继承 type

```typescript
type Admin = {
    role: string
}
interface User extends Admin {
    name: string
}
const hd: User ={
    role: 'admin',
    name: '张三'
}
```

interface 也可以 extends 继承 interface

```typescript
interface Admin {
    role:string
}
interface User extends Admin {
    name: string
}
const hd: User ={
    role: 'admin',
    name: '张三'
}
```

type 与 interface 不同 , 存在同名的 type  时将是不允许的

```typescript
type User {
	name: string
}
type User { // 会报错
	age: number
}
```

不过可以使用 & 来进行 type 合并

```typescript
type Name = {
    role: string
}
type Menber = {
    age: number
}
type ceshi = Name & Menber
const hd:ceshi = {
    role:'张三',
    age:12
}
```

下面的声名满足任何一个 type 都可以

```typescript
type Admin = {
    role: string
}
type Member = {
    age: number
}

type User = Admin | Member
const hd: User = {
    role:'张三'
}
```

##### implements

class 可以使用 implements 来实现 type 或 interface

```typescript
type Member = {
    name: string
}
class User implements Member {
    name:string ='张三'
}
```

## 泛型

泛型指使用时才定义类型, 即类型可以像参数一样定义, 主要解决类, 接口, 函数的复用性, 让它们可以处理多种类型

### 基本使用

```typescript
function dump<T>(arg: T): T{
    return arg
}

console.log(dump<string>('张三')) //这里也可以不使用<string>它会自动推断
```

### 类型继承

```typescript
//因为编译器不知道T上有length方法没有,所以要继承有length方法的类型,或自定义{length: number}
function getLength<T extends string | any[]>(arg: T): number{
    return arg.length
}

console.log(getLength('zhangsan'))
console.log(getLength(['张三','李四']))
```

## 装饰器

装饰器 (Decorators) 为我们在类的声名及成员上通过编程语法拓展其功能,装饰器一函数的形式声明

完整的装饰器功能配合 `reflect-metadata` 以实现依赖收集注入功能

### 装饰器类型

可用装饰器包含以下几种

|     **装饰器**     |  **说明**  | 默认收集元数据                                  |
| :----------------: | :--------: | ----------------------------------------------- |
|   ClassDecorator   |  类装饰器  | design:paramtypes                               |
|  MethodDecorator   | 方法装饰器 | design:paramtypes design:returntype design:type |
| PropertyDecorator  | 属性装饰器 | design:type                                     |
| ParameterDecorator | 参数装饰器 |                                                 |

### 该方法是实验性功能TS需要开启以下配置

```typescript
"experimentalDecorators": true,
"emitDecoratorMetadata": true
```

### 执行顺序

洋葱模型

- 不同类型装饰器 从上到下，参数优先，类在最后
- 一个地方同时使用多个装饰器 工厂函数从上到下，装饰器从下到上

### 类装饰器

类装饰器是对类的功能进行扩展

- 首先执行 RoleDecorator 装饰器，然后执行类的构造函数
- 装饰器会优先执行，这与装饰器与类的顺序无关

#### 装饰器参数

| **参数** | **说明** |
| -------- | -------- |
| 参数一   | 构造函数 |

- 普通方法是构造函数的原型对象 Prototype
- 静态方法是构造函数

#### 使用方法

```typescript
//拓展原有类
const MoveDecorator: ClassDecorator = (constructor: Function)=> {
    constructor.prototype.hd = '张三'
    constructor.prototype.getPosition = (): {x: number, y: number} =>{
        return {x: 100,y: 200}
    }
  return class extends constructor {
    name = '史蒂夫'
     constructor(...args:any[]){
      super(...args)
    	console.log(args)
   }
  }
}

@MoveDecorator
class Tank{
    constructor(){
        console.log('tank 构造函数')
    }
}

const tank = new Tank()
console.log((<any>tank).getPosition()) //这里直接使用tank会提示错误,使用断言来指定类型
```

#### 报错时使用给类添加默认属性或断言处理

```typescript
//添加默认属性
class Tank(){
    public hd: string | undefined
    public getPosition(){}
    constructor(){
        concole.log('tank 构造函数')
    }
}

//使用断言处理
const tank = new Tank()
console.log((tank as any).getPosition())
console.log((<any>tank).getPosition:())
```

#### 语法糖

```typescript
const MoveDecorator: ClassDecorator = (constructor: Function) => {
    constructor.prototype.hd = '后盾人'
    constructor.prototype.getPosition = (): { x: number, y: number } => {
        return { x: 100, y: 100 }
    }
}

class Tank {
    constructor() {
        console.log('tank 构造函数');
    }
}

MoveDecorator(Tank); //这里不适用 @ 符号就是这样注册
const tank = new Tank()
console.log(tank.getPosition());
```

#### 装饰器工厂

又是需要有条件的返回不同的装饰器,这是可以使用装饰器工厂来解决问题

核心理念是使用函数有条件的的返回了不同的定制的装饰器

```typescript
const MusicDecorator = (type: string): ClassDecorator => {
    switch (type) {
        case 'player':
            return (constructor: Function) => {
                constructor.prototype.playMusic = (): void => {
                    console.log(`播放【海阔天空】音乐`);
                }
            }
            break;
        default:
            return (constructor: Function) => {
                constructor.prototype.playMusic = (): void => {
                    console.log(`播放【喜洋洋】音乐`);
                }
            }

    }
}

@MusicDecorator('tank')
class Tank {
    constructor() {
    }
}
const tank = new Tank();
(<any>tank).playMusic();

@MusicDecorator('player')
class Player {
}

const xj: Player = new Player();
(xj as any).playMusic()
```

### 方法装饰器

| **参数** | **说明**                                                   |
| -------- | ---------------------------------------------------------- |
| 参数一   | 普通方法是构造函数的原型对象 Prototype, 静态方法是构造函数 |
| 参数二   | 方法名称                                                   |
| 参数三   | 属性描述,是否可读些,是否可以枚举,携带的方法                |

```typescript
const ShowDecorator: MethodDecorator = (target: Object, propertyKey: string | symbol, descriptor: PropertyDescriptor):void => {
    console.log(target) //对象
    console.log(propertyKey) //方法名
    console.log(descriptor.value) //方法实现 .value获取具体的代码实现可以修改代码具体实现
}

class Hd {
    @ShowDecorator
    show(){
        console.log('show method')
    }
}
const instance = new Hd()
instance.show()
```

### 属性装饰器

首先介绍装饰器函数参数说明

| **参数** | **说明**                                                   |
| -------- | ---------------------------------------------------------- |
| 参数一   | 普通方法是构造函数的原型对象 Prototype, 静态方法是构造函数 |
| 参数二   | 属性名称                                                   |

#### 基本使用

```typescript
const PropDecorator: PropertyDecorator = (target: Object, propertyKey: string | symbol): void =>{
    console.log(propertyKey)
}

class Hd {
    @PropDecorator
    public name: string | undefined = '张三'
    show(){
        console.log(33)
    }
}
```

#### 访问器

下面是定义属性 name 的值转换为小写的装饰器

```typescript
const PropDecorator: PropertyDecorator = (target: Object, propertyKey: string | symbol): void => {
  let value: string
  const getter = () => {
    return value
  }

  const setter = (v: string) => {
    value = v.toLowerCase()
  }

  Object.defineProperty(target, propertyKey, {
    set: setter,
    get: getter
  })
}

class Hd {
  @PropDecorator
  public name: string | undefined = '张三'
  show() {
    console.log(33)
  }
}

const ceshi = new Hd()
ceshi.name = 'WODDSDJFDS'
console.log(ceshi.name) // woddsdjfds

```

### 参数装饰器

可以对方法的参数设置装饰器,参数装饰器的返回值被忽略

**参数装饰器比方法装饰器优先执行**	

| **参数** | **说明**                                                   |
| -------- | ---------------------------------------------------------- |
| 参数一   | 普通方法是构造函数的原型对象 Prototype, 静态方法是构造函数 |
| 参数二   | 方法名称                                                   |
| 参数三   | 参数所在索引位置                                           |

```typescript
// 利用参数装饰器判断所需要的参数是否为空,只能判断一个参数,由此案例得知多个参数装饰器是从右往左执行的
import 'reflect-metadata' //这个包要安装
const RequiredDecorator: ParameterDecorator = (target: Object, propertyKey: string | symbol, parameterIndex: number): void => {
  let requiredParams: number[] = []
  requiredParams.push(parameterIndex)

  Reflect.defineMetadata('required', requiredParams, target, propertyKey)
}
const validateDecorator: MethodDecorator = (
  target: Object,
  propertyKey: string | symbol,
  descriptor: PropertyDescriptor
): void => {
  const method = descriptor.value
  descriptor.value = function () {
    const requiredParams: number[] = Reflect.getMetadata('required', target, propertyKey) || []
    requiredParams.forEach(index => {
      if (index > arguments.length || arguments[index] === undefined) {
        throw new Error('请传入参数')
      }
      return method.apply(this, arguments)
    })
  }
}

class User {
  @validateDecorator
  find(name: string, @RequiredDecorator id: number) {
    console.log(id)
  }
}

new User().find('测试', 34)

```

