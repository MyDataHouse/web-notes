### vue-print-nb 处理打印

1. 安装

   ```shell
   npm i vue-print-nb
   ```

2. 全局注册

   ```javascript
   import Print from 'vue-print-nb' //导入
       Vue.use(Print) //使用
   ```

3. 使用

   ```javascript
   v-print="printObj" //绑定要打印的对象传入一个对象对象的id属性,要打印的对象要起一个id
   data(){
       return {
          printObj:{
              id:'绑定的id'
          }
       }
   }
   ```

   

### vue-i18n 多语言处理

1. 安装

   ```javascript
   npm i vue-i18n
   ```

2. 全局注册使用

   ```javascript
   // 在单独的文件中定义 lang/index.js
   import i18n form 'vue-i18n'
   import Vue form 'vue'
   import Cookie form 'js-cookie'
   import elementEn form 'element-ui/lib/locale/lang/en' //导入英文语言包
   import elementZH form 'element-ui/lib/locale/lang/zh-CN' //导入中文语言包
   import customZH from './zh' // 引入自定义中文包
   import customEN from './en' // 引入自定义英文包
   
   Vue.use(i18n)
   export default new i18n({
       locale: Cookie.get('language') || 'zh',
       message:{
           en:{
               ...elementEn
                ...customEN // 将自定义的英文语言包引入
           },
           zh:{
               ...elementZH
                ...customZH // 将自定义的中文语言包引入
           }
       }
   })
   
   //在main.js中注册
   import i18n from '@/lang'
   
   Vue.use(ElementUI, {
     // t方法是去找 对应的语言下的值 去显示
     i18n: (key, value) => i18n.t(key, value)
   })
   
   new Vue({
     el: '#app',
     router,
     store,
     i18n, //添加
     render: h => h(App)
   })
   ```

3. 动态修改语言

   ```javascript
   Cookie.set('language', type) // 切换多语言存入cookie
   this.$i18n.locale = langagekey // 设置给本地的i18n插件
   this.$message.success('切换多语言成功')
   ```

4. 使用语言包

   ```javascript
   this.$t(key) // 全局注册后有了$t方法,参数是对应语言包的键名,返回对应的文字
   ```

   

