---
timezone: UTC+8
---

# Wayland

**GitHub ID:** evelive3

**Telegram:** @oxwayland

## Self-introduction

Web2从业者；全栈偏后端开发，主力语言Python、Golang，学习Rust中；爱好矿晶、化石、游戏

## Notes

<!-- Content_START -->
# 2025-08-04

# 正式打卡第一天

## 尝试 

- 昨天从[faucet](https://cloud.google.com/application/web3/faucet/ethereum/sepolia)领取了0.05个Sepolia
- 今天尝试从[sepolia-faucet.pk910.de](https://sepolia-faucet.pk910.de/)挖了2.5个Sepolia
- 给同学发了0.03个Sepolia
- 参加了以太坊中文周会并做了笔记
- 完成了[unphishable](https://unphishable.io/)钓鱼攻防挑战初级和中级内容
- 完成了[Web3 实习手册](https://web3intern.xyz/)内容的学习（其实前几天也在断断续续的看），拟了下近期学习目标
- 写了一篇关于[sepolia-faucet.pk910.de](https://sepolia-faucet.pk910.de/)的分析笔记

## 分析

- 网站
	- [sepolia-faucet.pk910.de](https://sepolia-faucet.pk910.de/)
- 行为分析
	- Sepolia是eth的测试网，已经完成PoS改造，不可能通过PoW获取到代币奖励
	- 运行该网站的mine程序后，CPU占用上升，最高可达100%，界面上可以看到Hash rate
	- 结束mine后，会向用户配置的钱包地址汇入Sepolia代币
	- 结合以上表现，猜测该网站可能以Mine Sepolia为诱饵，使用户自行执行可CPU mine的币种挖矿程序
- 官方网站声明
	- [github:pk910/PoWFaucet](https://github.com/pk910/PoWFaucet?tab=readme-ov-file)
		- For clarification: This faucet does NOT generate new coins with the "mining" process. It's just one of the protection methods the faucet uses to prevent anyone from requesting big amount of funds and draining the faucet wallet. If you want to run your own instance you need to transfer the funds you want to distribute to the faucet wallet yourself!
	- [How Our Faucet Protects Itself and You](https://github.com/pk910/PoWFaucet/wiki#how-our-faucet-protects-itself-and-you)
	- 该网站声明，PoW确实不能产生新币，mine过程只是为了防止恶意请求快速消耗faucet钱包余额，而设置的自我保护措施
- 代码分析
	- 该项目主要由三部分组成
		- 静态资源
		- typescript
			- 核心为faucet-client，实现了程序入口、三种worker和CSS外观
		- wasm
			- 包含CryptoNight, Scrypt, Argon2等抗ASIC加密算法的wasm实现，及一个sqlite3实现
	- client与server间通信通过WebSocket，端点为`/ws/pow`，通过`session id`参数进行会话管理
	- 框架使用`React`
- 运行中ws数据流监听
	- 端点
		- `wss://sepolia-faucet.pk910.de/ws/pow?session=0e17686e-05d8-41f4-a2f9-312cfb49d0d4&cliver=2.4.0`
	- Sent
		- 初始任务
			-
			  ```json
			  {
			      "id": 1,
			      "action": "foundShare",
			      "data": {
			          "nonce": 71,
			          "data": null,
			          "params": "argon2|0|13|4|4096|1|16|13",
			          "hashrate": 491.471512640656
			      }
			  }
			  ```
		- 后续任务
			-
			  ```json
			  {
			      "id": 44,
			      "action": "verifyResult",
			      "data": {
			          "shareId": "902358ad-899d-4592-bc4e-890d322525fd",
			          "params": "argon2|0|13|4|4096|1|16|13",
			          "isValid": true
			      }
			  }
			  ```
	- Received
		- 任务确认
			-
			  ```json
			  {
			      "action": "ok",
			      "data": null,
			      "rsp": 1
			  }
			  ```
		- 任务验证
			-
			  ```json
			  {
			      "action": "verify",
			      "data": {
			          "shareId": "46096c68-9f81-4ce8-b73d-a6b9ae68852b",
			          "preimage": "tMSRZ3ecDcE=",
			          "nonce": 2430725,
			          "data": null
			      }
			  }
			  ```
		- 本地状态更新
			-
			  ```json
			  {
			      "action": "updateBalance",
			      "data": {
			          "balance": "1986000000000000",
			          "reason": "valid share (reward: 1986000000000000)"
			      }
			  }
			  ```
		- 每次完成验证后，`rsp`+1开启下一个任务
- 结论
	- 使用了抗ASIC矿机的专用算法，与门罗等CPU币使用的算法高度重合，可能是为了自我保护，也可能是真实挖矿行为
	- 如果将计算部分完全剥离，用户侧只代理计算过程，与矿池通信完全放在server端，用户无法直接发现异常
	- 如果只针对钱包地址做防护，为每个地址设置24小时的冷却时间，无法防止通过大量钱包地址发起的恶意领取，使用CPU PoW确实是有效的防御手段
	- 无法证伪官方的声明，不排除使用用户CPU挖矿的可能，建议使用不需要挖矿的faucet，警惕需要额外授权的行为，注意CPU使用率是否异常升高


# 2025.07.29


<!-- Content_END -->
