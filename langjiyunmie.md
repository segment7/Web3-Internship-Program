---
timezone: UTC+8
---

# 潘腾达

**GitHub ID:** langjiyunmie

**Telegram:** @so yun

## Self-introduction

web3新人，希望在这次共学中，能找到机会

## Notes

<!-- Content_START -->
# 2025-08-04

成功安装了常用的工具，foundry和hardhat，收集了一些常用的命令

#Hardhat

## 用hardhat创建项目

```
yarn init   初始化项目安装依赖

yarn add --dev hardhat   安装 hardhat 工具，将 hardhat 包作为开发依赖项添加到你的项目中。

yarn hardhat
```
## 测试命令

```js
yarn hardhat compile //先进行编译，方便再写测试脚本的时候，hardhat可以直接找到目标文件

yarn hardhat  //加载下载的所有插件，查看使用

yarn hardhat run scripts/deploy.js    运行测试脚本

yarn hardhat run scripts/deploy.js --network hardhat  这里指定的是 hardhat本地网络，可以切换
								//如果没有添加任何信息，默认  defaultNetwork: "hardhat"
yarn add --dev @nomiclabs/hardhat-etherscan   //安装代码验证插件

```
#Foundry

## foundry安装

安装foundry框架推荐使用 Ubuntu系统，用虚拟机下载其iso文件即可，Windows系统会出很多问题，如果坚持要用Windows系统，可以下载wls2WSL2 (Windows Subsystem for Linux 2）

安装foundry的时候，如果存在访问不到情况，可能是要访问外网，操作可以参照这篇文章     https://blog.xzr.moe/archives/124/

之后使用该命令下载foundry

```
curl -L https://foundry.paradigm.xyz | bash
```

再通过这个命令来运行

```
foundryup
```

如果存在环境问题，在Ubuntu系统里面可以非常方便，个人之前在Windows系统安装非常麻烦，会出现各种bug，还有文件权限之类的问题，而且无法使用上面这个方便命令，好像是需要git才行。

第一步：编译

创建项目工程，如果是创建初始项目的话

```
forge init <work name>
```

init命令会创建项目目录，同时安装好 forge-std 库

或者手动安装库

```
forge install forge/forge-std
```

这里会遇到一个问题，就是在安装forge-std 库这一步，会让你设置好github的用户身份认证，用一下两个命令，信息填写就是你GitHub用户信息


## 使用命令

编译合约

```
forge compile
```

显示虚拟用户

```
anvil
```



## 识别测试脚本

```solidity
import {Script, console} from "forge-std/Script.sol"

CounterScript is Script  //合约继承 Script 
```

需要导入这个文件。因为与hardhat不同，脚本语言使用的是solidity。

# 2025.07.31


<!-- Content_END -->
