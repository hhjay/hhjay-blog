---
title: 链表和数组的区别
date: 2018-04-16 13:59:26
tags: js
---

## 链表
- 是一种基础数据结构，是一种线性表，并不是按线性的顺序存贮数据，而是在每一个节点里存到下一个节点的指针
- 链表增加了节点的指针域，空间开销比较大
- 不指定大小，拓展方便

## 数组
- 有序的元素序列，在内存中是一块连续的区域
- 要预留内存
- 不利于拓展

## 增、删、改操作数据的效率
- 增、删
    - 链表是指针指向的地址是线性的，所以只需要把指向修改即可，为O(1)
    - 数组是按地址存贮数据的，所以要把当前的地址给修改，再把之前的数组给连接上，为O(n)
- 读
    - 链表不能随机查找，只能从第一个开始遍历，查找效率低为O(n)
    - 数组读取效率很高，因为数组是连续的，知道每一个的内存地址就可以直接读取到数据，为O(1)

## 参考链接
- [维基.链表](https://zh.wikipedia.org/wiki/%E9%93%BE%E8%A1%A8)
- [百度.数组](https://baike.baidu.com/item/%E6%95%B0%E7%BB%84)