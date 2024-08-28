## 依赖安装

```shell
pnpm i @types/node @vitejs/plugin-vue-jsx @vue/eslint-config-prettier @vue/eslint-config-typescript autoprefixer eslint eslint-plugin-vue naive-ui postcss prettier sass tailwindcss unplugin-auto-import unplugin-vue-components vite-plugin-vue-setup-extend -D
```

```typescript
pnpm i pinia vue-router
```

## 项目配置

.editorconfig

```typescript
root = true

[*]
charset = utf-8
indent_style = space
indent_size = 2
end_of_line = lf
insert_final_newline = true
trim_trailing_whitespace = true
```

.eslintignore

```typescript
node_modules
dist
out
.gitignore

```

.eslintrc.cjs

```typescript
// eslint-disable-next-line no-undef
module.exports = {
  parser: '@typescript-eslint/parser',
  plugins: ['@typescript-eslint', 'prettier', 'import'],
  settings: {},
  extends: [
    'eslint:recommended',
    'plugin:@typescript-eslint/recommended',
    'plugin:prettier/recommended',
    '@vue/eslint-config-typescript/recommended',
    '@vue/eslint-config-prettier'
  ],
  rules: {
    'vue/require-default-prop': 'off',
    'vue/multi-word-component-names': 'off',
    '@typescript-eslint/no-explicit-any': 'off',
    'prettier/prettier': 'error',
    'import/no-unresolved': 'error',
    // 指定数组的元素之间要以空格隔开(,后面)， never参数：[ 之前和 ] 之后不能带空格，always参数：[ 之前和 ] 之后必须带空格
    'array-bracket-spacing': [2, 'never'],
    // always-multiline：多行模式必须带逗号，单行模式不能带逗号
    'comma-dangle': [2, 'never'],
    // 控制逗号前后的空格
    'comma-spacing': [2, { before: false, after: true }],
    // 控制逗号在行尾出现还是在行首出现
    'comma-style': [2, 'last'],
    // 使用 === 替代 ==
    eqeqeq: [2, 'allow-null'],
    strict: 2, //使用严格模式
    yoda: [2, 'never'], //禁止尤达条件
    'no-alert': 0, //禁止使用alert confirm prompt
    'no-duplicate-case': 2, //switch中的case标签不能重复
    'no-empty-character-class': 2, //正则表达式中的[]内容不能为空
    'no-eq-null': 2, //禁止对null使用==或!=运算符
    'no-multiple-empty-lines': [1, { max: 2 }], //空行最多不能超过2行
    quotes: [1, 'single'] //引号类型 `` "" ''
  }
};

```

.prettierignore

```typescript
out
dist
pnpm-lock.yaml
LICENSE.md

```

.prettierrc.cjs

```typescript
module.exports = {
  printWidth: 120, //单行长度
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

vite.config.ts

```typescript
import { fileURLToPath, URL } from 'node:url';
import { defineConfig, loadEnv } from 'vite';
import vue from '@vitejs/plugin-vue';
import vueJsx from '@vitejs/plugin-vue-jsx';
import legacy from '@vitejs/plugin-legacy';
import { NaiveUiResolver } from 'unplugin-vue-components/resolvers';
import AutoImport from 'unplugin-auto-import/vite'; //自动导入api
import Components from 'unplugin-vue-components/vite'; //自动导入elementui
import VueSetupExtend from 'vite-plugin-vue-setup-extend'; //组建名称
// import { createSvgIconsPlugin } from 'vite-plugin-svg-icons'; //svg批量加载
export default defineConfig(({ mode }) => {
  // 根据当前工作目录中的 `mode` 加载 .env 文件
  // 设置第三个参数为 '' 来加载所有环境变量，而不管是否有 `VITE_` 前缀。
  const env = loadEnv(mode, process.cwd(), '');
  return {
    plugins: [
      vue(),
      vueJsx(),
      legacy({
        targets: ['defaults', 'not IE 11']
      }),
      AutoImport({
        imports: [
          'vue',
          'vue-router',
          'pinia',
          {
            'naive-ui': ['useDialog', 'useMessage', 'useNotification', 'useLoadingBar']
          }
        ],
        dts: './src/auto-imports.d.ts'
      }),
      Components({
        resolvers: [NaiveUiResolver()],
        dts: './src/components.d.ts'
      }),
      VueSetupExtend()
    ],
    resolve: {
      alias: {
        '@': fileURLToPath(new URL('./src', import.meta.url))
      }
    },
    server: {
      host: 'localhost',
      port: parseInt(env.port),
      open: true,
      proxy: {
        '/api': {
          target: env.server,
          changeOrigin: true,
          ws: true,
          rewrite: (path) => path.replace(/^\/api/, '')
        }
      }
    }
  };
});

```

## main.ts

```typescript
import '@icon-park/vue-next/styles/index.css';
import 'virtual:svg-icons-register'; //这是虚拟模块的引入方式，用于给脚手架插件在打包和开发时做相应的处理
import Globalcomponents from '@renderer/components';
import router from '@renderer/router';
import { pinia } from '@renderer/store';
import { createApp } from 'vue';
import App from './App.vue';

import '@renderer/assets/style/variable.css';
import '@renderer/assets/style/tailwind.css';

createApp(App)
  .use(pinia)
  .use(Globalcomponents as any)
  .use(router)
  .mount('#app');

```



## vue-router

```typescript
import { createRouter, createWebHistory } from 'vue-router';
import processData from './modules/processData';
const mouelesRouters = [processData];

const routes = [
  // { path: '/login', component: () => import('@/views/login/index.vue') },
  {
    path: '/:catchAll(.*)', // 不识别的path自动匹配404
    redirect: '/'
  }
];

const router = createRouter({
  history: createWebHistory(import.meta.env.BASE_URL),
  routes: [...mouelesRouters, ...routes]
});

export default router;

```

## pinina

```typescript
import { createRouter, createWebHistory } from 'vue-router';
import processData from './modules/processData';
const mouelesRouters = [processData];

const routes = [
  // { path: '/login', component: () => import('@/views/login/index.vue') },
  {
    path: '/:catchAll(.*)', // 不识别的path自动匹配404
    redirect: '/'
  }
];

const router = createRouter({
  history: createWebHistory(import.meta.env.BASE_URL),
  routes: [...mouelesRouters, ...routes]
});

export default router;

//===============================================
export const useElectronStore = defineStore('Electron', () => {
  /** 主窗口id */
  const mainId = ref(0);
  /** 设置主窗口id */
  function setMainId(id: number) {
    mainId.value = id;
  }

  return { setMainId, mainId };
});
```

## 全局组件注册

```typescript
import type { Component, App } from 'vue';
import SvgIcon from './SvgIcon/index.vue';
import HeaderCtrl from './HeaderCtrl/index.vue';
const components: { [key: string]: Component } = { SvgIcon, HeaderCtrl };
export default {
  install(app: App) {
    for (const key in components) {
      app.component(key, components[key]);
    }
  }
};

```

