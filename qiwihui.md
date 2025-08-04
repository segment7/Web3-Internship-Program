---
timezone: UTC+8
---

# Chris Qiu

**GitHub ID:** qiwihui

**Telegram:** @qiwihui

## Self-introduction

web3 开发工程师

## Notes

<!-- Content_START -->
# 2025-08-04

## Morpho 借贷协议

### 一、借贷协议基本原理

去中心化借贷协议：DeFi 最核心的金融基础设施之一。它的基本目标是将闲置资金高效地匹配给有借款需求的用户，以便供给方赚取利息、借款方获取流动性。

1. 核心角色

存款人（Supplier）：向协议提供资产，赚取利息
借款人（Borrower）：抵押资产，从协议借入资金
清算人（Liquidator）：在借款人抵押不足时执行清算，维持协议

2. 传统借贷协议架构：池化模型（Pool-based）

以 Aave、Compound 为代表，采用资金池（Lending Pool）模型：
- 所有存款被汇聚到资金池中；
- 借款人从池中借款；
- 协议设定动态利率模型，利率受资金利用率影响；
- 资金利用率低时利率低，利用率高时利率上升，鼓励更多人存入以及借款人及时取回

### 二 、 Morpho协议整体结构

![](https://upload.techflowpost.com/upload/images/20250210/2025021016443701669851.png)

1. Morpho 

Morpho 市场是单一抵押品和单一借贷品的市场。所以只有提供借贷品的 Supplier 能够获取利息，而 Borrower 提供质押品的并不会产生利息。因为 borrower提供的质押品不会被用来进行借贷。
Morpho 中Borrower交互的最小单位称之为是 Market，包含抵押资产，借贷资产，价格预言机，最小抵押率(Lltv)，清算模型(irm)。
同时 morpho 是一个无许可的协议，任何人可以创建不同的市场，只不过对应的最小抵押率，清算模型的选择只能是协议通过治理预先设置的值。
Lltv: [0%; 38.5%; 62.5%; 77.0%; 86.0%; 91.5%; 94.5%; 96.5%; 98%]
irm: AdaptiveCurveIRM

例如：
- ETH / USDT，77.0%
- ETH / USDT,  62.5%
- USDT / USDC, 96.5%

2. Vault
Supplier 交互的最小单位也是 Market. 但是为了更好提供不同的质押需求，morpho 推出了 vault。
vault：通过官方认证的人/单位来管理不同的Vault, 每个Vault会将 supplier 提供的资金分配到不同的merket 中，以达到更高的资金利用效率。

Vault 相当于 aave 中的池，但是比池更灵活。

3. 清算：

待研究


# 2025.07.29


<!-- Content_END -->
