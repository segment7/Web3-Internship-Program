---
timezone: UTC+8
---

# kuve

**GitHub ID:** kuove

**Telegram:** @stephkuove

## Self-introduction

web2转型web3,希望学习测试与开发

## Notes

<!-- Content_START -->
# 2025-08-05

静态数组和动态数组
状态变量被永久保存在区块链中。所以在你的合约中创建动态数组来保存成结构的数据是非常有意义的。

public
private：私有函数命名用“_”开头
view函数：只读
pure函数：不访问数据
散列函数keccak256：把string转换为256位16进制数（不安全）

事件：是合约和区块链通讯的机制，前端监听事件，作出反映

映射：mapping(key => value)，通过键找值

msg.sender:当前调用者的address
注意：在 Solidity 中，功能执行始终需要从外部调用者开始。 一个合约只会在区块链上什么也不做，除非有人调用其中的函数。所以 msg.sender总是存在的。
require: 当不满足条件时抛出错误，停止执行
注：solidity不支持字符串比较，只能用keccak256函数进行哈希值比较

# 2025-08-04

web3测试方法
1.单元测试（Unit Testing）
目标：
测试智能合约中每个函数的独立功能。

方法：
编写测试用例：为每个函数编写测试用例，覆盖正常和异常情况。

使用测试框架：

Truffle：内置 Mocha 和 Chai 支持。

Hardhat：支持 Waffle 和 Ethers.js。

Foundry：支持 Solidity 原生测试。

模拟环境：使用本地区块链（如 Ganache、Hardhat Network）进行测试。


# 2025.07.29


<!-- Content_END -->
