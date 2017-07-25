title: solidity学习之安装solidity
category: solidity
tags: solidity
date: 2016-12-02 19:04:12
---


## 版本控制

　　solidity的版本控制遵循[语义化版本控制规范](http://semver.org/lang/zh-CN/)进行发布，使用旧版本也是可行的。但是旧版本有部分地方不能保证一定有效，它们可能会包含无证或者不好的变化。我们推荐使用最新版本。下面的安装包将使用最新版本。

<!--more-->
## Browser-Solidity(基于浏览器的Solidity)

　　如果你想尝试运行小型的Solidity的智能合约，你可以不需要任何安装直接使用基于浏览器的[Solidity](https://ethereum.github.io/browser-solidity)。如果你想要离线使用，可以到[github](https://github.com/ethereum/browser-solidity/tree/gh-pages)克隆或者下载.zip包。

## npm / Node.js
　　这可能是安装本地Solidity最便捷的方式。
　　在基于浏览器的Solidity上，Emscripten提供了一个跨平台JavaScript库，把C++源码编译为JavaScript，同时也提供NPM安装包。
	简单的使用以下命令即可安装：
```
npm install solc
```
　　关于Node.js的用法细节可以在[solc-jsrepository](https://github.com/ethereum/solc-js)找到.

## Docker
　　我们为编译器提供了最新的Docker构建版本。`stable`仓库包含最新的发布范本，`nightly`仓库包含的版本在开发分支中可能会包含一些潜在的不稳定改变。
```
docker run ethereum/solc:stable solc --version
```
　　目前，Docker镜像只包含编译器可执行文件，因此你必须做一些额外的工作来链接源和输出目录。

## Binary Packages

　　Solidity的二进制包可以在 [solidity/releases](https://github.com/ethereum/solidity/releases)找到。
　　对于Ubuntu最新的稳定版本，我们可以使用PPAs。
```
sudo add-apt-repository ppa:ethereum/ethereum
sudo apt-get update
sudo apt-get install solc
```
　　如果你想使用最新的另外一个版本
```
sudo add-apt-repository ppa:ethereum/ethereum
sudo add-apt-repository ppa:ethereum/ethereum-dev
sudo apt-get update
sudo apt-get install solc
```
Homebrew从Jenkins转移到了TravisCI时丢失了`pre-built bottles`， ，但是Homebrew应该仍然作为一种有效的从源码构建的一种方式。我们将在不久后添加`pre-built bottles`。

```
brew update
brew upgrade
brew tap ethereum/ethereum
brew install solidity
brew linkapps soliditybrew update
brew upgrade
brew tap ethereum/ethereum
brew install solidity
brew linkapps solidity
```

## 从源码进行构建
### 克隆仓库
执行以下命令克隆仓库：
```
git clone --recursive https://github.com/ethereum/solidity.git
cd solidity
```
如果你想向Solidity贡献，你应该`fork`Solidity并且添加你的` personal fork `。
```
cd solidity
git remote add personal git@github.com:[username]/solidity.git
```

### 依赖--macOS
　　对于macOS，确保你有安装最新版本的Xcode。这个包含了`Clang C++ compiler`,`Xcode IDE`和其他的Apple在`OS X`操作系统上构建C++的开发工具。如果你第一次安装Xcode，或者刚刚安装好一个更新的版本，然后你在做命令行构建钱需要同意license：
```
sudo xcodebuild -license accept
```
　　我们的OSX构建要求你[安装Homebrew](http://brew.sh/)包来安装额外的依赖。如果你想要擦除再次开始，这是如何[卸载Homewbrew](https://github.com/Homebrew/homebrew/blob/master/share/doc/homebrew/FAQ.md#how-do-i-uninstall-homebrew)。

### 依赖--windows
你需要为在windows上构建Solidity安装下列这些依赖。

| Software | Notes    |
| :--------: | :--------: |
| [Git for Windows](https://git-scm.com/download/win)                                  | 从github检索资源的命令行工具 |
| [CMake](https://cmake.org/download/)                                                 | 跨平台文件构造生成器         |
| [Visual Studio 2015](https://www.visualstudio.com/products/vs-2015-product-editions) | C++开发编译环境              |

### 额外依赖
　　我们现在拥有在macOS，windows和多数linux上的一键安装脚本，这个过去是一个多步的进程，但现在只需一行命令：
```
./scripts/install_deps.sh
```
在windows上：
```
scripts\install_deps.bat
```
### 命令行构建
在linux，macOS和其他unix机器上构建Solidity是非常相似的：
```
mkdir build
cd build
cmake .. && make
```
甚至在windows上：
```
mkdir build
cd build
cmake -G "Visual Studio 14 2015 Win64" ..
```
最新的指令集合会自动在构建目录创建**solidity.sln**。双击它会在 Visual Studio中打开。我们构建**RelWithDebugInfo**配置。
此外，在windows上你应该用命令行构建，像这样：
```
cmake --build . --config RelWithDebInfo
```






