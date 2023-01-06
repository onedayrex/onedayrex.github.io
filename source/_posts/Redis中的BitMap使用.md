---
title: Redis中的BitMap使用
date: 2023-01-07 01:11:03
tags: Redis BitMap
---
## 原理
使用最小的计量单位来存储数据，只能存储0或1，每一个上是1bit，8bit=1b=0.001kb，在`redis`中一个Key对应一个bitmap,一个bitmap可以存储2^32的数据，也就是512Mb，bitmap可以看成一个数组，通过offset下标来访问。其中能存储的信息大小最多为2个，0或者1.

## 用法
```
setbit key offset value
```
`setbit`设置`bitmap`值，`key`为`bitmap`名称，`offset`为`bitmap`下标，`value`为对应的值，只能设置0或1,也就是`true`和`false`。
例如：
```
setbit sign 0 1
```
设置名称为sign的`bitmap`的第0个位置为1

```
getbit key offset
```
`getbit`获取`bitmap`的值，`key`为`bitmap`名称，`offset`为`bitmap`下标
例如：
```
getbit sign 1
```
获取名称为sign的`bitmap`的第1个位置的值

```
bitop operation destkey key [key ...]
```
通过操作多个`bitmap`来进行`AND`、`XOR`、`NOT`等来进行结果比较，值存放在destkey中
例如：对公司100名员工每天打卡做一个记录，每天生成一个`bitmap`，需要统计这一个月有哪些员工是没有迟到过的，可以使用每天生成的`bitmap`做`AND`操作，结果在`destkey`中，也是一个`bitmap`
如：`bitop and result sign01 sign02`  等等，这样操作后再去`getbit`就可以得到哪些员工是1的则为没有迟到过的。

## 优点
1.基于最小的单位bit进行存储，所以非常省空间。  
2.设置时候时间复杂度O(1)、读取时候时间复杂度O(n)，操作是非常快的。  
3.二进制数据的存储，进行相关计算的时候非常快。  
4.方便扩容

## 缺点
redis中bit映射被限制在512MB之内，所以最大是2^32位。建议每个key的位数都控制下，因为读取时候时间复杂度O(n)，越大的串读的时间花销越多。
## 空间计算方式
大概的空间占用计算公式是：($offset/8/1024/1024)MB