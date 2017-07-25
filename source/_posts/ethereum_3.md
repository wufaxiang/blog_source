
---
title: 以太坊（三）：挖矿
date: 2017-01-02 23:49:12
tags: 
  - Ethereum
  - geth
category: Ethereum
---

在以太坊中挖矿可以有两种方式，CPU挖矿和GPU挖矿。
在以太坊的第一版本 Frontier 中，如果你想要进行挖矿，你需要一个GPU一个以太坊客户端比如Geth，但是CPU挖矿的效率不是很好。

Geth客户端仅仅包含一个CPU矿工分支，虽然有团队在测试GPU矿工分支，但它不属于 Frontier 的一部分了。

而 Ethereum 的 C++ 实现同时提供 GPU 矿工分支， 它们都是其的一部分。
<!--more-->
## CPU 挖矿

注意：如果你想在 Ethereum 公有链上进行挖矿，你必须先完全将区块同步之后，否在你不能在主链上进行挖矿。

当你用`geth`命令启动 Ethereum 客户端后， 它默认是没有开启挖矿的，如果你想开始进行挖矿，你则需要带上 `--mine` 命令来启动挖矿，`-minerthreads`参数可以用来配置你希望同时开启几个平行线程来进行挖矿。例如：

    geth --mine --minerthreads=4
    
表示你希望开启4个平行线程来进行挖矿。

当然，你也可以在控制台中对挖矿操作进行控制，`miner.start`的参数可以配置你需要开启的平行线程数量。

    > miner.start(8)
    true
    > miner.stop()
    true
    
注意：只有当你与主链同步，你通过挖矿获取的 Ether 才会有意义。

如果你想靠挖矿赚取 Ether ， 你必须拥有你自己的 etherbase（或者 coinbase）的地址集合， 它默认为你的主要账户。如果你没有 etherbase 地址，`geth --mine`是不会启动挖矿的。

你可以通过一下的命令行来设置你的 etherbase 地址：

    geth --etherbase 1 --mine  2>> geth.log // 1 is index: second account by creation order OR
    geth --etherbase '0xa4d8e9cae4d04b093aac82e6cd355b6b963fb7ff' --mine 2>> geth.log
    
在控制台中，你可以通过以下命令来设置你的 etherbase 地址：

    miner.setEtherbase(eth.accounts[2])
    
通过`miner.hashrate`， 你可以查看你当前的 hashrate ，单位是 H/s （每s进行多少次hash运算）

    > miner.hashrate
    712000

在你成功挖到了一些区块之后，你可以查看你的 etherbase 的余额了，假设你当前的 etherbase 地址是本地地址，你可以通过以下命令来进行查看：

    > eth.getBalance(eth.coinbase).toNumber();
    '34698870000000' 
    
在账户有了余额之后，我们就想进行转账来消费一些钱，但是转账之前，我们先要解锁账户：

    > personal.unlockAccount(eth.coinbase)
    Password
    true
当返回 true 之后就说明这个帐号已经解锁了。

转账相关我们下一张再详细解释。

## GPU 挖矿

我们上文主要讲了 CPU 挖矿的操作，对于 GPU 挖矿，所需环境不太相同，因此此不具体详谈，想了解的请点 [这里][1] 

[1]: https://github.com/ethereum/go-ethereum/wiki/Mining

**参看资料**：https://github.com/ethereum/go-ethereum/wiki/Mining



