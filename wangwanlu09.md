---
timezone: UTC+8
---

# wanlu

**GitHub ID:** wangwanlu09

**Telegram:** @wangwanlu

## Self-introduction

我是一名初级开发，正在学习以太坊智能合约调用相关知识，比如wagmi等，相关demo已经在我的repo中，希望未来学习更多web3相关开发知识

## Notes

<!-- Content_START -->
# 2025-08-06

## 使用 Scaffold-ETH 2 部署 NFT 项目

### **今日目标**

* 掌握 Scaffold-ETH 2 快速搭建和部署 Web3 应用的流程
* 在以太坊测试网（Sepolia）部署 NFT 智能合约
* 将前端 DApp 部署到 Vercel，生成可访问的公开链接

---

### ** 核心学习内容**

#### **1. Scaffold-ETH 2 的作用**

Scaffold-ETH 2 是一个 Web3 全栈开发框架，集成了：

* **智能合约开发（Hardhat）**
* **前端框架（Next.js）**
* **自动合约接口生成**
* **钱包集成和测试工具**

它的核心优势是让开发者 **从编写合约到部署前端一站式完成**。

---

#### **2. 项目部署流程**

1. **初始化 Scaffold-ETH 项目**

   * 自动生成 `hardhat` 和 `nextjs` 两个包。
   * 前端和合约集成度高，减少手动配置。

2. **生成部署钱包**

   * 使用 `yarn generate` 创建新钱包，并设置密码加密。
   * 支持导入到 MetaMask，或使用 Scaffold-ETH 的 Burner Wallet。

3. **配置 Sepolia 测试网**

   * 在 `hardhat.config.ts` 配置 RPC URL 和私钥。
   * Sepolia 是以太坊官方测试网，适合部署练习。

4. **部署智能合约**

   ```bash
   yarn deploy --network sepolia
   ```

   部署完成后输出合约地址，并同步到前端。

   本次部署的合约地址：

   ```
   0x297fFCAf15eAAB709b875ad74a7ED36B4591F895
   ```

   可以在 [Sepolia Etherscan](https://sepolia.etherscan.io/) 查询。

5. **启动前端应用**

   * 前端自动集成合约信息，支持连接钱包。
   * 本地调试：

     ```bash
     yarn start
     ```

6. **部署前端到 Vercel**

   * 使用 `yarn vercel` 进行上线，生成可访问的 URL。

   本次项目前端地址：
   [https://nft-deployment.vercel.app/](https://nft-deployment.vercel.app/)

---

#### **3. 学到的知识**

* Scaffold-ETH 2 提供一体化开发体验，省去了 ABI 地址手动绑定的繁琐操作。
* 明确了 **Web3 项目标准流程**：钱包 → 测试网 → 合约部署 → 前端集成 → 上线。
* 学会使用 Vercel 快速上线 Next.js DApp。
* 知道了 API Key 的作用（Alchemy RPC、Etherscan 验证）。

# 2025-08-05

学习了安全相关知识，对面试出现的恶意软件保持警惕，因最近案列太多。以前有关注漫雾后面还会持续关注。合约部署研究中。

# 2025-08-04

完成了全部web3入门导读，加入LXDAO Discord, NFT mint仍然有问题，但整个流程已经读完。


# 2025.07.30


<!-- Content_END -->
