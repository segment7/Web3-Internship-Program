---
timezone: UTC+8
---

# Neal Lee

**GitHub ID:** snaildarter

**Telegram:** @Neal_li

## Self-introduction

学习，使用，重复，反思，升华。

## Notes

<!-- Content_START -->
# 2025-08-04

## Dapp 架构

### 前端

界面 React/Vue
数据管理 Zustand/Redux
工具库 Typescript、Tailwind
了解钱包的库：Viem、wagmi、Rainbow

### 智能合约

solidity： hardhat

### 数据检索器

智能合约通常以 Event 形式释放日志事件，比如释放代表 NFT 转移的 Transfer 事件，数据检索器会检索这些数据并将其写入到 PostgreSql 等传统数据库中。
Dapp 在前端进行数据展示时需要检索器内的数据。一个简单的示例是某个 NFT 项目需要展示用户持有的所有 NFT，但是 NFT 合约并不会提供通过输入地址参数返回该地址下的所有 NFT 的函数，此时我们可以运行检索器将 Transfer 事件读取后写入传统数据库内，前端可以在传统数据库内检索用户持有的 NFT 数据。

### 区块链和去中心化存储

区块链用于存储智能合约的状态数据以及交易记录。去中心化存储如 IPFS 或 Arweave，用于存储大规模的非结构化数据（如图片、文档等），确保数据不易丢失和篡改。
通过使用去中心化存储，Dapp 确保所有数据在多个节点上备份，保证数据的持久性和去中心化的特性。

### 常见优化技巧

#### 减少存储操作

- 读取存储第一次需 2100 gas（后续 100 gas），而内存读取仅 3 gas。推荐多次访问同一存储数据时，将其缓存到内存以减少 SLOAD 次数
- 每次写入 storage 的成本高达 20,000 gas；优先使用 memory。
- 示例：

```solidity
// ❌ 非优化写法
mapping(address => uint256) public balances;
function deposit() public payable {
    balances[msg.sender] += msg.value;
}

// ✅ 优化写法（一次读，一次写）
function deposit() public payable {
    uint256 current = balances[msg.sender];
    balances[msg.sender] = current + msg.value;
}
```

#### 使用位压缩（Bit Packing）

- 将多个变量压缩到一个 uint256 中以节省存储空间。
- 示例：

```solidity
struct Packed {
    uint128 a;
    uint128 b;
}
```

#### 循环优化

- 减少不必要的运算，如 array.length 缓存到变量中。
- 示例

```solidity
// ❌ 非优化
for (uint256 i = 0; i < arr.length; i++) {
    ...
}
// ✅ 优化
uint256 len = arr.length;
for (uint i = 0; i < len; ++i) {
    ...
}
```

#### 函数可见性选择

external 比 public 更节省 gas，适用于仅背外部调用的函数。


# 2025.07.29


<!-- Content_END -->
