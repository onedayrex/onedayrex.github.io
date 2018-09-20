
---
title: gradle包冲突查找解决方法
---

```
gradle projects     查看项目目录
gradle :目录名称:dependencies  >> d:/api.text     查看依赖
```
通过以上查询到引用包所依赖的包

在引用包处使用
```
compile ("com.msxf.feedback:feedback-sdk:${feedback_version}"){
   exclude group:'org.slf4j', module: 'slf4j-log4j12'
}
```
排除依赖
