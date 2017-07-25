title: solidity学习之深入理解solidity--solidity源文件布局
category: solidity
tags: solidity
date: 2016-12-03 14:05:12
---

源文件可以包含多个合约的定义，包括指令和标识指令。

## 版本标识
源文件可以通过所谓的版本标识进行注释，以此来拒绝将来可能引起不兼容的编译版本。我们试图将这种变化保持在最小值，尤其是在句法中语义的改变，当然这不是总会发生的。因此，对于包含中断改变（breaking changes）的版本，读版本变化日志总是好的，这些版本将以`0.x.0 `或者 `x.0.0.`这样的形式。
<!--more-->
版本标识如下面这样使用：
```
pragma solidity ^0.4.0;
```
这样一个源文件将不会被比0.4.0之前的编译器编译，同时它也不会在0.5.0的编译器上生效。这个是在0.5.0之前不会有breaking changes，所以我们总是能确定我们的代码以我们期望的方式编译。我们不会确切的修复编译器的版本，所以bugfix版本仍然是可能的。

可以为编译版本指定更复杂的规则，表达式遵循npm使用。

## 导入其他源文件
### 语义和句法
solidity与JavaScript（从ES6开始）中可用的import语句非常相似，尽管solidity不知道`default export`的概念。

在全局级别，你可以以下面的形式使用import语句：
```
import "filename";
```
这条语句将"filename"中所有的全局symbols导入到当前的全局域
```
import * as symbolName from "filename";
```
创建一个新的全局符号`symbolName`，它的成员全部来自于"filename"中的全局符号.
```
import {symbol1 as alias, symbol2} from "filename";
```
创建两个新的全局变量别名：`alias`,`symbol2`，它将分别从"filename" 引入symbol1 和 symbol2
另外，Solidity语法不是ES6的子集，但可能（使用）更便利
```
import “filename” as symbolName;
```
等价于 `import * as symbolName from “filename”`;.


### 路径
在上文中，文件名总是用/作为目录分割符，. 是当前的目录，..是父目录，路径名称不用.开头的都将视为绝对路径。

从同一个目录下import 一个文件 x 作为当前文件，用 `import ”./x” as x;` 如果使用`import “x” as x;` 是不同的文件引用（在全局中使用`"include directory"`）,。
它将依赖于编译器（见后）来解析路径。通常，目录层次不必严格限定映射到你的本地文件系统，它也可以映射到ipfs,http或git上的其他资源

### 使用真正的编译器
当编译器启动时，不仅可以定义如何找到第一个元素的路径，也可能定义前缀重映射的路径，如 `github.com/ethereum/dapp-bin/library`将重映射到 `/usr/local/dapp-bin/library `，编译器将从这个路径下读取文件。 如果重映射的keys是前缀， （编译器将尝试）最长的路径。允许回退并且映射到`/usr/local/include/solidity`。此外，这些重新映射可以取决于上下文，这允许您配置要导入的包。 例如不同版本的同名库。

**solc：**
对于solc（命令行编译器），这些重新映射被提供作为`context:prefix=target`的参数，这个地方`context:`和`=target`是可选的部分。所有重映射的常规文件都将被编译（包括他们的依赖文件）。这个机制将完全向后兼容（只要没有文件名包含 = 或者 ：）。所有文件中或者`context`目录下的import以`prefix `开始的都将重定向用`target`替换掉`prefix`.

例如，如果你克隆`github.com/ethereum/dapp-bin/`本地到`/usr/local/dapp-bin`,你可以使用下面这种在源文件中：
```
import "github.com/ethereum/dapp-bin/library/iterable_mapping.sol" as it_mapping;
```
然后运行编译器：
```
solc github.com/ethereum/dapp-bin/=/usr/local/dapp-bin/ source.sol
```

一个更复杂的例子，假设你使用了一个非常旧的版本的dapp-bin，依赖了一些模块。旧版本的dapp-bin会在`/usr/local/dapp-bin_old`检查，然后你可以使用:
```
solc module1:github.com/ethereum/dapp-bin/=/usr/local/dapp-bin/ \
     module2:github.com/ethereum/dapp-bin/=/usr/local/dapp-bin_old/ \
     source.sol
```
以至于在`module2`中的导入指向旧版本但是`module1`指向新版本。

注意solc只允许你从指定目录下include文件：他们必须是一个显式定义的，包含目录或子目录的源文件， 或者是重映射目标的目录（子目录）。如果你允许直接include，只需添加重新映射`=/`。

如果有多个重映射，就要做一个合法文件，文件中选择最长的公共前缀。

**基于浏览器的solidity：**
基于浏览器的solidity编译器提供了自动重新映射到github，并且自动检索网络上的文件，你可以import迭代映射例如：
`import "github.com/ethereum/dapp-bin/library/iterable_mapping.sol" as it_mapping;`

其他源代码提供者可以以后增加进来。

## 注释
单行注释（//）和多行注释（/*...*/）都是可用的。
```
// 这是单行注释

/*
这是多行注释
*/
```
此外，有另外一种注释叫"natspec "，他们用三行斜杠`///`或者双星号书写，它们应该直接在函数声明或语句之上使用。您可以在这些注释中使用Doxygen风格的标记来记录函数，注释形式验证的条件，并提供确认文本，当用户尝试调用函数时向用户显示。

在下面的示例中，我们记录了合约的标题，两个输入参数的说明和两个返回值。

```
pragma solidity ^0.4.0;

/** @title Shape calculator.*/
contract shapeCalculator{
    /**@dev Calculates a rectangle's surface and perimeter.
     * @param w Width of the rectangle.
     * @param h Height of the rectangle.
     * @return s The calculated surface.
     * @return p The calculated perimeter.
     */
    function rectangle(uint w, uint h) returns (uint s, uint p) {
        s = w * h;
        p = 2 * (w + h);
    }
}
```




