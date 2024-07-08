## `sol!` 过程宏

`sol!` 过程宏解析 Solidity 语法以生成实现了 [alloy-sol-types](https://github.com/alloy-rs/core/tree/main/crates/sol-types) 特性的类型。它使用 [`syn-solidity`](https://github.com/alloy-rs/core/tree/main/crates/syn-solidity)，一个基于 [syn](https://github.com/dtolnay/syn) 的 Solidity 解析器。它旨在模仿官方 Solidity 编译器 (`solc`) 在解析有效 Solidity 代码时的行为。这意味着所有 `solc` `0.5.0` 及以上版本认可的有效 Solidity 代码都被支持。

在最基本的形式下，`sol!` 是这样使用的：

```rust,ignore
use alloy::{primitives::U256, sol};

// 用标准 Solidity 声明一个 Solidity 类型
sol! {
    struct Foo {
        uint256 bar;
        bool baz;
    }
}

// 一个对应的 Rust 结构体被生成了！

// pub struct Foo {
//     pub bar: Uint<256, 4>,
//     pub baz: bool,
// }

let foo = Foo { bar: U256::from(42), baz: true };
```

### 使用方法

有多种使用 `sol!` 宏的方法。

你可以直接编写 Solidity 代码：

```rust,ignore
sol! {
    contract Counter {
        uint256 public number;

        function setNumber(uint256 newNumber) public {
            number = newNumber;
        }

        function increment() public {
            number++;
        }
    }
}
```

或者提供一个 Solidity 文件的路径：

```rust,ignore
sol!(
    #[sol(rpc)]
    Counter,
    "artifacts/Counter.json"
);
```

另外，如果你启用了 `json` 特性标志，你可以提供一个 ABI，或者是一个 JSON 格式的路径：

```rust,ignore
sol!(
   ICounter,
   r#"[
        {
            "type": "function",
            "name": "increment",
            "inputs": [],
            "outputs": [],
            "stateMutability": "nonpayable"
        },
        {
            "type": "function",
            "name": "number",
            "inputs": [],
            "outputs": [
                {
                    "name": "",
                    "type": "uint256",
                    "internalType": "uint256"
                }
            ],
            "stateMutability": "view"
        },
        {
            "type": "function",
            "name": "setNumber",
            "inputs": [
                {
                    "name": "newNumber",
                    "type": "uint256",
                    "internalType": "uint256"
                }
            ],
            "outputs": [],
            "stateMutability": "nonpayable"
        }
   ]"#
);
```

这与以下内容相同：

```rust,ignore
sol! {
    interface ICounter {
        uint256 public number;

        function setNumber(uint256 newNumber);

        function increment();
    }
}
```

另外，你也可以通过文件加载一个 ABI：

```rust,ignore
sol!(
    ICounter,
    "abi/Counter.json"
);
```

你也可以直接使用函数：

```rust,ignore
sol!(
    function swapExactTokensForTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
      ) external returns (uint256[] memory amounts);
);

println!("解码 https://etherscan.io/tx/0xd1b449d8b1552156957309bffb988924569de34fbf21b51e7af31070cc80fe9a");

let input = hex::decode("0x38ed173900000000000000000000000000000000000000000001a717cc0a3e4f84c00000000000000000000000000000000000000000000000000000000000000283568400000000000000000000000000000000000000000000000000000000000000a0000000000000000000000000201f129111c60401630932d9f9811bd5b5fff34e000000000000000000000000000000000000000000000000000000006227723d000000000000000000000000000000000000000000000000000000000000000200000000000000000000000095ad61b0a150d79219dcf64e1e6cc01f0b64c4ce000000000000000000000000dac17f958d2ee523a2206206994597c13d831ec7")?;

// 使用生成的 `swapExactTokensForTokens` 绑定解码输入。
let decoded = swapExactTokensForTokensCall::abi_decode(&input, false);
```

### 属性

结合 `sol!` 宏的 `#[sol(rpc)]` 属性，[`CallBuilder`](https://docs.rs/alloy/latest/alloy/contract/struct.CallBuilder.html) 可以用来与链上合约进行交互。`#[sol(rpc)]` 属性为合约中的每个函数生成一个方法，该方法返回该函数的 `CallBuilder`。

如果提供了 `#[sol(bytecode = "0x...")]`，合约可以通过 `Counter::deploy` 部署，并会创建一个新实例。

```rust,ignore
//! 示例展示如何使用 `#[sol(rpc)]` 和 #[sol(bytecode = "0x...")] 属性
{{#include ../../lib/examples/examples/contracts/examples/deploy_from_contract.rs:2:}}
```