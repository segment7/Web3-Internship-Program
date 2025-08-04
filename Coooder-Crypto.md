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
