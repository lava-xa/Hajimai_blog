---
title: 好用的一些组网类的工具
published: 2026-05-10
description: ''
image: './newwlan/win10.jpg'
tags: []
category: '好用的工具'
draft: false 
lang: ''
---

# 组网工具

本文将着重推荐有这些特点的

- 去中心化程度高---这是大势所趋
- 使用方便，有客户端图形化界面
- 能够跨平台使用

> [!WARNING]
> 本文仅仅用作技术交流，请勿用于违法犯罪行为，一切造成的法律责任与本人无关

> [!WARNING]
> 本人坚定维护共产党领导，坚持一个中国原则，坚定打击诈骗黑产违法犯罪行为



## Easytier

一个非常好的开源项目，主要用于各种nat类型的组网

> 基于wireguard,能够做到几乎完全的去中心化，同时也可以做到全中心化

> 兼容各种主流平台，win、linux、mac、Android

> 有官方图形化界面方便操作

> 各种nat类型都可使用，成功率还行。甚至nat4也可以用



## Wireguard
![logo](newwlan/wireguard.png)

适合有公网ip的情况

> Linux 内核官方支持，被linus称之为艺术品

> 各种工具的基础，完全的去中心化

- 极高的安全性，采用各种现代化加密方式
- 仅有几千行核心代码
- 极简的官方图形化界面

## 一些frp工具

> [!WARNING]
> frp有较高风险，请先设置防火墙或鉴权再进行操作

> [!WARNING]
> 许多地方已经一刀切明确禁止frp，如校园网

如果你只是想进行mc联机，可以尝试使用樱花内网穿透（Sakura Frp）

## 局域网传输文件

经过上面组网后，理论上99%都可以形成一个虚拟局域网

### 传输此处推荐localsend

- [Localsend](https://localsend.org/)
- 兼容各种主流平台，win、linux、mac、Android,ios
- 完全开源免费
- 支持多种发送，有完整的交互鉴权

当然你也可以开启windows的局域网发现，可以直接在文件管理器内发现，非常方便

### 远程连接

- rdp：windows自带或krdc

- wol工具：openwrt插件

- ssh：tabby或termius

- vnc：krdc或vnc viewer






