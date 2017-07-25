
---
title: 以太坊（四）：以太坊的账户类型和交易
date: 2017-01-04 21:57:12
tags: 
  - Ethereum
  - geth
category: Ethereum
---


## 账户类型和交易

在 Ethereum 中有两种类型的账户：

 * 常规账户或者叫外部控制账户（我们前面用命令创建的账户）
 * 合约账户（如代码段，一个类）

这两种账户它们都有自己的以太币余额。

这两种账户都可以发起交易，虽然合约账户仅仅只能响应他们收到的其他交易。因此，以太坊区块链中的所有操作都是通过外部账号发起交易来执行的。
<!--more-->
## 以太币交易
假设你使用来作为发送者的账户中的资金是有效的，你就可以使用它来进行转账。转账操作非常简单，使用下面的一条命令就可以向目标账户转账一些以太币：

    eth.sendTransaction({from: '0x036a03fc47084741f83938296a1c8ef67f6e34fa', to: '0xa8ade7feab1ece71446bed25fa0cf6745c19c3d5', value: web3.toWei(1, "ether")})
    
## 写智能合约

智能合约以一个特殊的二进制形式（以太坊虚拟机）运行在区块链上，当前合约一般以solidity语言编写，当然也有一些其他的语言可以编写智能合约，例如[serpent][1] 和 [LLL][2].

solidity学习请参考[官方文档][3].

## 编译合约

编译合约，有几种不同的方式：
第一种，geth支持通过系统调用solc命令来对合约进行编译，详情可查看[solc][4]
第二种，可以通过浏览器的方式来进行实时编译。
第三种，在控制台下我们也可以对合约进行编译。

首先，我们需要通过`eth.getCompilers()`命令确认当前使用的编译器类型。也可以通过以下方式来确认当前是否可以编译solidity编写的智能合约：

    > eth.compile.solidity("")
    eth_compileSolidity method not available: solc (solidity compiler) not found
        at InvalidResponse (<anonymous>:-57465:-25)
        at send (<anonymous>:-115373:-25)
        at solidity (<anonymous>:-104109:-25)
        at <anonymous>:1:1
    
以上说明没有找到solidity编译环境，此时我们需要先安装好solc，并且通过以下方式将solc放入我们自定义的路径下：

    geth --datadir ~/frontier/00 --solc /usr/local/bin/solc --natspec
    
当前也可以在控制台中进行设置：

    > admin.setSolc("/usr/local/bin/solc")
    solc v0.9.32
    Solidity Compiler: /usr/local/bin/solc
    Christian <c@ethdev.com> and Lefteris <lefteris@ethdev.com> (c) 2014-2015
    true
    
然后，我们就可以编译智能合约啦。

下面是一个编译的例子，我们先写一个简单的合约，将它赋值给source变量：

    > source = "contract test { function multiply(uint a) returns(uint d) { return a * 7; } }"
    
这个合约提供一个方法，给一个整数`a`，然后会返回我们 `a*7`的值。

之后我们可以使用`eth.compile.solidity`来将合约编译：

    > contract = eth.compile.solidity(source).test
    {
      code: '605280600c6000396000f3006000357c010000000000000000000000000000000000000000000000000000000090048063c6888fa114602e57005b60376004356041565b8060005260206000f35b6000600782029050604d565b91905056',
      info: {
        language: 'Solidity',
        languageVersion: '0',
        compilerVersion: '0.9.13',
        abiDefinition: [{
          constant: false,
          inputs: [{
            name: 'a',
            type: 'uint256'
          } ],
          name: 'multiply',
          outputs: [{
            name: 'd',
            type: 'uint256'
          } ],
          type: 'function'
        } ],
        userDoc: {
          methods: {
          }
        },
        developerDoc: {
          methods: {
          }
        },
        source: 'contract test { function multiply(uint a) returns(uint d) { return a * 7; } }'
      }
    }
    
下面是如何通过jsonrpc接口调用的方式来编译合约：

    ./geth --datadir ~/eth/ --loglevel 6 --logtostderr=true --rpc --rpcport 8100 --rpccorsdomain '*' --mine console  2>> ~/eth/eth.log
    curl -X POST --data '{"jsonrpc":"2.0","method":"eth_compileSolidity","params":["contract test { function multiply(uint a) returns(uint d) { return a * 7; } }"],"id":1}' http://127.0.0.1:8100

合约编译到此结束啦！

## 创建和部署合约

现在假设你已经拥有了一个已经解锁了的合约账户和一些资金，你就可以通过发送交易给一个空地址来在区块链上创建一个合约。

    primaryAddress = eth.accounts[0]
    MyContract = eth.contract(abi);
    contact = MyContract.new(arg1, arg2, ...,{from: primaryAddress, data: evmCode})
    
`arg1`,`arg2`是合约构造时的参数，可以是任意多个。

异步方式可以通过下面这样：

    MyContract.new([arg1, arg2, ...,]{from: primaryAccount, data: evmCode}, function(err, contract) {
      if (!err && contract.address)
        console.log(contract.address); 
    });
    


[1]:https://github.com/ethereum/wiki/wiki/Serpent
[2]:https://github.com/ethereum/cpp-ethereum/wiki/LLL
[3]:http://solidity.readthedocs.io/en/latest/
[4]:https://github.com/ethereum/cpp-ethereum/tree/develop/solc