---
timezone: UTC+8
---

# Coooder

**GitHub ID:** Coooder-Crypto

**Telegram:** @Coooder_Crypto

## Self-introduction

想来学习新的知识

## Notes

<!-- Content_START -->
# 2025-08-06

今天把论文看完了，第一次看到论文里的设计，感觉挺 low 的，但是后面查了一圈，web3 好像确实还没有类似的实现方案。虽然之前也看过一些 web3 的论文，但是随着学到的东西越来越多，看东西的感悟也越来越不一样。收获还挺大的，可惜文章还没有发表，有的东西不太方便放过来。

时间允许的话明天再找一些优秀的论文看一看

# 2025-08-05

### SBT：灵魂代币

不可转让的 SBT：https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4105763

今天看 DID 相关的论文，提到了 SBT 相关的内容，让我想到了两年前刚开始学习 DID 的时候就有币圈师兄问我有没有看 SBT，当时心说以后详细看下，一转眼两年了都。趁这个机会把 V 神的这个文章看了下。

### **Fiat-Shamir heuristic**

将 **交互式零知识证明协议** 转换为 **非交互式零知识证明（NIZK）** 

交互式证明：

- Prover 发送证明
- Verifier 发送挑战
- Prover 响应挑战

非交互式证明：

- 用哈希函数模拟 Verifier 的挑战 challenge = H ( commitment )

应用：

zk-SNARKs，zk-STARKs，Schnorr签名

安全性依赖于哈希函数像随机预言机

### zk-SNARKs、zk-STARKs

去年这部分看不懂一点，现在理解起来轻松多了。btw 去年还在找各种中文资料，刚刚顺着论文的 related work 就把这部分重新理了一遍。

# 2025-08-04

今天制定了下学习计划，首先基础的 web3 知识都已经比较了解了，然后最近也做完了研究生的第一个工作，后面的一个月刚好要看一些论文，打算看一些区块链相关的论文，然后把一些之前没了解过的知识整理下。
今天看了一个去中心化身份认证的文章，以下是一些学到的知识

### Pedersen Commitment

密码学承诺方案，允许对一个秘密值或者消息生成一个绑定但是隐藏的承诺，同时保持完美隐藏性（Perfect Hiding）和计算绑定性（Computation Binding）

- Perfect Hiding：即使拥有无限计算能力，也无法推断原始数据。SHA-256 不是完美隐藏的
- Computation Binding：计算可行范围内，无法找到两个不同的消息生成相同的承诺

和 ZKP 可以很好的结合：用 Pedersen Commitment 隐藏私有数据 m，生成承诺 C，然后在 ZKP 中证明，“我知道 m 和 r，是的 C=，且 m 满足某些条件”

而且 Pedersen Commitment 是加法同态的


# 2025.07.30


<!-- Content_END -->
