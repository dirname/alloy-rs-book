## 安装

[Alloy](https://github.com/alloy-rs/alloy) 包含多个 crate，这些 crate 提供了与任何基于以太坊的区块链交互所需的各种功能。

最简单的入门方式是使用 Cargo 从命令行添加带有 `full` 功能标志的 `alloy` crate：

```sh
cargo add alloy --features full
```

或者，你可以在你的 `Cargo.toml` 文件中添加以下内容：

```toml
alloy = { version = "0.1", features = ["full"] }
```

如果你需要更细粒度的控制所包含的功能，可以在 `Cargo.toml` 文件中添加各个独立的 crate，或者使用带有所需功能的 `alloy` crate。

在 `alloy` 被添加为依赖项后，你可以按如下方式导入 `alloy`：

```rust,ignore
use alloy::{
    network::{eip2718::Encodable2718, EthereumWallet, TransactionBuilder},
    primitives::U256,
    providers::{Provider, ProviderBuilder},
    rpc::types::TransactionRequest,
    signers::local::PrivateKeySigner,
};
```

### 功能

[`alloy`](https://github.com/alloy-rs/alloy/tree/main/crates/alloy) meta-crate 定义了许多 [功能标志](https://github.com/alloy-rs/alloy/blob/main/crates/alloy/Cargo.toml)：

默认

- `std`
- `reqwest`

全功能，包含最常用的标志，适合入门使用 `alloy`。

- `full`

通用功能
- `consensus`
- `contract`
- `eips`
- `genesis`
- `network`
- `node-bindings`

提供者
- `providers`
- `provider-http`
- `provider-ipc`
- `provider-ws`

RPC
- `rpc`
- `json-rpc`
- `rpc-client`
- `rpc-client-ipc`
- `rpc-client-ws`
- `rpc-types`
- `rpc-types-admin`
- `rpc-types-anvil`
- `rpc-types-beacon`
- `rpc-types-debug`
- `rpc-types-engine`
- `rpc-types-eth`
- `rpc-types-json`
- `rpc-types-trace`
- `rpc-types-txpool`

签名者
- `signers`
- `signer-aws`
- `signer-gcp`
- `signer-ledger`
- `signer-ledger-browser`
- `signer-ledger-node`
- `signer-local`
- `signer-trezor`
- `signer-keystore`
- `signer-mnemonic`
- `signer-mnemonic-all-languages`
- `signer-yubihsm`

默认情况下，`alloy` 使用 [`reqwest`](https://crates.io/crates/reqwest) 作为 HTTP 客户端。你也可以选择切换到 [`hyper`](https://crates.io/crates/hyper)。
`reqwest` 和 `hyper` 功能标志是互斥的。

完整的可用功能列表可以在 [docs.rs](https://docs.rs/crate/alloy/latest/features) 或者 [`alloy` crate 的 `Cargo.toml`](https://github.com/alloy-rs/alloy/blob/main/crates/alloy/Cargo.toml) 中找到。

这些功能标志大部分与以下独立 crate 的功能相对应，并启用这些功能。

### Crate

`alloy` 包含以下 crate：

- [alloy](https://github.com/alloy-rs/alloy/tree/main/crates/alloy) - 整个项目的 meta-crate，包括 [`alloy-core`](https://docs.rs/alloy-core)
- [alloy-consensus](https://github.com/alloy-rs/alloy/tree/main/crates/consensus) - 以太坊共识接口
- [alloy-contract](https://github.com/alloy-rs/alloy/tree/main/crates/contract) - 与链上合约交互
- [alloy-eips](https://github.com/alloy-rs/alloy/tree/main/crates/eips) - 以太坊改进提案 (EIP) 实现
- [alloy-genesis](https://github.com/alloy-rs/alloy/tree/main/crates/genesis) - 以太坊创世文件定义
- [alloy-json-rpc](https://github.com/alloy-rs/alloy/tree/main/crates/json-rpc) - JSON-RPC 2.0 客户端的核心数据类型
- [alloy-network](https://github.com/alloy-rs/alloy/tree/main/crates/network) - RPC 类型的网络抽象
- [alloy-node-bindings](https://github.com/alloy-rs/alloy/tree/main/crates/node-bindings) - 以太坊执行层客户端绑定
- [alloy-provider](https://github.com/alloy-rs/alloy/tree/main/crates/provider) - 与以太坊区块链接口
- [alloy-pubsub](https://github.com/alloy-rs/alloy/tree/main/crates/pubsub) - 以太坊 JSON-RPC [发布-订阅](https://en.wikipedia.org/wiki/Publish%E2%80%93subscribe_pattern) tower 服务和类型定义
- [alloy-rpc-client](https://github.com/alloy-rs/alloy/tree/main/crates/rpc-client) - 低级以太坊 JSON-RPC 客户端实现
- [alloy-rpc-types](https://github.com/alloy-rs/alloy/tree/main/crates/rpc-types) - 所有以太坊 JSON-RPC 类型的 meta-crate
  - [alloy-rpc-types-admin](https://github.com/alloy-rs/alloy/tree/main/crates/rpc-types-admin) - `admin` 以太坊 JSON-RPC 命名空间的类型
  - [alloy-rpc-types-anvil](https://github.com/alloy-rs/alloy/tree/main/crates/rpc-types-anvil) - [Anvil](https://github.com/foundry-rs/foundry) 开发节点的以太坊 JSON-RPC 命名空间的类型
  - [alloy-rpc-types-beacon](https://github.com/alloy-rs/alloy/tree/main/crates/rpc-types-beacon) - [以太坊信标节点 API](https://ethereum.github.io/beacon-APIs) 类型
  - [alloy-rpc-types-engine](https://github.com/alloy-rs/alloy/tree/main/crates/rpc-types-engine) - `engine` 以太坊 JSON-RPC 命名空间的类型
  - [alloy-rpc-types-eth](https://github.com/alloy-rs/alloy/tree/main/crates/rpc-types-eth) - `eth` 以太坊 JSON-RPC 命名空间的类型
  - [alloy-rpc-types-trace](https://github.com/alloy-rs/alloy/tree/main/crates/rpc-types-trace) - `trace` 以太坊 JSON-RPC 命名空间的类型
  - [alloy-rpc-types-txpool](https://github.com/alloy-rs/alloy/tree/main/crates/rpc-types-txpool) - `txpool` 以太坊 JSON-RPC 命名空间的类型
- [alloy-serde](https://github.com/alloy-rs/alloy/tree/main/crates/serde) - [Serde](https://serde.rs) 相关实用工具
- [alloy-signer](https://github.com/alloy-rs/alloy/tree/main/crates/signer) - 以太坊签名者抽象
  - [alloy-signer-aws](https://github.com/alloy-rs/alloy/tree/main/crates/signer-aws) - [AWS KMS](https://aws.amazon.com/kms) 签名实现
  - [alloy-signer-gcp](https://github.com/alloy-rs/alloy/tree/main/crates/signer-gcp) - [GCP KMS](https://cloud.google.com/kms) 签名实现
  - [alloy-signer-ledger](https://github.com/alloy-rs/alloy/tree/main/crates/signer-ledger) - [Ledger](https://www.ledger.com) 签名实现
  - [alloy-signer-local](https://github.com/alloy-rs/alloy/tree/main/crates/signer-local) - 本地（私钥，密钥库，助记词，YubiHSM）签名实现
  - [alloy-signer-trezor](https://github.com/alloy-rs/alloy/tree/main/crates/signer-trezor) - [Trezor](https://trezor.io) 签名实现
- [alloy-transport](https://github.com/alloy-rs/alloy/tree/main/crates/transport) - 低级以太坊 JSON-RPC 传输抽象
  - [alloy-transport-http](https://github.com/alloy-rs/alloy/tree/main/crates/transport-http) - HTTP 传输实现
  - [alloy-transport-ipc](https://github.com/alloy-rs/alloy/tree/main/crates/transport-ipc) - IPC 传输实现
  - [alloy-transport-ws](https://github.com/alloy-rs/alloy/tree/main/crates/transport-ws) - WS 传输实现

`alloy-core` 包含以下 crate：

- [alloy-core](https://github.com/alloy-rs/core/tree/main/crates/core) - 整个项目的 meta-crate
- [alloy-dyn-abi](https://github.com/alloy-rs/core/tree/main/crates/dyn-abi) - 运行时 [ABI](https://docs.soliditylang.org/en/latest/abi-spec.html) 和 [EIP-712](https://eips.ethereum.org/EIPS/eip-712) 实现
- [alloy-json-abi](https://github.com/alloy-rs/core/tree/main/crates/json-abi) - 全面的以太坊 [JSON-ABI](https://docs.soliditylang.org/en/latest/abi-spec.html#json) 实现
- [alloy-primitives](https://github.com/alloy-rs/core/tree/main/crates/primitives) - 基本的整数和字节类型
- [alloy-sol-macro-expander](https://github.com/alloy-rs/core/tree/main/crates/sol-macro-expander) - 用于 Solidity 到 Rust 过程宏的扩展器
- [alloy-sol-macro-input](https://github.com/alloy-rs/core/tree/main/crates/sol-macro-input) - [`sol!`](https://docs.rs/alloy-sol-macro/latest/alloy_sol_macro/macro.sol.html) 宏类型的输入类型
- [alloy-sol-macro](https://github.com/alloy-rs/core/tree/main/crates/sol-macro) - [`sol!`](https://docs.rs/alloy-sol-macro/latest/alloy_sol_macro/macro.sol.html) 过程宏
- [alloy-sol-type-parser](https://github.com/alloy-rs/core/tree/main/crates/sol-type-parser) - Solidity 类型字符串的简单解析器
- [alloy-sol-types](https://github.com/alloy-rs/core/tree/main/crates/sol-types) - 编译时 [ABI](https://docs.soliditylang.org/en/latest/abi-spec.html) 和 [EIP-712](https://eips.ethereum.org/EIPS/eip-712) 实现
- [syn-solidity](https://github.com/alloy-rs/core/tree/main/crates/syn-solidity) - [`syn`](https://github.com/dtolnay/syn)-驱动的 Solidity 解析器