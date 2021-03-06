+++
categories = ["博文"]
date = "2016-05-20T00:00:00+08:00"
description = ""
isCJKLanguage = true
tags = ["elixir"]
title = "JavaScript 程序员眼中的 Elixir：进程"
draft = true
+++

身为 JavaScript 程序员的时候，有一些编程概念感觉距离我们十分遥远，比如说：并行／并发，多进程／线程，等等。这些概念我们多多少少都了解一些，但是在平时写代码的时候却几乎接触不到。JavaScript 是单线程的，它也有并发编程模型但却和进程／线程无关，而是基于事件循环（Event Loop）的编程模型。Elixir 则很不同，它是基于虚拟机进程（VM Process）的并发编程模型。在正式开始学习 Elixir 之前，我最大的期待就在这方面：写一些从前从未写过的东西。

但这并不意味着同样的功能无法用 JavaScript 实现，只不过是可利用的资源不同，于是实现的思路不同、语法不同、底层的实现原理也不同罢了。这里面也不存在绝对的高下之分，对于 JavaScript 越了解就越觉得 JavaScript 发展到今天这个样子其实是很有道理的，你用 Elixir 来做 UI 编程就不太合适，反之生硬的把 JavaScript 当作是纯函数式语言来使用也是很不明智的举措。这方面其实是很有说头的，我希望以后能开个单篇来详细阐述。

那么这篇还是要专注在 Elixir 上，来领略一下它的并发编程模型中最重要的概念：进程。

## 此进程非彼进程

说到进程，通常我们都是指操作系统进程（OS Processes），然而 Elixir 的进程却不是的，它的进程是由其所依赖的 Erlang 虚拟机创建管理的。可能从某种角度（以我现在的水准很难说清楚是哪种）来说它更接近于其它编程语言中的线程（Thread）概念，但是却有一个非常重要的区别：非共享内存，使得 Erlang 不将其称之为线程以免误导初学者。

这些事情对于纯粹的 JavaScript 程序员来说会比较难以理解，我尝试尽可能简单而准确的解释一些（但也只是皮毛）概念以便于理解它们的意义何在。

当你打开一个应用程序，从普通用户的角度来说就是打开这个应用程序，而从程序员的角度来说则是运行了一段代码，再到计算机的角度那就是开启了一个进程。开启一个进程意味着为此规划出一段内存地址供其所用，代码的作用就是阐述如何使用这些内存资源。

如果这个程序要同时做多件事情，那么可以创建多个子进程来各负其责；那些共同的状态可以保存在主进程之中共享访问，而各自独立的状态则分属对应的子进程。如果系统中有多个核心的处理器（多核 CPU）便可以并行计算；如果只有单核，也可以由 CPU 进行调度来分配计算资源。

计算资源的分配调度（也就是进程之间的切换）其消耗是比较大的，所以我们也可以在一个进程中虚拟多个线程来达到并行的目的。但是线程只能共享进程的资源，所以对于独立的状态管理就会比较困难（因此多线程编程需要有“锁”等机制来处理资源竟态等问题）。

简单地说，多进程的编程模型相对简单，状态管理相对容易，但资源消耗比较大（俗话说的比较吃硬件）；多线程则相对消耗资源较小，可是状态管理困难，编程模型也相应变得复杂。这是一个很典型的“鱼与熊掌”的故事。

Erlang 虚拟机的进程是兼具两方面的优点的，首先它很轻量，资源消耗与线程相当。其次它是非共享内存的，编程模型十分简单，状态管理也非常容易。此时此刻先请不要问我为什么，等我成为 Erlang 虚拟机专家的时候我会分享我的所学所知的。😸

总之呢，因为 Erlang 虚拟机进程所具备的特点，所以在 Elixir／Erlang 中使用成千上万的进程来同时进行运算处理是很平常的事情，这就是为什么它十分适合高并发编程的缘故。
