# pnpm monorepo架构

## monorepo架构目录

```bash
my-monorepo
├─ packages
│  ├─ app
│  │  └─ package.json
│  ├─ docs
│  │  └─ package.json
│  └─ web
│     └─ package.json
└─ package.json

```

根目录下的package.json 添加字段，标明子工作区路径

```json
 "workspaces": [
    "packages/app",
    "packages/docs",
    "packages/web"
  ]
```

.gitignore添加忽略所有`node_modules`文件夹

```bash
/**/node_modules/
```

如果不想在每个子包内下载相同的依赖的话可以下载到，项目根目录

```shell
pnpm add 包名  -w
```

## 子项目之间的引用问题

.npmrc 文件添加配置让pnpm 自动链接本地工作区，而不是从node_modules里选取，这会导致修改源码没有效果

```yaml
link-workspace-packages=true
```

如果没有用需要手动执行命令进行本地工作区链接

```shell
pnpm -F main add common
# or
pnpm --filter main add common
#意思是向 main 项目中 添加 common 作为依赖项，main,common 这个名称是从各自的 package.json的name字段来的
```

