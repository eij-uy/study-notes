[toc]

# 搭建vite + vue3 + ts + pinia 最新版本项目

首先在tsconfig.json的include数组中放入所有需要ts编译的文件

## 配置环境变量并在vite.config.ts中使用

1. vite.env.d.ts中加入

~~~typescript
export interface ViteEnv {
    readonly VITE_... : string
    ...
}
    
interface ImportMetaEnv extends ViteEnv {
    __:unknown
}

interface ImportMeta {
  readonly env: ImportMetaEnv
}
~~~

2. 在vite.config.ts中,

   需要使用pnpm add @types/node -D 下载开发依赖， 不然报错

~~~typescript
import { defineConfig, loadEnv } from 'vite'
import vue from '@vitejs/plugin-vue'
import { wrapperEnv } from './build/createEnv'
import { resolve } from 'path'

// https://vitejs.dev/config/
export default defineConfig(({ command, mode }) => {
  const root = process.cwd()
  // root就是项目在磁盘上的绝对路径
  const env = loadEnv(mode, root)
  const viteEnv = wrapperEnv(env)
  // viteEnv就是当前环境变量
  return {
    plugins: [vue()]
  }
})

~~~

3. 与src文件夹同级build中创建createEnv.ts文件

~~~typescript
import { ViteEnv } from '@/vite-env'
export function wrapperEnv(envConf: Recordable): ViteEnv {
  const ret: any = {};

  for (const envName of Object.keys(envConf)) {
    let realName = envConf[envName].replace(/\\n/g, "\n");
    realName = realName === "true" ? true : realName === "false" ? false : realName;
    if (envName === "VITE_PORT") realName = Number(realName);
    if (envName === "VITE_PROXY") {
      try {
        realName = JSON.parse(realName);
      } catch (error) {}
    }
    ret[envName] = realName;
  }
  return ret;
}
~~~

---

## 配置 plugins 

1. 在与src同级的build中创建plugins.ts文件
   1. vite-plugin-eslint 是让 eslint 的错误显示在页面上
   2. vite-plugin-vue-setup-extend 插件 是为了可以在script标签上定义组件name
   3. vite-plugin-compression 插件 是为了开启压缩， 进一步减小体积
      - VITE_BUILD_COMPRESS_DELETE_ORIGIN_FILE：压缩配置gzip或者brotli 压缩打包， 如果需要多个， 使用逗号分割
      - VITE_BUILD_COMPRESS：使用什么打包， 使用多个打包使用,分割
   4. rollup-plugin-visualizer 是为了在打包时生成一个依赖声明管理html

~~~typescript
import { PluginOption } from "vite";
import vue from '@vitejs/plugin-vue'
import eslintPlugin from "vite-plugin-eslint"
import viteSetupExtend from 'vite-plugin-vue-setup-extend'

import { ViteEnv } from "@/vite-env";
import viteCompression from 'vite-plugin-compression'
import { visualizer } from "rollup-plugin-visualizer";


export const createVitePlugins = (viteEnv: ViteEnv, isBuild = false):(PluginOption | PluginOption[])[] => {
  return [
    vue(),
    //name 可以写在 script 标签上
    viteSetupExtend(),
    // eslint 信息显示在页面
    eslintPlugin(),
    isBuild && createCompression(viteEnv),
    isBuild && (visualizer({
      filename: "stats.html",
      gzipSize: true,
      brotliSize: true
    }))
  ] as PluginOption[] | PluginOption[][]
}

/**
 * 根据 compress 配置，生成不同的压缩规则
 * @param viteEnv
 */
const createCompression = (viteEnv: ViteEnv): PluginOption | PluginOption[] => {
  const { VITE_BUILD_COMPRESS = "none", VITE_BUILD_COMPRESS_DELETE_ORIGIN_FILE } = viteEnv;
  const compressList = VITE_BUILD_COMPRESS.split(",");
  const plugins: PluginOption[] = [];
  if (compressList.includes("gzip")) {
    plugins.push(
      viteCompression({
        ext: ".gz",
        algorithm: "gzip",
        deleteOriginFile: VITE_BUILD_COMPRESS_DELETE_ORIGIN_FILE
      })
    );
  }
  if (compressList.includes("brotli")) {
    plugins.push(
      viteCompression({
        ext: ".br",
        algorithm: "brotliCompress",
        deleteOriginFile: VITE_BUILD_COMPRESS_DELETE_ORIGIN_FILE
      })
    );
  }

  return plugins;
};
~~~

2. 在vite.config.ts中引入

~~~typescript
import { defineConfig, loadEnv } from 'vite'
import { createVitePlugins } from "./build/plugins";
import { wrapperEnv } from "./build/createEnv";

// https://vitejs.dev/config/
export default defineConfig(({ command, mode }) => {
  const root = process.cwd()
  // root就是项目在磁盘上的绝对路径
  const env = loadEnv(mode, root)
  const viteEnv = wrapperEnv(env)
  // viteEnv就是当前环境变量

  return {
    plugins: createVitePlugins(viteEnv, command === 'build'),
  }
})
~~~

---

## 配置 @ 等符号

1. 在vite.config.ts中

   如果出现报错则需要安装 @types/node 插件， 对node类型进行补充

~~~typescript
import { defineConfig, loadEnv } from 'vite'
import { resolve } from 'path'
import { createVitePlugins } from "./build/plugins";
import { wrapperEnv } from "./build/createEnv";

// https://vitejs.dev/config/
export default defineConfig(({ command, mode }) => {
  const root = process.cwd()
  // root就是项目在磁盘上的绝对路径
  const env = loadEnv(mode, root)
  const viteEnv = wrapperEnv(env)
  // viteEnv就是当前环境变量

  return {
    plugins: createVitePlugins(viteEnv, command === 'build'),
    resolve: {
       alis: {
           '@': resolve(root, './src'),
           ...
       }
    }
  }
})
~~~

2. 在tsconfig.json中的compilerOptions中放入

~~~json
"baseUrl": "./",
"paths": {
  "@/*":[
    "src/*",
    ...
  ]
}
~~~

---

## 如果是移动端， 移动端适配

1. vite.config.ts中

~~~typescript
import { defineConfig, loadEnv } from 'vite'
import { resolve } from 'path'
import { createVitePlugins } from "./build/plugins";
import { wrapperEnv } from "./build/createEnv";

// https://vitejs.dev/config/
export default defineConfig(({ command, mode }) => {
  const root = process.cwd()
  // root就是项目在磁盘上的绝对路径
  const env = loadEnv(mode, root)
  const viteEnv = wrapperEnv(env)
  // viteEnv就是当前环境变量

  return {
    plugins: createVitePlugins(viteEnv, command === 'build'),
    resolve: {
       alis: {
           '@': resolve(root, './src'),
           ...
       }
    },
    css: {
        postcss: {
            plugins: [
                pxtoViewPort({
                    unitToConvert: 'px',
                    viewportWidth: 375, // UI设计稿的宽度
                    unitPrecision: 6, // 转换后的精度，即小数点位数
                    propList: ['*'], // 指定转换的css属性的单位，*代表全部css属性的单位都进行转换
                    viewportUnit: 'vw', // 指定需要转换成的视窗单位，默认vw
                    fontViewportUnit: 'vw', // 指定字体需要转换成的视窗单位，默认vw
                    selectorBlackList: ['ignore-'], // 指定不转换为视窗单位的类名，
                    minPixelValue: 1, // 默认值1，小于或等于1px则不进行转换
                    mediaQuery: true, // 是否在媒体查询的css代码中也进行转换，默认false
                    replace: true, // 是否转换后直接更换属性值
                    landscape: false // 是否处理横屏情况
                })
            ]
        }
    }
  }
})

~~~

2. 在src下新建types文件夹加入postcss-px-vireport.d.ts

~~~typescript
declare module "postcss-px-to-viewport" {
  type Options = {
    unitToConvert: "px" | "rem" | "cm" | "em"
    viewportWidth: number
    viewportHeight: number // not now used;
    unitPrecision: number
    viewportUnit: string
    fontViewportUnit: string // vmin is more suitable.
    selectorBlackList: string[]
    propList: string[]
    minPixelValue: number
    mediaQuery: boolean
    replace: boolean
    landscape: boolean
    landscapeUnit: string
    landscapeWidth: number
  }

  export default function (options: Partial<Options>): any
}
~~~

## eslint

1. eslintignore

~~~text
node_modules
*.md
.vscode
.idea
dist
/public 
stats.html //打包后 在项目根目录生成的 依赖分析文件
...
~~~

2. eslintrc.cjs

**需要下载的插件**

1. eslint
2. eslint-config-prettier
3. eslint-plugin-prettier
4. eslint-plugin-vue
5. @typescript-eslint/eslint-plugin
6. @typescript-eslint/parser
7. vite-plugin-eslint
8. prettier

~~~cjs
// @see: http://eslint.cn

module.exports = {
  root: true,
  env: {
    browser: true,
    node: true,
    es6: true
  },
  // 指定如何解析语法
  parser: "vue-eslint-parser",
  // 优先级低于 parse 的语法解析配置
  parserOptions: {
    parser: "@typescript-eslint/parser",
    ecmaVersion: 2020,
    sourceType: "module",
    jsxPragma: "React",
    ecmaFeatures: {
      jsx: true
    }
  },
  // 继承某些已有的规则
  extends: [
    "plugin:vue/vue3-recommended",
    "plugin:@typescript-eslint/recommended",
    "plugin:prettier/recommended"
  ],
  /**
   * "off" 或 0    ==>  关闭规则
   * "warn" 或 1   ==>  打开的规则作为警告（不影响代码执行）
   * "error" 或 2  ==>  规则作为一个错误（代码不能执行，界面报错）
   */
  rules: {
    // eslint (http://eslint.cn/docs/rules)
    "no-var": "error", // 要求使用 let 或 const 而不是 var
    "no-multiple-empty-lines": [ "error", { max: 1 } ], // 不允许多个空行
    "prefer-const": "off", // 使用 let 关键字声明但在初始分配后从未重新分配的变量，要求使用 const
    "no-use-before-define": "off", // 禁止在 函数/类/变量 定义之前使用它们
    "no-irregular-whitespace": "off", // 禁止不规则的空白

    // typeScript (https://typescript-eslint.io/rules)
    "@typescript-eslint/no-unused-vars": "error", // 禁止定义未使用的变量
    "@typescript-eslint/prefer-ts-expect-error": "error", // 禁止使用 @ts-ignore
    "@typescript-eslint/no-inferrable-types": "off", // 可以轻松推断的显式类型可能会增加不必要的冗长
    "@typescript-eslint/no-namespace": "off", // 禁止使用自定义 TypeScript 模块和命名空间。
    "@typescript-eslint/no-explicit-any": "off", // 禁止使用 any 类型
    "@typescript-eslint/ban-types": "off", // 禁止使用特定类型
    "@typescript-eslint/explicit-function-return-type": "off", // 不允许对初始化为数字、字符串或布尔值的变量或参数进行显式类型声明
    "@typescript-eslint/no-var-requires": "off", // 不允许在 import 语句中使用 require 语句
    "@typescript-eslint/no-empty-function": "off", // 禁止空函数
    "@typescript-eslint/no-use-before-define": "off", // 禁止在变量定义之前使用它们
    "@typescript-eslint/ban-ts-comment": "off", // 禁止 @ts-<directive> 使用注释或要求在指令后进行描述
    "@typescript-eslint/no-non-null-assertion": "off", // 不允许使用后缀运算符的非空断言(!)
    "@typescript-eslint/explicit-module-boundary-types": "off", // 要求导出函数和类的公共类方法的显式返回和参数类型

    // vue (https://eslint.vuejs.org/rules)
    "vue/script-setup-uses-vars": "error", // 防止<script setup>使用的变量<template>被标记为未使用，此规则仅在启用该no-unused-vars规则时有效。
    "vue/v-slot-style": "error", // 强制执行 v-slot 指令样式
    "vue/no-mutating-props": "off", // 不允许组件 prop的改变
    "vue/no-v-html": "off", // 禁止使用 v-html
    "vue/custom-event-name-casing": "off", // 为自定义事件名称强制使用特定大小写
    "vue/attributes-order": "off", // vue api使用顺序，强制执行属性顺序
    "vue/one-component-per-file": "off", // 强制每个组件都应该在自己的文件中
    "vue/html-closing-bracket-newline": "off", // 在标签的右括号之前要求或禁止换行
    "vue/max-attributes-per-line": "off", // 强制每行的最大属性数
    "vue/multiline-html-element-content-newline": "off", // 在多行元素的内容之前和之后需要换行符
    "vue/singleline-html-element-content-newline": "off", // 在单行元素的内容之前和之后需要换行符
    "vue/attribute-hyphenation": "off", // 对模板中的自定义组件强制执行属性命名样式
    "vue/require-default-prop": "off", // 此规则要求为每个 prop 为必填时，必须提供默认值
    "vue/multi-word-component-names": "off", // 要求组件名称始终为 “-” 链接的单词
    "prettier/prettier": "off"
  }
};
~~~

## prettier

1. prettierignore

~~~text
/dist/*
/node_modules/**

**/*.svg
stats.html
~~~



1. prettier.cjs

~~~cjs
// @see: https://www.prettier.cn

module.exports = {
  // 指定最大换行长度
  printWidth: 80,
  // 缩进制表符宽度 | 空格数
  tabWidth: 2,
  // 使用制表符而不是空格缩进行 (true：制表符，false：空格)
  useTabs: false,
  // 结尾不用分号 (true：有，false：没有)
  semi: false,
  // 使用单引号 (true：单引号，false：双引号)
  singleQuote: false,
  // 在对象字面量中决定是否将属性名用引号括起来 可选值 "<as-needed|consistent|preserve>"
  quoteProps: "as-needed",
  // 在JSX中使用单引号而不是双引号 (true：单引号，false：双引号)
  jsxSingleQuote: false,
  // 多行时尽可能打印尾随逗号 可选值"<none|es5|all>"
  trailingComma: "none",
  // 在对象，数组括号与文字之间加空格 "{ foo: bar }" (true：有，false：没有)
  bracketSpacing: true,
  // 将 > 多行元素放在最后一行的末尾，而不是单独放在下一行 (true：放末尾，false：单独一行)
  bracketSameLine: false,
  // (x) => {} 箭头函数参数只有一个时是否要有小括号 (avoid：省略括号，always：不省略括号)
  arrowParens: "avoid",
  // 指定要使用的解析器，不需要写文件开头的 @prettier
  requirePragma: false,
  // 可以在文件顶部插入一个特殊标记，指定该文件已使用 Prettier 格式化
  insertPragma: false,
  // 用于控制文本是否应该被换行以及如何进行换行
  proseWrap: "preserve",
  // 在html中空格是否是敏感的 "css" - 遵守 CSS 显示属性的默认值， "strict" - 空格被认为是敏感的 ，"ignore" - 空格被认为是不敏感的
  htmlWhitespaceSensitivity: "css",
  // 控制在 Vue 单文件组件中 <script> 和 <style> 标签内的代码缩进方式
  vueIndentScriptAndStyle: false,
  // 换行符使用 lf 结尾是 可选值 "<auto|lf|crlf|cr>"
  endOfLine: "auto",
  // 这两个选项可用于格式化以给定字符偏移量（分别包括和不包括）开始和结束的代码 (rangeStart：开始，rangeEnd：结束)
  rangeStart: 0,
  rangeEnd: Infinity
};
~~~

## 样式重置css 通过包管理器下载

**normalize.css**



## 其他配置

root: 根目录

webpack的publicPath => vite平替 base

## 报错问题

### 引入vue文件报错问题

在vite-env.d.ts中放入

~~~typescript
declare module '*.vue' {
	import type { DefineComponent } from 'vue'
	const component: DefineComponent<{}, {}, any>
	export default component
}
~~~

