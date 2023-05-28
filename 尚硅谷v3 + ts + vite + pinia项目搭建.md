[toc]

# 项目搭建

## 一、项目初始化

### 1.1、环境准备

- node v 17.0.1
- pnpm 8.5.0

### 1.2、初始化项目

~~~javascript
pnpm create vite //使用 pnpm 创建 vite 项目
~~~

## 二、项目配置

### 一、eslint 配置

**eslint 中文官网：http://eslint.cn/**

Eslint 最初是由 Nicholas C. Zakas 与 2013 年 6 月创建的开源项目，它的目标是提供一个插件化的 **javascript 代码检测工具 **

- 首先安装 eslint

  ~~~javascript
  pnpm add eslint -D
  ~~~

- 生成配置文件：eslint.cjs

  ~~~javascript
  npx eslint --init
  ~~~

- .eslint.cjs 配置文件

  ~~~javascript
  module.exports = {
      "env": {
          "browser": true, // 浏览器端
          "es2021": true // 也就是 es12
      },
      // 规则继承
      "extends": [
          // 全部规则默认是关闭的，这个配置项开启推荐规则，推荐规则参照文档
          // 比如：函数不能重名、对象不能出现重复 key
          "eslint:recommended",
          // vue3 的语法规则
          "plugin:vue/vue3-essential",
          // ts 的语法规则
          "plugin:@typescript-eslint/recommended"
      ],
      // 要为特定类型的文件指定处理器  --->  比如说 markdown
      "overrides": [
      ],
      // 指定解析器：解析器
      // Esprima 默认解析器
      // Babel-Eslint babel解析器
      // @typescript-eslint/parser ts解析器
      "parser": "@typescript-eslint/parser",
      // 指定解析器选项 
      "parserOptions": {
          "ecmaVersion": "latest", // 校验 ECMA 最新版本
          "sourceType": "module" // 设置为 “script” (默认)，或者"module"代码在 ECMAScript 模块中
      },
      // ESlint 支持使用第三方插件。在使用插件之前，必须先安装它
      // 该 eslint-plugin- 前缀可以从插件名称被省略
      "plugins": [
          "vue",
          "@typescript-eslint"
      ],
      // eslint 规则
      "rules": {
      }
  }
  
  ~~~

#### 1.1、vue3 环境代码校验插件

~~~javascript
# 让所有与 prettier 规则存在冲突的 Eslint rules 失效，并使用 prettier 进行代码检查
"eslint-config-prettier"： "^8.8.0",
"eslint-plugin-import": "^2.27.5",
"eslint-plugin-node": "^11.1.0",
# 运行更漂亮的 Eslint，使 prettier 规则优先级更高，Eslint 优先级低
"eslint-plugin-prettier": "^4.2.1",
# vue.js 的 Eslint 插件(查找 vue 的语法错误，发现错误指令，查找违规风格指南)
"eslint-plugin-vue": "^9.14.1",
# 该解析器允许使用 Eslint 校验所有 babel code
"@babel/eslint-parser": "^7.21.8"
~~~

安装指令

~~~javascript
pnpm add -D eslint-plugin-import eslint-plugin-vue eslint-plugin-node eslint-plugin-prettier eslint-config-prettier @babel/eslint-parser
~~~

#### 1.2、修改 .eslintrc.cjs 配置文件

~~~javascript
// @see https://eslint.bootcss.com/docs/rules/

module.exports = {
	"env": {
		"browser": true,
		"es2021": true,
		/* ----------------- 下面是新增的 */
		"node": true,
		"jest": true
	},
	/* 指定如何解析语法 */
	"parser": "vue-eslint-parser", /* 这里修改了 */
	/* 优先级低于 parse 的语法解析配置 */
	"parserOptions": {
		"ecmaVersion": "latest",
		"sourceType": "module",
		/* ----------------- 下面是新增的 */
		"parser": "@typescript-eslint/parse",
		"jsxPragma": "React",
		"ecmaFeatures": {
			jsx: true
		}
	},
	/* 继承已有的规则 */
	"extends": [
		"eslint:recommended",
		"plugin:vue/vue3-essential",
		"plugin:@typescript-eslint/recommended",
		"plugin:prettier/recommended"
	],
	"plugins": [ "vue", "@typescript-eslint" ],
	/*
	 *	"off" 或 0    ===>  关闭规则
	 *	"warn" 或 1   ===>  打开的规则作为警告(不影响代码执行)
	 *	"error" 或 2  ===>  规则作为一个错误(代码不能执行，界面报错)
	 */
	"rules": {
		// eslint (http://eslint.bootcss.com/docs/rules/)
		"no-var": "error", //要求使用 let 或 const 而不是 var
		"no-multiple-empty-lines": [ "warn", { max: 1 } ], // 不允许多个空行
		"no-console": process.env.NODE_ENV === "production" ? "error" : "off",
		"no-debugger": process.env.NODE_ENV === "production" ? "error" : "off",
		"no-unexpected-multiline": "error", // 禁止空余的多行
		"no-useless-escape": "off", // 禁止不必要的转义字符

		// typescript (https://typescript-eslint.io/rules)
		"@typescript-eslint/no-unused-vars": "error", // 禁止定义未使用的变量
		"@typescript-eslint/prefer-ts-expect-error": "error", // 禁止使用 @ts-ignore
		"@typescript-eslint/no-explicit-any": "off", // 禁止使用 any 类型
		"@typescript-eslint/no-non-null-assertion": "off",
		"@typescript-eslint/no-namespace": "off", // 禁止使用自定义 TypeScript 模块和命名空间
		"@typescript-eslint/semi": "off",

		// eslint-plugin-vue (https://eslint.vuejs.org/rules/)
		"vue/multi-word-component-names": "off", // 要求组件名称始终为 "-" 链接的单词
		"vue/script-setup-uses-vars": "error", // 防止 <script setup> 使用的变量 <template> 被标记未使用
		"vue/no-mutating-props": "off", // 不允许组件 prop 的改变
		"vue/attribute-hyphenation": "off", // 对模板的自定义组件强制执行属性命名样式
	}
}

~~~

#### 1.3、.eslintignore 配置 eslint 忽略哪些文件

~~~javascript
dist
node_modules
~~~

#### 1.4、运行脚本

**package.json 新增两个运行脚本**

~~~json
"scripts": {
    "lint": "eslint src",
    "fix": "eslint src --fix"
}
~~~

### 二、prettier 配置

有了 eslnt，为什么还要有  prettier？

- eslint 针对的是 javascript，它是一个检测工具，包含 js 语法以及少部分格式问题，在 eslint 看来，语法对了就能保证代码正常运行，格式问题属于其次

- 而 prettier 属于格式化工具，它看不惯格式不统一，所以它就把 eslint 没干好的事情接着干，另外，prettier 支持包含 js 在内的多种语言

**总结：eslint 和 prettier 这两兄弟一个保证 js 代码质量，一个保证代码美观**

#### 2.1、安装依赖包

~~~javascript
pnpm add -D eslint-plugin-prettier prettier eslint-config-prettier
~~~

#### 2.2、.prettierrc.json 添加规则

~~~json
{
    "singleQuote": true, // 要求字符串都是单引号
    "semi": false, // 语句最后加不加分好
    "bracketSpacing": true,
    "htmlWhitespaceSensitivity": "ignore",
    "endOfLine": "auto",
    "trailingComma": "all",
    "tabWidth": 2 // 
}
~~~

#### 2.3、.prettierignore 配置 prettier 忽略哪些文件

~~~
/dist/*
/html/*
.local
/node_modules/**
**/*.svg
**/*.sh
/public/*
~~~

**通过 pnpm run lint 去检测语法，如果出现不规范格式，通过 pnpm run fix 格式化**

### 三、stylelint 配置

stylelint 为 css 的 lint 工具，可格式化 css 代码，检查 css 代码错误与不合理的写法，指定 css 的书写顺序等

项目中使用 scss 作为预处理器，安装以下依赖：

~~~javascript
pnpm add sass sass-loader stylelint postcss postcss-scss postcss-html stylelint-config-prettier stylelint-config-recess-order stylelint-config-recommended-scss stylelint-config-standard stylelint-config-standard-vue stylelint-scss stylelint-order stylelint-config-standard-scss -D
~~~

#### 3.1、.stylelintrc.cjs 配置文件

**官网：https://stylelint.bootcss.com/**

~~~javascript
// @see https://stylelint.bootcss.com/

module.exports = {
    extends: [
        "stylelint-config-standard", // 配置 stylelint 拓展插件
        "stylelint-config-html/vue", // 配置 vue 中 template 样式格式化
        "stylelint-config-standard-scss", // 配置 stylelint scss 插件
        "stylelint-config-recommended-vue/scss", // 配置 vue 中的 scss 样式格式化
        "stylelint-config-recess-order", // 配置 stylelint css 属性书写顺序插件
        "stylelint-config-prettier", // 配置 stylelint 和 prettier 兼容
    ],
    overrides: [
        {
            files: ["**/*.(scss|css|vue|html)"],
            customSyntax: "postcss-scss"
        },
        {
            files: ["**.*.(html|vue)"],
            customSyntax: "postcss-html"
        }
    ],
    ignoreFiles: [
        "**/*.js",
        "**/*.jsx",
        "**/*.tsx",
        "**/*.ts",
        "**/*.json",
        "**/*.md",
        "**/*.yaml"
    ],
    /**
     * null	=> 关闭改规则
     * always => 必须
     */
    rules: {
        "value-keyword-case": null, // 在 css 中使用 v-bind，不报错
        "no-descending-specificity": null, // 禁止具有较高优先级的选择器后出现被其覆盖的较低优先级的选择器
        "function-url-quotes": "always", // 要求禁止 URL 的引号 "always(必须加上引号)" | "never(没有引号)"
        "no-empty-source": null, // 关闭禁止空源码
        "selector-class-pattern": null, // 关闭强制选择器类名的格式
        "property-no-unknown": null, // 禁止位置的属性(true 为不允许)
        "block-opening-brace-space-before": "always", // d大扩好之前必须有一个空格或不能有空白符
        "value-no-vendor-prefix": null, // 关闭属性值前缀 --webkit-box
        "property-no-vendor-prefix": null, // 关闭属性前缀 --webkit-mask
        "selector-pseudo-class-no-unknown": [
            // 不允许位置的选择器
            true,
            {
                ignorePseudoClasses: ["global", "v-deep", "deep"], // 忽略属性，修改 element 默认样式的时候能使用到
            }
        ]
    }
}
~~~

#### 3.2、.stylelintignore 配置 stylelint 忽略哪些文件

~~~
/node_modules/*
/dist/*
/html/*
/public/*
~~~

#### 3.3、运行脚本

~~~json
"scripts": {
    "lint:style": "stylelint src/**/*.{css,scss,vue} --cache --fix"
}
~~~

最后配置统一的 prettier 来格式化我们的 js 和 css，html 代码

~~~json
"scripts": {
    "dev": "vite --open", // --open 是运行项目后自动打开网页
    "build": "vue-tsc && vite build",
    "preview": "vite preview",
    "lint": "eslint src",
    "fix": "eslint src --fix",
    "format": "prettier --write \"./**/*.{html,vue,ts,js,json,md}\"",
    "lint:eslint": "eslint src/**/*.{ts,vue} --cache --fix",
    "lint:style": "stylelint src/**/*.{css,scss,vue} --cache --fix"
}
~~~

### 四、husky 配置

在上面我们已经继承好了代码校验工具，但是需要每次手动的执行命令才会格式化代码。如果有人没有格式化代码就提交了远程仓库中，那这个规范就没有了意义，所以我们需要强制让开发人员按照代码规范来提交

要做到这件事情，就需要利用 husky 在代码提交之前出发 git hook (git 在客户端的钩子)，然后执行 `pnpm run format` 来自动的格式化代码

**安装 husky**

~~~javascript
pnpm add husky -D
~~~

**执行**

~~~javascript
npx husky-init
~~~

**会在根目录下生成一个 .husky 目录，在这个目录下面会有一个 pre-commit 文件，这个文件里面的命令在我们执行 commit 的时候就会执行**

在`.husky/pre-commit`文件添加如下命令

~~~javascript
#~/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.sh"
pnpm run format
~~~

当我们对代码执行 commit 操作的时候，就会执行命令，对代码进行格式化，然后再提交

### 五、commitlint 配置

对于我们的 commit 信息，也是有统一规范的，不能随便写，要让每个人都按照统一的标准来执行，我们可以利用 commitlint 来实现

安装包

~~~javascript
pnpm add @commitlint/config-conventional @commitlint/cli -D
~~~

添加配置文件，新建`commitlint.config.cjs` ( 注意是 cjs )，然后添加下面的代码：

~~~javascript
module.exports = {
    extends: ["@commitlint/config-conventional"],
    // 校验规则
    rules: {
        "type-enum": [
            2,
            "always",
            [
                "feet",
                "fix",
                "docs",
                "style",
                "refactor",
                "perf",
                "test",
                "chore",
                "revert",
                "build"
            ]
        ],
        "type-case": [0],
        "type-empty": [0],
        "scope-empty": [0],
        "scope-case": [0],
        "subject-full-stop": [0, "never"],
        "subject-case": [0, "never"],
        "header-max-length": [0, "always", 72]
    }
}
~~~

在 `package.json`中配置 scripts 命令

~~~javascript
# 在 scripts 中添加下面的代码
"scripts": {
    "commitlint": "commitlint --config commitlint.config.cjs -e -V"
}
~~~

配置结束，现在当我们填写`commit`信息的视乎，前面就需要带着下面的 `subject`

~~~javascript
"feat", // 新特性，新功能
"fix", // 修改bug
"docs", // 文档修改
"style", // 代码格式修改，注意不是 css 修改
"refactor", // 代码重构
"perf", // 优化相关，比如提升性能、体验
"test", // 测试用例修改
"chore", // 其他修改，比如改变构建流程、或者增加依赖库、工具等
"revert", // 回滚到上一个版本
"build", // 编译相关的修改，例如发布版本，对项目构建或者依赖的改动
~~~

配置 husky

~~~javascript
npx husky add .husky/commit-msg
~~~

在生成的 commit-msg 文件中添加下面的命令

~~~
#!/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.sh"
pnpm commitlint
~~~

这样的话 当我们 commit 提交信息时，就不能再随意写了，必须是 `git commit -m "fix: xxx"`符合类型的才可以，**需要注意的是类型的后面需要用英文的冒号，并且冒号后面是需要空一格的，这个是不能省略的**

### 六、强制使用 pnpm 下载包

为了统一我们的包管理工具，我们需要强制让用户使用 pnpm 来安装依赖包

在根目录创建 `scripts/preinstall.js`文件，添加下面的内容

~~~javascript
if(!/pnpm/.test(process.env.npm_execpath || "")){
    console.warn(
    	`\u001[33mThis repository must using pnpm as the package manager` +
        `for scripts to work properly.\u001b[39m\n`,
    )
    process.exit(1)
}
~~~

配置命令

~~~
"scripts": {
	"preinstall": "node ./scripts/preinstall.js"
}
~~~

**当我们使用 npm 或者 yarn 来安装包的时候，就会报错了，原理就是在 install 的时候会触发 preinstall ( npm 提供的生命周期钩子 ) 这个文件里面的代码**

### 三、项目集成

#### 3.1 集成 element-plus

**官网：https://element-plus.gitee.io/zh-CN/**

~~~javascript
pnpm add element-plus @element-plus/icons-vue
~~~

**入口文件 main.ts 全局安装 element-plus， element-plus 默认支持语言英语设置成中文**

~~~typescript
import ElementPlus from "element-plus"
import "element-plus/dist/index.css"
// @ts-ignore 忽略当前文件 ts 类型的检测否则有红色提示(打包会失败)
import zhCn from 'element-plus/dist/locale/zh-cn.mjs'
app.use(ElementPlus, {
    locale: zhCn
})
~~~

**Element Plus 全局组件类型声明**

~~~json
// ts.config.json
{
    "compilerOptions": {
        //...
        "types": ["element-plus/global"]
    }
}
~~~

**配置完毕可以测试 `element-plus` 组件与图表的使用**

#### 3.2、src路径别名的配置

在开发项目的时候文件和文件关系可能很负责，引入我们需要给 src 文件夹配置一个别名。

~~~typescript
// vite.config.ts
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import { resolve } from 'path'
export default defineConfig(({ command, mode }) => {
  const root = process.cwd()
  // root就是项目在磁盘上的绝对路径
  return {
    plugins: [vue()],
    resolve: {
        alias: {
            "@": resolve(root, './src') // 相对路径别名配置，使用 @ 代替 src
        }
    }
  }
})
~~~

**TypeScript 编译配置**

~~~json
// tsconfig.json
{
    "compilerOptions": {
        "baseUrl": "./", // 解析非相对模块的基地址，默认是当前目录
        "paths": {
            "@/*": ["src/*"]
        }
    }
}
~~~

