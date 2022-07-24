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

   

