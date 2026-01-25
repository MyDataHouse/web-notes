# pnpm monorepo管理

## 创建 pnpm-worksapce.yaml 文件

```shell
touch pnpm-workspace.yaml
```

写入子包目录

```yaml
packages:
	- 'packages/*'
	- 'apps/*'
```

执行工程级别命令

```shell
pnpm --workspace--root [...] #在项目根目录执行pnpm 命令
#简化版本
pnpm -W [...]
```

在子包中执行命令

```shell
#进入子包目录中执行
#或
pnpm -C 子包路径 [...]
```

## 环境版本锁定

```json
//package.jon
"engines":{
  "node":">=22.14.0",
  "npm":">=10.9.2",
  "pnpm":">=10.15.1"
}
```

```shell
#.npmrc  严格校验环境版本，不对报错
engine-strict=true
```

## Typescript

根目录定义好通用的ts配置， 每个子包如果有特殊配置的话，每个子包，继承根目录配置，在写自己的配置

```shell
pnpm -Dw add typescript @types/node
```

```shell
touch tsconfig.json
```

```json
//tsconfig.json

```

## Prettier

没有特殊配制，和平常一致

```js
//.prettierrc.cjs
module.exports = {
  printWidth: 150, //单行长度
  tabWidth: 2, //缩进长度
  useTabs: false, //使用空格代替tab缩进
  semi: true, //句末使用分号
  singleQuote: true, //使用单引号
  quoteProps: 'as-needed', //仅在必需时为对象的key添加引号
  jsxSingleQuote: true, // jsx中使用单引号
  trailingComma: 'none', //多行时尽可能打印尾随逗号
  bracketSpacing: true, //在对象前后添加空格-eg: { foo: bar }
  jsxBracketSameLine: true, //多属性html标签的‘>’折行放置
  arrowParens: 'always', //单参数箭头函数参数周围使用圆括号-eg: (x) => x
  requirePragma: false, //无需顶部注释即可格式化
  insertPragma: false, //在已被preitter格式化的文件顶部加上标注
  proseWrap: 'preserve', //不知道怎么翻译
  htmlWhitespaceSensitivity: 'ignore', //对HTML全局空白不敏感
  vueIndentScriptAndStyle: false, //不对vue中的script及style标签缩进
  endOfLine: 'auto', //结束行形式
  embeddedLanguageFormatting: 'auto' //对引用代码进行格式化
};

```

配置忽略文件

```
//.prettierignore
out
dist
pnpm-lock.yaml
LICENSE.md
node_modules
.rollup.cache
.vscode
build
resources
src/renderer/src/public
```

## Eslint

| 类别             | 库名                                          |
| ---------------- | --------------------------------------------- |
| 核心引擎         | eslint                                        |
| 官方规则集       | @eslint/js                                    |
| 全局变量支持     | globals                                       |
| Typescript       | typescript-eslint                             |
| 类型定义（辅助） | @types/node                                   |
| Prettier集成     | eslint-plugin-prettier,eslint-config-prettier |
| Vue.js支持       | eslint-plugin-vue                             |

## Commitizen 约束Git代码提交规范

安装

```shell
pnpm -Dw add @commitlint/cli @commitlint/config-conventional commitizen cz-git
```

- @commitlint/cli 是 commitlint 工具的核心
- @commitlint/config-conventional 是基于 conventional commits 规范的配置文件
- commitizen 提供了一个交互式撰写commit信息的插件
- Cz-git 是国人开发的一款工具，工程性更强，自定义更高，交互性更好

配置命令

```json
// package.json
"scripts":{
  "commit":"git-cz"
}
"config":{
  "commitizen":{
    "path":"node_modules/cz-git"
	}
}
```

配置cz-git