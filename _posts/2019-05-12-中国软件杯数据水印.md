---
layout: post
title: "基于追溯处理机制的运维服务大数据的安全与共享"
categories: [contest]
excerpt: "数据共享后缺乏有效的监管手段，需要针对共享方在数据中增加特定的水印来保障数据的可追溯性，在共享方非法出售数据后，通过数据中的水印判别出售者的身份，便于追溯。"
image:
  feature: https://github.com/Feithle/Feithle.github.io/blob/master/img/PagesImges/data-security.jpg?raw=true
  credit: Greg Rakozy
---
# 单链表的基本操作
## 创建
**利用数组中的元素创建单链表**
{% highlighe ruby %}
#include <stdio.h>
#include <stdlib.h>
#include <malloc.h>
typedef struct LNode {//定义结点
	int data;
	struct LNode* next;
}LNode,* LinkList;

/*
头插法，数组转化为单链表，达到创建单链表的目的
*/
LinkList createLinkListFromHead_1(int a[],int n) {//a是待插入的数组，n是插入元素
	int i, x;
	LNode* L, * p;
	L = (LNode *)malloc(sizeof(LNode));//头结点
	L->next = NULL;
	//简单循环插入
	for (i = 0; i < n;i++) {//头插法核心代码
		p = (LNode*)malloc(sizeof(LNode));//创建新结点
		p->data = a[i];//为新结点值域赋值
		p->next = L->next;
		L->next = p;
	}
	return L;
}
{% endhighlight %}
