第一次写这个，迷惑但又有意思

SPDX（Software Package Data Exchange）是一种旨在帮助软件组件之间共享许可证信息的标准。SPDX标识符是一个简短的字符串，用于指定软件组件的许可证类型。通过使用SPDX标识符，开发人员和工具可以轻松识别和处理许可证信息，从而减少混淆和错误。

在Solidity智能合约中，`SPDX-License-Identifier`用于指定合约的许可证类型。常见的许可证类型包括MIT、GPL-3.0、Apache-2.0等。使用SPDX标识符有助于明确合约的版权和许可证信息，使其符合开源最佳实践。

# 1

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract HelloWorld{
	string public greet = "Hello World!";
}
```



# 2

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.13;

contract Counter{
    uint public count = 18;

    function get() public view returns (uint){
        return count;
    }

    function inc() public {
        count += 1;
    }

    
    function dec() public {
        count -= 1;
    }
}
```

我他妈真服了这傻逼remix，你妈运行就是出问题，改一下代码老子刷新一次。

这无非就是一个改进版。

首先deploy需要消耗eth，完事儿对数据加减也会消耗ETH

get和数据本身不消耗eth

