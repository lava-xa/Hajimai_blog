---
title: ios_with_tg
published: 2026-07-03
description: '解决ios下telegram消息无法正常推送的问题'
image: ''
tags: []
category: ''
draft: false 
lang: ''
---

ios内telegram的推送已经许久没有恢复，此文介绍一种常用的解决telegram无法正常通知的方法

![tg](./ios_with_tg/Logo-256.svg)

### 在代理软件内导入配置

- 此处建议使用小火箭，并将其导入到模块。
- [配置文件](/file/apple_push.module)
### 包含apn流量

导入完后，如果没有正常通知，在设置--> 隧道 内打开包含所有网络，并打开包含apns,如何开关飞行模式


