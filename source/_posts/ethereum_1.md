
---
title: 以太坊（一）：以太坊安装
date: 2017-01-02 20:21:12
tags: 
  - Ethereum
  - geth
category: Ethereum
---

以太坊有各种的客户端，go，c++等等，本文使用的是go语言客户端go-ethereum，它也是目前使用最广泛的以太坊客户端。

## linux平台
以本人使用的 ubuntu16.04 LST 版本为例，其他的请查看[这里][1]

依次运行一下命令可完成go客户端geth的安装：

    sudo apt-get install software-properties-common
    sudo add-apt-repository -y ppa:ethereum/ethereum
    sudo apt-get update
    sudo apt-get install ethereum
<!--more-->
## Mac平台
首先需要按转Homebrew来进行安装
```
/usr/bin/ruby -e "$(curl -fsSL     https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
安装后Homebrew后，用以下命令即可完成安装

    brew tap ethereum/ethereum
    brew install ethereum
    
想获取更多安装信息请查看[这里][1]

[1]: https://github.com/ethereum/go-ilding-Ethereum
