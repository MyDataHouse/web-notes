### 关于upload组件的使用(待补充)

#### 一. list-type:picture-card(上传图片类型的注意事项)

基本流程

1. 处理添加图片样式,有图片是否显示

   ```javascript
   //添加以下样式,动态控制上传框是否显示
   <style>
   .disabled .el-upload--picture-card {
     display: none;
   }
   </style>
   
   //给<el-upload>添加如下class
   :class="{disabled : FileComputed}" //FileComputed 是计算属性控制为真或假
   
   //定义计算属性
   computed:{
       FileComputed(){
           return this.list.length >= 1 //如过已经有一张图片就不显示上传
       }
   }
   ```

   

2. 添加图片预览

   ```javascript
   //在upload组件后面添加dialog组件
   <el-dialog>
   	<el-image :src='imgUrl' />    
   </el-dialog>
   //在upload组件中使用on-preview属性绑定函数控制dialog是否显示
   ```

3. 删除图片

   ```javascript
   //在upload组件中使用on-remove属相绑定函数
   remove(file, fileList){ //file是删除的文件, fiileList是删除后的文件列表
       this.list = fileList
   }
   ```

4. 监听文件状态改变

   ```javascript
   //on-change 属相在文件状态改变时的钩子，添加文件、上传成功和上传失败时都会被调用
   change(file,fileList){
       if(file.status === 'ready'){ //判断现在文件读取的状态执行相应操作
            this.list = fileList.map(item => item)
           this.$emit('update:disabled', true) //禁用保存按钮
       }
        if (file.status === 'fail') {
           this.list = [{ url: this.imgUrl }] //上传失败后图片恢复到没修改前
         }
       this.$emit('update:disabled', false) //不管上传成功失败都解除按钮的禁用状态
   }
   ```

5. 上传之前的文件检查

   ```javascript
   //before-upload属性绑定函数,上传文件之前的钩子，参数为上传的文件，若返回 false 或者返回 Promise 且被 reject，则停止上传。
   beforeUpload(file) {
         const type = ['image/jpeg', 'image/png', 'image/gif'] //判断文件格式
         if (!type.includes(file.type)) {
           this.$message.error('上传图片只能是 JPG、GIF、PNG 格式!')
           return false
         }
         const maxSize = 5 * 1024 * 1024
         if (file.size > maxSize) { //判断文件大小
           this.$message.error('图片大小最大不能超过5M')
           return false
         }
         this.currentFileUid = file.uid  //保存uid用来找到list对应的图片进行线上地址替换
         return true
       },
   ```

6. 上传文件

   ```javascript
   //http-request属相绑定函数实现自定义上传
   async uploadImg(params){
       try{
            this.showProgress = true //显示进度条
             const form = new FormData() //创建表单对象
             form.append('photo', params.file) //添加文件
           //解构返回的图片线上地址,this.upload是处理上传进度的回调函数
           const {data:{data}} = await updateUserPhotoAPI(form,this.upload)
           //修改上传图片对象的路径为线上地址
           this.list = this.list.map(item => {
               if (item.uid === this.currentFileUid) {
                 return { url: data.photo, upload: true }
               }
               return item
             })
           //将线上地址传给表单绑定数据
           this.$emit('update:imgUrl', data.photo)
            params.onSuccess() // 告诉upload组件上传成功
       }catch(error){
           console.log(error)
           params.onError() // 告诉upload组件上传失败
   
       }
   }
   ```

7. 进度条处理

   ```javascript
   //在api中添加 onUploadProgress 处理进度
   export const updateUserPhotoApi = (data, token, upLoad) =>//upLoad是传进来的回调函数用来接收进度参数进一步处理
     request({
       method: 'PATCH',
       url: '/v1_0/user/photo',
       data,
       headers: { Authorization: 'Bearer ' + token }, //忽视这行代码
       onUploadProgress: function(progress) {
         upLoad(progress)
       }
     })
   
   //在upload组件文件中
       // 进度条处理
       upLoad(progress) {
         this.progressWidth = Math.floor((progress.loaded / progress.total) * 100)
       }
   ```

   