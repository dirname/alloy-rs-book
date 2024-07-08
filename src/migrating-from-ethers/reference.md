## 参考

[ethers-rs](https://github.com/gakonst/ethers-rs/) 已经被弃用，迁移至 [Alloy](https://github.com/alloy-rs/) 和 [Foundry](https://github.com/foundry-rs/)。

以下是找到特定 crate、依赖或信息源的迁移路径的参考指南。

### 文档

- 书籍: [`ethers-rs/book`](https://github.com/gakonst/ethers-rs/tree/master/book) `->` [`alloy-rs/book`](https://github.com/alloy-rs/book)

### 示例

- 示例: [`ethers-rs/examples`](https://github.com/gakonst/ethers-rs/tree/master/examples) `->` [`alloy-rs/examples`](https://github.com/alloy-rs/examples)

### Crates

- Meta-crate: [`ethers`](https://github.com/gakonst/ethers-rs/tree/master/ethers) `->` [`alloy`](https://github.com/alloy-rs/alloy/tree/main/crates/alloy)
- 地址簿: [`ethers::addressbook`](https://github.com/gakonst/ethers-rs/tree/master/ethers-addressbook) `->` 无计划
- 编译器: [`ethers::solc`](https://github.com/gakonst/ethers-rs/tree/master/ethers-solc) `->` [`foundry-compilers`](https://github.com/foundry-rs/compilers)
- 合约: [`ethers::contract`](https://github.com/gakonst/ethers-rs/tree/master/ethers-contract) `->` [`alloy::contract`](https://github.com/alloy-rs/alloy/tree/main/crates/contract)
- 核心: [`ethers::core`](https://github.com/gakonst/ethers-rs/tree/master/ethers-core) `->` [`alloy::core`](https://github.com/alloy-rs/core)
  - 类型: [`ethers::core::types::*`](https://github.com/gakonst/ethers-rs/tree/master/ethers-core/src/types) `->` 见 [Types](#types) 部分
- Etherscan: [`ethers::etherscan`](https://github.com/gakonst/ethers-rs/tree/master/ethers-etherscan) `->` [`foundry-block-explorers`](https://github.com/foundry-rs/block-explorers)
- 中间件: [`ethers::middleware`](https://github.com/gakonst/ethers-rs/tree/master/ethers-middleware) `->` 填充器 [`alloy::provider::{fillers, layers}`](https://github.com/alloy-rs/alloy/tree/main/crates/provider/src)
  - Gas 预言机: [`ethers::middleware::GasOracleMiddleware`](https://github.com/gakonst/ethers-rs/tree/master/ethers-middleware/src/gas_oracle/middleware.rs) `->` Gas 填充器 [`alloy::provider::GasFiller`](https://github.com/alloy-rs/examples/tree/main/examples/fillers/examples/gas_filler.rs)
  - Gas 加速器: [`ethers::middleware::GasEscalatorMiddleware`](https://github.com/gakonst/ethers-rs/tree/master/ethers-middleware/src/gas_escalator) `->` 无计划
  - 变换器: [`ethers::middleware::TransformerMiddleware`](https://github.com/gakonst/ethers-rs/tree/master/ethers-middleware/src/transformer) `->` 无计划
  - 策略: [`ethers::middleware::policy::*`](https://github.com/gakonst/ethers-rs/blob/master/ethers-middleware/src/policy.rs) `->` 无计划
  - Timelag: [`ethers::middleware::timelag::*`](https://github.com/gakonst/ethers-rs/tree/master/ethers-middleware/src/timelag) `->` 无计划
  - Nonce 管理器: [`ethers::middleware::NonceManagerMiddleware`](https://github.com/gakonst/ethers-rs/tree/master/ethers-middleware/src/nonce_manager.rs) `->` Nonce 填充器[`alloy::provider::NonceFiller`](https://github.com/alloy-rs/alloy/tree/main/crates/provider/src/fillers/nonce.rs)
  - 签名: [`ethers::middleware::Signer`](https://github.com/gakonst/ethers-rs/tree/master/ethers-middleware/src/signer.rs) `->` 钱包填充器 [`alloy::provider::WalletFiller`](https://github.com/alloy-rs/alloy/tree/main/crates/provider/src/fillers/wallet.rs)
- 提供者: [`ethers::providers`](https://github.com/gakonst/ethers-rs/tree/master/ethers-providers) `->` 提供者 [`alloy::providers`](https://github.com/alloy-rs/alloy/tree/main/crates/provider)
- 传输: [`ethers::providers::transports`](https://github.com/gakonst/ethers-rs/tree/master/ethers-providers/src/rpc/transports) `->` [`alloy::transports`](https://github.com/alloy-rs/alloy/tree/main/crates/transport)
  - HTTP: [`ethers::providers::Http`](https://github.com/gakonst/ethers-rs/tree/master/ethers-providers/src/rpc/transports/http.rs) `->` [`alloy::transports::http`](https://github.com/alloy-rs/alloy/tree/main/crates/transport-http)
  - IPC: [`ethers::providers::Ipc`](https://github.com/gakonst/ethers-rs/tree/master/ethers-providers/src/rpc/transports/ipc.rs) `->` [`alloy::transports::ipc`](https://github.com/alloy-rs/alloy/tree/main/crates/transport-ipc)
  - WS: [`ethers::providers::Ws`](https://github.com/gakonst/ethers-rs/tree/master/ethers-providers/src/rpc/transports/ws) `->` [`alloy::transports::ws`](https://github.com/alloy-rs/alloy/tree/main/crates/transport-ws)
- 签名者: [`ethers::signers`](https://github.com/gakonst/ethers-rs/tree/master/ethers-signers) `->` 签名者 [`alloy::signers`](https://github.com/alloy-rs/alloy/tree/main/crates/signer)
  - AWS: [`ethers::signers::aws::*`](https://github.com/gakonst/ethers-rs/tree/master/ethers-signers/src/aws) `->` [`alloy::signers::aws`](https://github.com/alloy-rs/alloy/tree/main/crates/signer-aws)
  - Ledger: [`ethers::signers::ledger::*`](https://github.com/gakonst/ethers-rs/tree/master/ethers-signers/src/ledger) `->` [`alloy::signers::ledger`](https://github.com/alloy-rs/alloy/tree/main/crates/signer-ledger)
  - Trezor: [`ethers::signers::trezor::*`](https://github.com/gakonst/ethers-rs/tree/master/ethers-signers/src/trezor) `->` [`alloy::signer::trezor`](https://github.com/alloy-rs/alloy/tree/main/crates/signer-trezor)
  - 钱包: [`ethers::signers::wallet::*`](https://github.com/gakonst/ethers-rs/tree/master/ethers-signers/src/wallet) `->` [`alloy::signer::local`](https://github.com/alloy-rs/alloy/tree/main/crates/signer-local)

### 类型

#### 原始类型

- 地址: [`ethers::types::Address`](https://github.com/gakonst/ethers-rs/tree/master/ethers-core/src/types/mod.rs) `->` [`alloy::primitives::Address`](https://github.com/alloy-rs/core/tree/main/crates/primitives/src/lib.rs)
- U64: [`ethers::types::U64`](https://github.com/gakonst/ethers-rs/tree/master/ethers-core/src/types/mod.rs) `->` [`alloy::primitives::U64`](https://github.com/alloy-rs/core/tree/main/crates/primitives/src/lib.rs)
- U128: [`ethers::types::U128`](https://github.com/gakonst/ethers-rs/tree/master/ethers-core/src/types/mod.rs) `->` [`alloy::primitives::U128`](https://github.com/alloy-rs/core/tree/main/crates/primitives/src/lib.rs)
- U256: [`ethers::types::U256`](https://github.com/gakonst/ethers-rs/tree/master/ethers-core/src/types/mod.rs) `->` [`alloy::primitives::U256`](https://github.com/alloy-rs/core/tree/main/crates/primitives/src/lib.rs)
- U512: [`ethers::types::U512`](https://github.com/gakonst/ethers-rs/tree/master/ethers-core/src/types/mod.rs) `->` [`alloy::primitives::U512`](https://github.com/alloy-rs/core/tree/main/crates/primitives/src/lib.rs)
- H32: [`ethers::types::H32`](https://github.com/gakonst/ethers-rs/tree/master/ethers-core/src/types/mod.rs) `->` [`alloy::primitives::aliases::B32`](https://github.com/alloy-rs/core/tree/main/crates/primitives/src/lib.rs)
- H64: [`ethers::types::H64`](https://github.com/gakonst/ethers-rs/tree/master/ethers-core/src/types/mod.rs) `->` [`alloy::primitives::B64`](https://github.com/alloy-rs/core/tree/main/crates/primitives/src/lib.rs)
- H128: [`ethers::types::H128`](https://github.com/gakonst/ethers-rs/tree/master/ethers-core/src/types/mod.rs) `->` [`alloy::primitives::B128`](https://github.com/alloy-rs/core/tree/main/crates/primitives/src/lib.rs)
- H160: [`ethers::types::H160`](https://github.com/gakonst/ethers-rs/tree/master/ethers-core/src/types/mod.rs) `->` [`alloy::primitives::B160`](https://github.com/alloy-rs/core/tree/main/crates/primitives/src/lib.rs)
- H256: [`ethers::types::H256`](https://github.com/gakonst/ethers-rs/tree/master/ethers-core/src/types/mod.rs) `->` [`alloy::primitives::B256`](https://github.com/alloy-rs/core/tree/main/crates/primitives/src/lib.rs)
- H512: [`ethers::types::H512`](https://github.com/gakonst/ethers-rs/tree/master/ethers-core/src/types/mod.rs) `->` [`alloy::primitives::B512`](https://github.com/alloy-rs/core/tree/main/crates/primitives/src/lib.rs)
- Bloom: [`ethers::types::Bloom`](https://github.com/gakonst/ethers-rs/tree/master/ethers-core/src/types/mod.rs) `->` [`alloy::primitives::Bloom`](https://github.com/alloy-rs/core/tree/main/crates/primitives/src/lib.rs)
- TxHash: [`ethers::types::TxHash`](https://github.com/gakonst/ethers-rs/tree/master/ethers-core/src/types/mod.rs) `->` [`alloy::primitives::TxHash`](https://github.com/alloy-rs/core/tree/main/crates/primitives/src/lib.rs)

由于 `ruint` 的一个[限制](https://github.com/alloy-rs/core/issues/554#issuecomment-1978620017)，BigEndianHash [`ethers::types::BigEndianHash`](https://github.com/gakonst/ethers-rs/tree/master/ethers-core/src/types/mod.rs) 可以如下表示：

```rust,ignore
// `U256` => `B256`
let x = B256::from(u256);

// `B256` => `U256`
let x: U256 = b256.into();
let x = U256::from_be_bytes(b256.into())
```

#### RPC

- 字节: [`ethers::types::bytes::*`](https://github.com/gakonst/ethers-rs/tree/master/ethers-core/src/types/bytes.rs) `->` [`alloy::primitives::Bytes`](https://github.com/alloy-rs/core/tree/main/crates/primitives/src/lib.rs)
- 链: [`ethers::types::Chain`](https://github.com/gakonst/ethers-rs/tree/master/ethers-core/src/types/chain.rs) `->` [`alloy-rs/chains`](https://github.com/alloy-rs/chains)
- ENS: [`ethers::types::ens`](https://github.com/gakonst/ethers-rs/tree/master/ethers-core/src/types/ens.rs) `->` [`foundry-common`](https://github.com/foundry-rs/foundry/tree/master/crates/common/src/ens.rs)
- Trace: [`ethers::types::trace::*`](https://github.com/gakonst/ethers-rs/tree/master/ethers-core/src/types/trace) `->` [`alloy::rpc::types::trace`](https://github.com/alloy-rs/alloy/tree/main/crates/rpc-types-trace)
- {Block, Fee, Filter, Log, Syncing, Transaction, TxPool}: [`ethers::types::*`](https://github.com/gakonst/ethers-rs/tree/master/ethers-core/src/types) `->` [`alloy::rpc::types::eth::*`](https://github.com/alloy-rs/alloy/tree/main/crates/rpc-types/src/eth)
- Proof: [`ethers::types::proof::*`](https://github.com/gakonst/ethers-rs/tree/master/ethers-core/src/types/proof.rs) `->` 账户 [`alloy::rpc::types::eth::account::*`](https://github.com/alloy-rs/alloy/tree/main/crates/rpc-types/src/eth/account.rs)
- 签名: [`ethers::types::signature::*`](https://github.com/gakonst/ethers-rs/tree/master/ethers-core/src/types/signature.rs) `->` [`alloy::rpc::types::eth::transaction::signature::*`](https://github.com/alloy-rs/alloy/tree/main/crates/rpc-types/src/eth/transaction/signature.rs)
- 提现: [`ethers::types::withdrawal::*`](https://github.com/gakonst/ethers-rs/tree/master/ethers-core/src/types/withdrawal.rs) `->` EIP4895 [`alloy::eips::eip4895`](https://github.com/alloy-rs/alloy/tree/main/crates/eips/src/eip4895.rs)
- 操作码: [`ethers::types::opcode::*`](https://github.com/gakonst/ethers-rs/tree/master/ethers-core/src/types/opcode.rs) `->` [`syn-solidity`](https://github.com/alloy-rs/core/tree/main/crates/syn-solidity)

### ABI

- 绑定: [`abigen!`](https://github.com/gakonst/ethers-rs/tree/51fe937f6515689b17a3a83b74a05984ad3a7f11/ethers-contract/ethers-contract-abigen) `->` [`sol!`](https://github.com/alloy-rs/core/tree/main/crates/sol-types)，可用于 [`alloy::sol`](https://github.com/alloy-rs/alloy/blob/aea7e07b4b335a3a35e3870a6c277d397d0f3932/crates/alloy/src/lib.rs#L52-L64).