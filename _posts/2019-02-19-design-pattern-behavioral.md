---
layout: post
title: "设计模式之行为模式"
tagline: ""
description: ""
category: 学习笔记
tags: [design-pattern, java, ]
last_updated:
---

设计模式的行为模式关注类和对象如何交互，以及如何负责对应的事务，也就是怎么定义类的行为和职责。

## 模板方法模式 {#template}
模板方法 (Template Method) 模式是类的行为模式。准备一个抽象类，将部分逻辑以具体方法以及具体构造函数的形式实现，然后声明一些抽象方法来迫使子类实现剩余的逻辑。不同的子类可以以不同的方式实现这些抽象方法，从而对剩余的逻辑有不同的实现。这就是模板方法模式的用意。

Java 的集合就是一个典型的，利用了模板方法模式的例子。Java 集合中的 Collection 集合包括 List 和 Set 两大组成部分。List 是队列，而 Set 是没有重复元素的集合。它们共同的接口都在 Collection 接口声明。

模板方法涉及到两个角色，抽象模板和具体模板。抽象模板定义：

- 一个或者多个抽象操作
- 定义并实现模板方法，给出顶级逻辑骨架，而逻辑组成的步骤在对应的抽象操作中，推迟到子类实现

具体模板：

- 实现父类所有抽象方法
- 每一个抽象模板角色都可以定义多个具体模板角色实现，从而使得顶级逻辑实现各不相同

## 观察者模式 {#observer}
观察者模式是对象的行为模式，又叫发布 - 订阅 (Publish/Subscribe) 模式、模型 - 视图 (Model/View) 模式、源 - 监听器 (Source/Listener) 模式或从属者 (Dependents) 模式。

观察者模式很好理解，举一个具体的例子，比如订阅报纸杂志，这里涉及到两个对象，一个是报社，一个是订阅者，订阅者订阅报纸，那么只要报纸有更新就送到订阅者，而如果订阅者取消订阅，那么就收不到更新了。理解这个模式就很好理解观察者模式了。

观察者模式定义了一种**一对多**的依赖关系，让多个观察者对象同时监听某一个主题对象。这个主题对象在状态上发生变化时，会通知所有观察者对象，使它们能够自动更新自己。

### java.util.Observable 的坑
很容易看到 JDK 中的 Observable 是一个类，不是一个接口，甚至没有实现任何一个接口。正因为这样，Observable 子类化的过程，不允许添加 Observable 的行为到 superclass，这就限制了重用这些类的可能。

另外应为没有 Observable 接口，那么就不能有自己的实现来和 Java 内置的 Observer 接口交互。

再次，看 Observable 类中的 setChanged() 方法，是 protected，这意味着你只能子类化 Observable，而不能创建一个 Observable 的实例然后将其放到新的对象中。

所以总结来说就是，如果能够继承 Observable 那就用 JDK 提供的额方式，如果想要更多的扩展性那就用自己的方式实现观察者模式，而不要使用 JDK 的方法。


## 迭代器模式 {#iterator}

迭代器模式又叫游标 (Cursor) 模式，是对象的行为模式。迭代器模式可以顺序地访问一个聚集中的元素而不必暴露聚集的内部表象（internal representation）。


