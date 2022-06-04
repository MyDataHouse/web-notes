## 一.yarn简介

Facebook 贡献的 Javascript 包管理器。

yarn的特点:

速度快:Yarn 缓存了每个下载过的包，所以再次使用时无需重复下载。 同时利用并行下载以最大化资源利用率，因此安装速度更快。

安全性高:在执行代码之前，Yarn 会通过算法校验每个安装包的完整性。

可靠性高:使用详细、简洁的锁文件格式和明确的安装算法，Yarn 能够保证在不同系统上无差异的工作。



## 二.安装

1.可以直接从官网进行安装 

```
https://yarnpkg.com/en/docs/install
```

2.可以直接使用 npm 安装

```
npm install -g yarn@1.22.5
```

### 三.初始化init

```
yarn init -y   //默认使用文件信息,不要使用中文 生成package.json
```

### 四.添加依赖

```
yarn add [package]
yarn add [package]@[version]
yarn add [package]@[tag]

yarn global add [package] //全局安装

将依赖项添加到不同依赖项类别中
分别添加到 devDependencies、peerDependencies 和 optionalDependencies 类别中：

yarn add [package] --dev  #简写 -D
yarn add [package] --peer
yarn add [package] --optional
```

### 五.更新依赖包

```
yarn upgrade [package]
yarn upgrade [package]@[version]
yarn upgrade [package]@[tag]
```

### 六.移除依赖包

```
yarn remove [package] 
```

### 七.查看包的信息

```
yarn info [package]
该命令将获取有关软件包的信息，并以树格式返回。该程序包不一定必须在本地安装。
```

### 八.发布包的npm资源库

```
yarn publish
```

### 九.启动create-react-app脚手架的服务器

```
yarn start
```

### 十.打包生成 create-react-app的生产文件

```
yarn build
```

### 十一.缓存列表

```
yarn cache list
```

### 十二.缓存目录

```
yarn cache dir
```

### 十三.yarn配置信息

```
yarn config list

yarn config set[key][value] //修改信息
```