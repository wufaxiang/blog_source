title: solidity学习之深入理解solidity--合约结构
category: solidity
tags: solidity
date: 2016-12-03 14:36:12
---

solidity合约与类对象非常的相似，每个合约可以包含（ 状态变量, 函数, 函数修饰符, 时间, 结构体类型和枚举类型）的定义。

## 状态变量
状态变量是合约存储器中永久储存的值。
```
pragma solidity ^0.4.0;

contract SimpleStorage {
    uint storedData; // State variable
    // ...
}
```
<!--more-->
## 函数
函数是一个合约中的代码执行单元
```
pragma solidity ^0.4.0;

contract SimpleAuction {
    function bid() payable { // 函数
        // ...
    }
}
```
函数调用可以在内部或者外部发生，并且对于其他合约有不同的可见性。

## 函数修饰符
函数修饰符可以在声明的方式中补充函数的语义
```
pragma solidity ^0.4.0;

contract Purchase {
    address public seller;

    modifier onlySeller() { // 修饰
        if (msg.sender != seller) throw;
        _;
    }

    function abort() onlySeller { // Modifier usage
        // ...
    }
}
```

## 事件
事件是和EVM（以太虚拟机）日志设施的方便的接口
```
pragma solidity ^0.4.0;

contract SimpleAuction {
    event HighestBidIncreased(address bidder, uint amount); // 事件

    function bid() payable {
        // ...
        HighestBidIncreased(msg.sender, msg.value); // 触发事件
    }
}
```

## 结构体类型
结构是一组用户定义的变量
```
pragma solidity ^0.4.0;

contract Ballot {
    struct Voter { // Struct
        uint weight;
        bool voted;
        address delegate;
        uint vote;
    }
}
```
## 枚举类型
枚举是用来创建一个特定值的集合的类型
```
pragma solidity ^0.4.0;

contract Purchase {
    enum State { Created, Locked, Inactive } // Enum
}
```




