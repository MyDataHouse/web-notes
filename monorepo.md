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

```yaml
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
//package.json
"workspaces": [
    "packages/app",
    "packages/docs",
    "packages/web"
  ]
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

在子包的package.json里面直接写

```json
//"workspace:@1.0.0" 也可以指定吧版本
"dependencies":{
"要引入的包名": "workspace:*" 
}
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

`prettier`脚本命令

```json
"scripts":{
    //......其他省略
    "lint:prettier": "prettier --write \"**/*.{js,ts,mjs,cjs,json,tsx,css,less,scss,vue,html,md}\"",
}
```



## Eslint

| 类别                 | 库名                                               |
| -------------------- | -------------------------------------------------- |
| **核心引擎**         | `eslint`                                           |
| **官方规则集**       | `@eslint/js`                                       |
| **全局变量支持**     | `globals`                                          |
| **TypeScript 支持**  | `typescript-eslint`                                |
| **类型定义（辅助）** | `@types/node`                                      |
| **Prettier 集成**    | `eslint-plugin-prettier`, `eslint-config-prettier` |
| **Vue.js 支持**      | `eslint-plugin-vue`                                |

```js
import { defineConfig } from 'eslint/config';
import tseslint from '@electron-toolkit/eslint-config-ts';
import eslintConfigPrettier from '@electron-toolkit/eslint-config-prettier';
import eslintPluginVue from 'eslint-plugin-vue';
import pluginJs from '@eslint/js';
import vueParser from 'vue-eslint-parser';
import eslintrcAutoImport from './eslintrc-auto-import.json' with { type: 'json' };

export default defineConfig(
  { ignores: ['**/node_modules', '**/dist', '**/out', '**/.git/**', '**/public/**', '**/local_modules/**'] },
  pluginJs.configs.recommended,
  tseslint.configs.recommended,
  eslintPluginVue.configs['flat/recommended'],
  {
    files: ['**/*.vue'],
    languageOptions: {
      parser: vueParser,
      parserOptions: {
        ecmaFeatures: {
          jsx: true
        },
        extraFileExtensions: ['.vue'],
        parser: tseslint.parser
      },
      globals: { ...eslintrcAutoImport.globals }
    }
  },
  {
    files: ['**/*.{ts,mts,tsx,vue}'],
    rules: {
      'vue/require-default-prop': 'off',
      'vue/multi-word-component-names': 'off',
      '@typescript-eslint/no-explicit-any': 'off', // 关闭 any 类型的检查
      '@typescript-eslint/explicit-function-return-type': 'off', // 关闭函数返回类型的检查
      'vue/one-component-per-file': 'off',
      'vue/block-lang': [
        'error',
        {
          script: {
            lang: ['ts', 'tsx']
          }
        }
      ]
    }
  },
  eslintConfigPrettier
);

```

脚本命令

```json
"scripts":{
    //......其他省略
    "lint:eslint": "eslint",
}
```



## Commitizen 约束Git代码提交规范

安装

```shell
pnpm -Dw add @commitlint/cli @commitlint/config-conventional commitizen cz-git
```

- `@commitlint/cli 是 commitlint` 工具的核心。
- `@commitlint/config-conventional` 是基于 conventional commits 规范的配置文件。
- `commitizen` 提供了一个交互式撰写commit信息的插件
- [cz-git](https://cz-git.qbb.sh/zh/guide/)是国人开发了这一款工具，工程性更强，自定义更高，交互性更好。

配置命令

```json
// package.json
"scripts": {
  // 其他省略
	"commit": "git-cz"
},
"config": {
  "commitizen": {
    "path": "node_modules/cz-git"
  }
}
```

配置`cz-git`

```shell
touch commitlint.config.js
```

```js
/** @type {import('cz-git').UserConfig} */
export default {
  extends: ['@commitlint/config-conventional'],
  rules: {
    // @see: https://commitlint.js.org/#/reference-rules
    'body-leading-blank': [2, 'always'],
    'footer-leading-blank': [1, 'always'],
    'header-max-length': [2, 'always', 108],
    'subject-empty': [2, 'never'],
    'type-empty': [2, 'never'],
    'subject-case': [0],
    'type-enum': [
      2,
      'always',
      ['feat', 'fix', 'docs', 'style', 'refactor', 'perf', 'test', 'build', 'ci', 'chore', 'revert', 'wip', 'workflow', 'types', 'release']
    ]
  },
  prompt: {
    types: [
      { value: 'feat', name: '✨ 新功能: 新增功能' },
      { value: 'fix', name: '🐛 修复: 修复缺陷' },
      { value: 'docs', name: '📚 文档: 更新文档' },
      { value: 'refactor', name: '📦 重构: 代码重构（不新增功能也不修复 bug）' },
      { value: 'perf', name: '🚀 性能: 提升性能' },
      { value: 'test', name: '🧪 测试: 添加测试' },
      { value: 'chore', name: '🔧 工具: 更改构建流程或辅助工具' },
      { value: 'revert', name: '⏪ 回滚: 代码回滚' },
      { value: 'style', name: '🎨 样式: 格式调整（不影响代码运行）' },
      { value: 'wip', name: '🚧 工作中: 开发中' },
      { value: 'workflow', name: '⚙️ 工作流: 工作流改进' },
      { value: 'types', name: '🩺 类型定义: 类型定义更新' },
      { value: 'release', name: '🚀 发布: 发布新版本' }
    ],
    scopes: ['root', 'docs', '二维代码', '三维代码', 'utils'], // 影响范围自定义的字符串（可选）
    allowCustomScopes: true,
    skipQuestions: ['body', 'footerPrefix', 'footer', 'breaking'], // 跳过“详细描述”和“底部信息”
    messages: {
      type: '📌 请选择提交类型:',
      scope: '🎯 请选择影响范围 (可选):',
      subject: '📝 请简要描述更改:',
      body: '🔍 详细描述 (可选):',
      footer: '🔗 关联的 ISSUE 或 BREAKING CHANGE (可选):',
      confirmCommit: '✅ 确认提交?'
    }
  }
};

```

### husky 配置git 的hook 时机执行哪些事情

安装`husky`

```shell
pnpm -Dw add husky
```

初始化

```cmd
pnpx husky init
#pnpm 10以上使用
pnpm dlx husky init
```

配置

```cmd
#!/usr/bin/env sh
pnpm lint:prettier && pnpm lint:eslint
```

### lint-staged 检查暂存区内容

安装

```shell
pnpm -Dw add lint-staged
```

配置命令

```json
//名字叫precommit 是 因为 git-cz会在提交之前自动运行这个命令进行检查
"precommit": "lint-staged"
```

配置文件

```js
// .lintstagedrc.js
//意思是这些后缀的文件，指定什么命令检查
export default {
  "*.{js,ts,mjs,cjs,json,tsx,css,less,scss,vue,html,md}": ["cspell lint"],
  "*.{js,ts,vue,md}": ["prettier --write", "eslint"]
};

```

重新配置husky