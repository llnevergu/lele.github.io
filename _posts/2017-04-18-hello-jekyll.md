---
layout: post
title: 'Hello woniu'
date: 2020-04-02
author: woniu
cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: woniu
---

> 通过ReetrantLock了解AQS.

### 先看下AQS的数据结构

数据结构的解释：
exclusiveOwnerThread: 用于存储当前持有锁的线程
state: 0表示没有线程持有锁，
          大于0的话，分两种情况：
            1）对于独占锁，state代表重入次数；
            2）对于共享锁，可用于表示当前还有多少线程持久锁（描述可能不准确）
Node节点：是一个双向链表，用于存储阻塞等待的线程相关数据，
            数据区有三个字段：
            waitStatus表示对应节点（也用于后继节点）的状态；
            thread指向对应的线程；
            nextWaiter:解释见上图文本说明，
ps：关于共享和独占模式，这里有个比较灵性的涉及，如果nextWaiter为null，则为独占，否则为共享模式；
这样有个好处，使用ReetrantLock的过程中，将节点从条件队列移入至同步队列中时，将节点的nextWaiter置为null即可

### 类图



### 代码段

static ReetrantLock lock = new ReetrantLock();
void method1() {
    try{
        lock.lock();
        // do sth...
    } finally {
        lock.unlock();
    }
}

### 先来个最简单的场景
t1 获取锁
t2 尝试获取锁，被阻塞；
t1 释放锁；
t2 获取锁成功；
t3 尝试获取锁，被阻塞；

### 来直观感受下AQS的同步队列变化情况：
