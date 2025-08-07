---
timezone: UTC+8
---

# Tommy

**GitHub ID:** tommyjqliu

**Telegram:** @tommyjqliu

## Self-introduction

你好，我本科毕业于华南理工大学软件工程，现在于澳洲修读软件工程硕士。之前主要接触web2现在希望转型为web3开发。

## Notes

<!-- Content_START -->
# 2025-08-07

# 虚拟货币相关法律风险与合规要点

## 一、常见涉罪罪名
- **集资诈骗罪**（如光锥币、大圣币等ICO案例）
- 非法吸收公众存款罪
- 组织、领导传销活动罪
- 开设赌场罪
- 非法经营罪

## 二、核心红线：虚拟货币发行与募资
- **ICO（Initial Coin Offering）**  
  ▶︎ 国内明确禁止虚拟货币发行融资  
  ▶︎ 海外持牌成本高且合规复杂  

## 三、关键风险识别要点
### （一）资金汇集场景
1. 交易所与虚拟货币基金运作模式  
2. 是否涉及法币兑换（人民币出入金）  
3. 海外合规性审查：  
   - AML（反洗钱）与KYC（用户识别）制度是否完备  
   - 技术层面是否屏蔽中国大陆用户  

### （二）传销与赌博风险
- **项目扩张合规性**：  
  ▶︎ 避免多级返佣、团队计酬等传销特征  
  ▶︎ 非典型风险案例：合约交易、NFT玩法设计  

### （三）洗钱与外汇管制
- 潜在风险角色：  
  ▶︎ 成为洗钱链条环节  
  ▶︎ 充当外汇兑换媒介  
- **难点**：非管理层难以追踪资金路径  

## 四、合规判断经验准则
1. **主体资质**：  
   - 是否在受监管司法辖区注册  
   - 是否通过第三方审计/安全测试  
2. **风控措施**：  
   - 完备的KYC/AML制度  
   - 公开团队背景、资金路径等信息  
3. **业务模式**：  
   - 是否存在高风险模块（如杠杆、赌博机制）  
   - 是否采用层级返利等高风险扩张方式  

## 五、责任层级与法律盲区
### 责任等级（由高到低）：
1. 核心管理层  
2. 宣传/社区负责人  
3. 业务核心环节参与者  
4. 纯技术提供者  

### 知情推定原则：
- 技术提供者需主动尽调，**"明知可疑而不询问"可能被推定知情**  
- 免责前提：已尽合理防范义务

# 2025-08-06

# EOS的失败

1. **过度中心化**：21个超级节点被交易所和资本垄断，贿选、互投成风，违背区块链去中心化原则。
2. **经济模型崩盘**：资源租赁（RAM/CPU）复杂昂贵，高通胀+抛压致币价暴跌95%，生态资金滥用。
3. **性能虚假宣传**：号称百万TPS，实际仅数千且依赖节点协作，拥堵时用户体验极差。
4. **开发环境恶劣**：C++合约门槛高，工具链混乱，开发者流失。
5. **生态泡沫化**：依赖博彩DApp，无可持续应用；社区基金腐败，优质项目枯竭。
6. **团队失信**：Block.one募资41亿美元后消极开发，高管离职，社区分裂。

# Q

去中心化和高性能，应该优先选择谁？
公链的不同治理模型有哪些，对比他们的异同？
应该优先激励哪些生态用户（用户，开发者）？
公链如何管理资金来获取用户信任？

### **共识算法对比总结**

### **PoW（工作量证明）**

- **代表项目**：比特币、莱特币
- **工作原理**：矿工竞争算力解题，最快解出者出块
- **安全性**：极高（51%算力攻击成本高）
- **去中心化**：高（但矿池集中化问题）
- **TPS**：3-7（比特币）
- **能耗**：极高
- **适用场景**：价值存储、高安全性区块链

### **PoS（权益证明）**

- **代表项目**：以太坊2.0、Cardano
- **工作原理**：按持币量和时间随机选择验证者
- **安全性**：高（51%持币攻击成本高）
- **去中心化**：中（富者愈富效应）
- **TPS**：100-1,000+
- **能耗**：极低
- **适用场景**：智能合约平台、DeFi

### **DPoS（委托权益证明）**

- **代表项目**：EOS、TRON
- **工作原理**：持币者投票选超级节点，节点轮流出块
- **安全性**：中（依赖代表诚信）
- **去中心化**：低（节点寡头化）
- **TPS**：1,000-4,000+
- **能耗**：极低
- **适用场景**：高性能DApp、企业链

### **PBFT（实用拜占庭容错）**

- **代表项目**：Hyperledger Fabric
- **工作原理**：节点轮流提案，需2/3以上投票通过
- **安全性**：高（可容错1/3恶意节点）
- **去中心化**：低（需许可链）
- **TPS**：1,000-10,000
- **能耗**：低
- **适用场景**：联盟链、金融机构

### **Tendermint（BFT+PoS）**

- **代表项目**：Cosmos
- **工作原理**：PoS选出验证者，BFT共识快速确认
- **安全性**：高（即时最终性）
- **去中心化**：中（验证者选举制）
- **TPS**：1,000-10,000
- **能耗**：极低
- **适用场景**：跨链枢纽、需快速确认的公链

### **PoH（历史证明）**

- **代表项目**：Solana
- **工作原理**：时间戳序列化交易，减少节点同步时间
- **安全性**：中（依赖时钟同步）
- **去中心化**：中（验证者集中化）
- **TPS**：50,000+
- **能耗**：低
- **适用场景**：高频交易、Web3应用

### **总结特点**

- **最高安全性**：PoW > PoS/Tendermint/PBFT > DPoS > PoH
- **最高去中心化**：PoW > PoS > Tendermint > PoH > DPoS > PBFT
- **最高TPS**：PoH > DPoS > PBFT/Tendermint > PoS > PoW
- **最低能耗**：PoS/DPoS/Tendermint/PoH > PBFT > PoW

# 2025-08-05

# Token Approval

Check token approvals to revoke unwanted one

[Token Approvals | Etherscan](https://etherscan.io/tokenapprovalchecker) 

[Token Approvals | Arbitrum One](https://arbiscan.io/tokenapprovalchecker?search=0xafd35749459860f490325858cd7b3ad3606b07cc)

## Special cases

Permit2 (used by Uniswap) manages a internal approval table, and the approvals on it can not be revoke through normal scan.

# Sign Contract

• Always verify the actual function call and parameters of transactions

# 2025-08-04

# Wallet Security Best Practices
## 1. Protect Your Recovery Phrase
Never leak or share your recovery phrase (seed phrase).

Prevent copying to clipboard to avoid accidental exposure.

Never provide it to anyone, even if they claim to be support.

## 2. Token Approvals & DApps
Never approve tokens for unknown or untrusted DApps.

Revoke unused approvals periodically using tools like Etherscan or Revoke.cash.

## 3. Always Double-Check
Recipient address – Verify before sending.

Token address – Confirm legitimacy (avoid fake tokens).

Website URL – Ensure it’s the official site (check for typos or HTTPS).


# 2025.08.01


<!-- Content_END -->
