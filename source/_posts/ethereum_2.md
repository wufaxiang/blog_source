
---
title: 以太坊（二）：账户管理
date: 2017-01-02 21:40:12
tags: 
  - Ethereum
  - geth
category: Ethereum
---

## 1. 概述

Geth客户端的账户管理是通过一下命令行来对账户进行管理的：

    account command [arguments...]

通过以上的命令行形式，可以创建新的账户，查看所有账户，给账户分配一个私钥，更新最新的密钥形式和更改账户密码这些操作。
<!--more-->
密钥存储在`<DATADIR>/keystore`文件中，所以建议最好备份一下这个文件，当前最新的密钥的形式是`UTC--<created_at UTC ISO8601>-<address hex>`，列表中的账户列出来时是按照字典序来进行排列的，但它们实际上是按照时间戳的顺序也就是创建的顺序排列的。

常用命令：

        list    输出所有账户的地址
        new     创建一个新的账户
        update  更新已经存在的账户
        import  给账户导入私钥
        
你也可以通过一下命令行来或许详细的信息：

    geth account help <subcommand>


警告：使用ethereum很重要的一点是要记住密码，如果丢失了你的密码，你就没办法访问你的账户了，包括里面的以太币。所以，不要忘了自己账户的密码。
    
## 2. 实例

### 创建账户

在go-ethereum客户端中创建新的以太坊有很多中方法，请听我慢慢谈来。

第一种，在终端下使用geth命令：

    geth account new
    
使用这个简单的命令，就可以创建一个新的账户啦，它会提示你输入两遍密码，输入成功后会返回你创建的这个帐号的地址，记住你的密码欧，后面需要它来进行解锁。

第二种，非交互式方式

    geth --password <passwordfile> account new
    
你可以将密码放在一个文本文件中，然后通过上面的命令在终端中执行，同样也可以创建一个新的账户，这次它不会提示你再输入密码了，因为你已经将密码放在文本文件中了。但是，这样也就暴露了你的密码了哟。

第三种，控制台下创建账户

    > personal.newAccount("passphrase")
    
在控制台，你可以进行新帐户的创建，`passphrase`是你要创建帐号的密码，记住哟，解锁的时候需要的。

第四种，通过导入一个私钥来创建一个新帐户

    geth --password <passwordfile> account import <keyfile>
同时，你还可以通过使用以上命令导入存放了私钥的文本文件来创建一个新的账户，这也是一种非交互式的方式。

### 更新账户

更新账户也有两种形式：

    geth account update b0047c606f3af7392e073ed13253f8f4710b08b6
    geth account update 2
你可以在`update`后面跟上账户的地址或者序号来对指定的账户进行密码和密钥形式的更新。


### 导入预售钱包

导入预售钱包非常简单，当然前提是你必须要知道你的密码欧

    geth wallet import /path/to/my/presale.wallet
    
使用上面的命令就可以进行导入了，它也可以通过使用`--password`参数来进行非交互式的导入

### 列出所有账户

我们创建账户之后，那么我们如何来查看我们创建的帐号呢？其实很简单，
在终端下，输入以下命令：

    $ geth account list
    Account #0: {d1ade25ccd3d550a7eb532ac759cac7be09c2719}
    Account #1: {da65665fc30803cb1fb7e6d86691e20b1826dee0}
    Account #2: {e470b1a7d2c9c5c6f03bbaa8fa20db6d404a0c32}
    Account #3: {f4dd5c3794f1fd0cdc0327a83aa472609c806e99}
    
就可以列出你创建的所有账户地址了，需要注意的是，其排列顺序在不同节点不是唯一的。

在控制台中也可以列出所有创建的账户：

    > eth.accounts
    ['0x407d73d8a49eeb85d32cf465507dd71d507100c1']
    
还有一种方式也可以列出创建的所有账户，使用RPC：

    # Request
    $ curl -X POST --data '{"jsonrpc":"2.0","method":"eth_accounts","params":[],"id":1} http://127.0.0.1:8545'
    
    # Result
    {
      "id":1,
      "jsonrpc": "2.0",
      "result": ["0x407d73d8a49eeb85d32cf465507dd71d507100c1"]
    }
    
我们在编程时，经常需要使用rpc请求方式来进行命令的操作。

### 解锁帐号

我们在使用帐号进行转账等操作前，都需要先对帐号解锁，才能继续后面的操作。

    geth --password <(echo this is not secret!) --unlock primary --rpccorsdomain localhost --verbosity 6 2>> geth.log 
    
除了帐号的地址，你也可以使用序号来代表你需要解锁的帐号，而希望解锁多个帐号，则可以按以下方式：

    geth --unlock "0x407d73d8a49eeb85d32cf465507dd71d507100c1 0 5 e470b1a7d2c9c5c6f03bbaa8fa20db6d404a0c32"
    
注意：如果使用非交互式的方式，那么文件中每个密码占一行，不能全部在一行。

在控制台下，也可以进行帐号的解锁：
    
    personal.unlockAccount(address, "password")
    
### 查询账户余额

在控制台下，可以使用以下命令来查询某个账户的以太币余额：
    
    > web3.fromWei(eth.getBalance(eth.coinbase), "ether")

同时，你可以使用一个js函数来查询显示所有账户的余额：
    
```javascript
function checkAllBalances() {
    var totalBal = 0;
    for (var acctNum in eth.accounts) {
        var acct = eth.accounts[acctNum];
        var acctBal = web3.fromWei(eth.getBalance(acct), "ether");
        totalBal += parseFloat(acctBal);
        console.log("  eth.accounts[" + acctNum + "]: \t" + acct + " \tbalance: " + acctBal + " ether");
    }
    console.log("  Total balance: " + totalBal + " ether");
};
```
然后执行可以在控制台下执行js函数来显示所有账户余额：

    > checkAllBalances();
    eth.accounts[0]: 0xd1ade25ccd3d550a7eb532ac759cac7be09c2719   balance: 63.11848 ether
    eth.accounts[1]: 0xda65665fc30803cb1fb7e6d86691e20b1826dee0   balance: 0 ether
    eth.accounts[2]: 0xe470b1a7d2c9c5c6f03bbaa8fa20db6d404a0c32   balance: 1 ether
    eth.accounts[3]: 0xf4dd5c3794f1fd0cdc0327a83aa472609c806e99   balance: 6 ether

每当你重新启动geth控制台后，其js函数就被清空了，如果你希望一直都能使用，你可以将js函数存在本地，然后使用`loadScript`函数直接调用js文件

    > loadScript("/Users/username/gethload.js")
    true



