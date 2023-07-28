---
title: vue+nwjs
date: 2023-07-029 11:35:03
tags: vue nwjs
---

## 1、创建正常的vue项目

使用`vue-cli`工具创建`vue`项目

```bash
#全局安装或更新vue-cli工具
npm i -g @vue/cli

#创建vue项目
vue create 项目名

#创建好后先运行看vue是否搭建完成
cd 项目目录
yarn serve
```



## 2、添加nw.js依赖

```bash
#在vue项目下安装nw.js依赖，这里安装在当前项目下，也可以全局安装，这里只安装在项目dev环境中
yarn add nw --dev
```



## 3、修改`package.json`添加`nw.js`配置

### 添加nw.js主配置

在`package.json`中添`nw.js`需要的配置，添加`main`，这里配置为需要启动`nw.js`的路径或者`url`，这里配置为`vue`启动后的开发地址`http://localhost:8080`

### 添加启动参数

原来`vue`启动是使用`yarn serve`，在`package.json`中可以看到最终的命令是`vue-cli-service serve`，我们需要做的事情第一是编译构建启动`vue`，前面的命令就能满足，第二是启动`nw.js`并且把`vue`的内容套在`nw.js`中，`nw.js`的启动命令是`nw .`，在`vue-cli-service serve`命令后加上就行需要注意这里使用`concurrently`来添加`nw .`，因为`vue`启动后是挂起的，使用`concurrentley`表示同时执行两个命令，如果用`&&`则会出现`vue`启动后一直挂起没有启动`nw.js`的情况，完整的`package.json`如下：

```json
{
  "name": "conver",
  "version": "0.1.0",
  "main": "http://localhost:8080",
  "private": true,
  "scripts": {
    "serve": "concurrently \"vue-cli-service serve\" \"nw .\"",
    "build": "vue-cli-service build",
    "lint": "vue-cli-service lint"
  },
  "dependencies": {
    "core-js": "^3.8.3",
    "vue": "^3.2.36",
    "vue-router": "4"
  },
  "devDependencies": {
    "@babel/core": "^7.12.16",
    "@babel/eslint-parser": "^7.12.16",
    "@vue/cli-plugin-babel": "~5.0.0",
    "@vue/cli-plugin-eslint": "~5.0.0",
    "@vue/cli-service": "~5.0.0",
    "concurrently": "^8.2.0",
    "eslint": "^7.32.0",
    "eslint-plugin-vue": "^8.0.3",
    "nw": "^0.78.0"
  },
  "eslintConfig": {
    "root": true,
    "env": {
      "node": true
    },
    "extends": [
      "plugin:vue/vue3-essential",
      "eslint:recommended"
    ],
    "parserOptions": {
      "parser": "@babel/eslint-parser"
    },
    "rules": {}
  },
  "browserslist": [
    "> 1%",
    "last 2 versions",
    "not dead",
    "not ie 11"
  ]
}


```

