[TOC]

# rollup学习笔记

## 要下载的包

### 命令

npm i @babel/core @babel/preset-env rollup rollup-plugin-babel rollup-plugin-serve -D

- @babel/core 
- @babel/preset-env
- rollup 
- rollup-plugin-babel
- rollup-plugin-serve

## rollup配置

### rollup.config.js

```json
import babel from 'rollup-plugin-babel'
import serve from 'rollup-plugin-serve'

export default {
    input: './src/index,js',
    output: {
        file: 'dist/vue.js',
        format: 'umd',  // umd打包方式 => 会在window上生成一个下面的name变量
        name: 'Vue',    // 放到window上的变量名
        sourcemap: true, // 将转化前的代码与转化后的代码进行映射
    },
    plugins:[
      //将高级语法转换成低级语法
      babel({
          exclude: 'node_modules/**' //排除node_modules下面所有的文件, 不转换node_modules下的文件
      }),
      // 打开服务
      serve({
          port: 3000, // 打开的服务的端口号
          contentBase: '', // 空字符串代表当前目录
          openPage: '/index.html' // 要打开的页面
      })
    ]
}

```

### .babelrc

```json
{
  "presets": [
    "@babel/preset-env"
  ]
}
```

## 启动配置

### package.json

```json
{
  "scripts": {
    "dev": "rollup -c -w", 
    "test": "echo \"Error: no test specified\" && exit 1"
```



**scripts中增加dev || \*\* : rollup -c -w** 

- -c 代表 执行配置文件
- -w 代表 热刷新