---
description: 此页主要介绍一些关于项目和目标程序的问题。
---

# 基础知识

## 项目

一个项目应该是一个可以连接到`FuzzX`平台的自主更新的代码库（例如，一个项目可以是一个在`GitHub`平台上托管的代码仓库），加上这样一个限制条件的目的是：确保我们总是对最近提交的的代码进行`Fuzzing`。

[点击这里](https://gustrikelab.gitbook.io/fuzzx/kai-fa-wen-dang/xiang-mu)一栏来学习如何将您的代码连接到`FuzzX`。

## 目标程序

每个`FuzzX`项目都包含目标程序。目标程序是可以在你的代码上运行由`FuzzX`生成的测试用例的简单组件。每一个目标程序都遵循以下三条规则：

1. 读取测试用例
2. 运行测试用例
3. 核查错误

为了便于您的理解，也可以将目标程序想象成不需要自行定义测试数据（`FuzzX`已经提供了）的单元测试。

[点击这里](https://gustrikelab.gitbook.io/fuzzx/kai-fa-wen-dang/mu-biao-cheng-xu)可以学习如何配置您自己的目标程序。

