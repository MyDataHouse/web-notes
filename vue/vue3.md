# vue3和vue2中的不同点

### 1.`v-if`  `v-for`优先级顺序不同

- 在vue2中`v-for` 比`v-if`优先级高
- 在vue3中`v-if`比`v-for`优先级高

### 2.`v-for`中的`ref`数组

- 在vue2中，在`v-for`中使用`ref`来自动填充ref属性添加到`$refs数组`可以获取到

- 在vue3中，在`v-for`中使用`ref`来自动填充ref属性不会自动添加到`$refs数组`获取不到,可以自己写函数保存对应的元素对象上

	```javascript
	<template>
	    <div v-for='(item,index) in 5' :key=index :ref='setItemRef'>
	    </div>
	</template>
	<script>
	        data(){
	    return{
	        itemRefs:[]
	    }
	},
	methods:{
	    setItemRef(el){
	        if(el){
	            this.itemRefs.push(el)
	        }
	    }
	}
	</script>
	```

	

### 3.`$children`被废除

- vue2中访问当前实力的直接子组件
- vue3中已被**移除**，使用`$refs`访问子组件

### 4.`setup`语法糖-组合式API

组合式API来解决，使用（data,computed,methods,watch）等组件选项来组织逻辑通常很有效。然而，当我们的组件变得很大时，鼠标来回翻滚查看对应的逻辑代码，会很麻烦，这回让组件的代码阅读变得难以理解，所以使用组合式API可以解决这个问题

#### 4.1 在vue2和vue3中对data中的数据添加新属性

- vue2中对data中的对象或数组增加新属性后，新属性会失去响应式，要使用$set添加上

	```javascript
	data{
	    return{
	        a:{
	            a:1,
	            b:2
	        }
	    }
	}
	this.$set(this.a, 'c', 3)
	```

- 在vue3中只需要使用reactive包裹起来(需要的时候再加)

	```javascript
	import {ref, reactive} from 'vue' 
	//导入的是渲染函数
	setup(){
	    let obj = reactive(
	    	{
	            a:1,
	            b:2
	        }
	    )
	    let btn = ()=>{ obj.c=3 }
	    return {obj, btn}
	}
	```

- 渲染函数
	- ref：就是定义数据，什么类型的数据都可以定义， 一般时简单数据，数组时用它，拿数组数据时需要.value
	- reactive：也是定义数据，一般定义复杂类型，一般定义对象时用它
- 响应式区别
	- vue2: object.defineProperty()
	- vue3: Proxy()
		1. object.defineProperty()存在的问题
			1. 不能监听数组的变化
			2. 必须遍历对象的每一个属性，递归
		2. Proxy()
			1. 不需要遍历，只需要递归
			2. vue3已经做好了

#### 4.2 生命周期函数

1. 在vue3中setup组合式API中没有，`beforeCreate` `created`生命周期
2. 在vue3中使用生命周期函数要加`on`例如`onMounted`
3. 在同时使用选项式API和组合式API时，**组合式API优先级高**
4. 在vue3中使用新API要从vue中引入，每个文件都要引
5. 为了防止麻烦，有npm包来自动引入 unplugin-auto-import -D,需要额外配置

#### 4.3 响应式工具

1. `toRefs`使用它，组件可以**解构**/展开返回的对象而不会失去响应性：

	```javascript
	import {toRefs} from 'vue'
	setup(){
	    //当使用{name,year}来结构数据，时数据会失去响应
	    let obj = {
	        name:'ax',
	        year:12
	    }
	    return {...toRefs(obj)} //更解构效果一样，当时具备响应式
	}
	```

	

#### 4.5 计算属性`computed`

1. 跟vue2使用没什么区别，需要单独引入computed

	```javascript
	import {computed, ref} from 'vue'
	setup(){
	    let year = ref(13)
	    let yearChange = computed(
	    	 ()=>{
	        return year++
	   	 })
	    return {
	        yeaer, yearChange
	    }
	}
	```
	
	```javascript
	import {computed, ref} from 'vue'
	setup(){
	    let year = ref(13)
	    let yearChange = computed(
	    	get(){
	        return year++
	        }
			set(val){
	            console.log(val)
	        }
	    )
	    return { year, yearChange }
	}
	```
	
	

#### 4.6  监听属性watch

1. vue3中监听属性

	```javascript
	import { watch, ref } from 'vue'
	setup(){
	    let msg = ref('数据')
	    watch(msg,(newvlaue,oldvalue)=>{
	        console.log(newvalue, oldvalue)
	    },{
	        immediate:true,
	        deep:true
	    })
	    return {
	        msg
	    }
	}
	```
	
2. 监听多个值

	```javascript
	import { watch, ref } from 'vue'
	setup(){
	    let msg = ref('数据')
	    let str = ref('测试')
	    // 第一个参数是监听多个数据的数组
	    watch([msg, str],(newvlaue,oldvalue)=>{
	        console.log(newvalue, oldvalue)
	    },{
	        immediate:true,
	        deep:true
	    })
	    return {
	        msg,
	        str
	    }
	}
	```

	

3. 监听‘对象’中某个对象

	```javascript
	import { watch, reactive } from 'vue'
	setup(){
	    let msg = reactive({
	        name:'西汉三',
	        arr:['1','2']
	    })
	    watch( ()=>msg,(newvlaue,oldvalue)=>{
	        console.log(newvalue, oldvalue)
	    })
	    return {
	        msg
	    }
	}
	```

	

4. 立即执行监听函数

	```vue
	watchEffect(
	()=>{
	    console.log(msg.value)
	}
	)
	//监听所有数据只要有数据改变就执行，并对数据进行响应
	```

	

#### 4.7 路由

this.$router 改变 useRouter

this.$route 改变 useRoute

#### 4.8 组件传值-父传子

- 父组件

	```javascript
	<template>
	    <div>
	     <List :msg='msg'></List>
	    </div>
	</template>
	<script setup>
	    import {ref} from 'vue'
	    import List from './components/List.vue'
		let msg = ref('数据')
	<script>
	```

- 子组件

	```javascript
	<template>
	    <div>
	     {{msg}}
	    </div>
	</template>
	<script setup>
	    import {defineProps} from 'vue'
	    defineProps({
	        msg:{
	            type:String,
	            default:'111'
	        }
	    })
	<script>
	 //=============================================================
	       方式二
	<template>
	    <div>
	     {{msg}}
	    </div>
	</template>
	<script type='text/javascript'>
	    export default {
		props:['msg'],
	        setup(props){
	       let msg = props.msg
	       return (msg)
	    }
	}
	<script>
	//=================================================================
	    方式三
	<template>
	    <div>
	     {{msg}}
	    </div>
	</template>
	<script type='text/javascript'>
	    import { toRefs } from 'vue'
	    export default {
		props:['msg'],
	        setup(props){
	       let {msg} = toRefs(props)
	       return (msg)
	    }
	}
	<script>
	```

	

#### 4.9 组件传值-子传父

- 父组件

	```javascript
	<template>
	    <div>
	     <List :msg='msg'></List>
	    </div>
	</template>
	<script setup>
	    import {ref} from 'vue'
	    import List from './components/List.vue'
		let msg = ref('数据')
	    const fn = (n)==>{ msg = n }
	<script>
	```

	

- 子组件

	```javascript
	<template>
	    <div @click='change('we')'>
	    </div>
	</template>
	<script setup>
	    import {defineEmits} from 'vue'
		const emit = defintEmits(['fn'])
	    const change = (n)=> emit('fn',n)
	<script>
	//=============================================================
	        方式二
	<template>
	    <div @click='change('we')'>
	    </div>
	</template>
	<script type='text/javascript'>
	    export default {
			emits: ['fn'],
	    	setup(props,{emit}){
	            const change = (n)=>{emit('fn',n)}
	        }
	}
	<script>
	```


#### 4.10 Teleport 传送

- 将指定内容传送到指定位置
- <teleport to = '#container'></teltport>
- <teleport to = '.main'></teltport>
- <teleport to = 'body'></teltport>

#### 4.11 动态组件

```javascript
<template>
    <component :is='com.com'></component>
</template>
<script setup>
  import { ref } from 'vue'
	import A from './components/A.vue'
	let com = ref({
        // 要使用markRaw包裹起来对组件本身不进行响应
        com: markRaw(A)
    })
<script>
```

#### 4.12 异步组件

vueusea 包

- 使用场景一

  - 组件按需引入：当用户访问到了组件再去加载该组件

  	```javascript
  	<template>
  	    <div ref='target'>
  	        <C v-if='targetIsVisible'></C>
  	    </div>
  	</template>
  	<script setup>
  	    import { useIntersectionObserver } from '@vueuse/core'
  		import {defineAsyncComponent} from 'vue'
  		const C = defineAsyncComponent(()=>{
  	        import('../components/C.vue')
  	    })
  	    const target = ref()
  		const targetIsVisible= ref(false)
  	    const { stop } = useIntersectionObserver(
  	    target,
  	        ([{isIntersecting}])=>{
  	            if(isIntetsecting){
  	                targetIsVisible.value = isIntersecting
  	            }
  	        }   
  	    )
  	</script>
  	    
  	    //==============================================================
  	    //下面的是使用suspense，获取加载状态的使用
  	    <template>
  	    <div ref='target'>
  	        <Suspense v-if='targetIsVisible'>
  	        	<template #default>
  	                <C></C>
  	            </template>
  				<template #fallbackk>
  	                加载中...
  	            </template>
  	        </Suspense>
  	    </div>
  	</template>
  	<script setup>
  	    import { useIntersectionObserver } from '@vueuse/core'
  		import {defineAsyncComponent} from 'vue'
  		const C = defineAsyncComponent(()=>{
  	        import('../components/C.vue')
  	    })
  	    const target = ref()
  		const targetIsVisible= ref(false)
  	    const { stop } = useIntersectionObserver(
  	    target,
  	        ([{isIntersecting}])=>{
  	            if(isIntetsecting){
  	                targetIsVisible.value = isIntersecting
  	            }
  	        }   
  	    )
  	</script>
  	```
  	
  	

- 使用场景二， 异步组件中，还有异步操作

	npm run build 打包完成后，异步组件有单独的js文件，是从主体js分包出来的

	```javascript
	<template>
	    <Suspense>
	    	<template #default>
	            <A></A>
	        </template>
			<template #fallback>
	            加载中...
	        </template>
	    </Suspense>
	</template>
	<script setup>
	    import {defineAsyncComponent} from 'vue'
		cosnt A = defineAsyncComponent(()=>{ 
	    import('../components/A.vue')
	    })
	</script>
	```

	

#### 4.13 mixin

```javascript
//定义混入代码
import {ref} from 'vue'
export const ceshi = function(){
    let a = ref(3)
    let b = ref(false)
    let bBtn=()=>{
        a.value += 1
        b.value = true
        setTimeout(()=>{
            b.value = false
        },2000)
    }
    return {
        a,b,bBtn
    }
}

//使用混入代码
<script setup>
    import { ceshi } from '../mxions/mixin.js'
	let { a, b, bBtn } = ceshi()
</script>
```

#### 4.14 provide inject

```javascript
//在父组件中
<script setup>
import { ref } from 'vue'   
let num = ref(100)
provide('dataName', num)
</script>

//在子组件中
<script setup>
import { inject } from 'vue'
const num = inject('dataName')
</script>
```

