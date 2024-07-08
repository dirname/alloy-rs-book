## 交易生命周期

本文将带你了解将 `100 wei` 从 `Alice` 发送到 `Bob` 的交易定义过程，签署交易并广播已签署的交易到以太坊网络。

让我们以[`TransactionRequest`](https://docs.rs/alloy/latest/alloy/rpc/types/eth/struct.TransactionRequest.html)的形式表达我们的意图:

```rust,ignore
// 构建一个从 Alice 发送 100 wei 到 Bob 的交易。
let tx = TransactionRequest::default()
    .with_from(alice)
    .with_to(bob)
    .with_nonce(nonce)
    .with_chain_id(chain_id)
    .with_value(U256::from(100))
    .with_gas_price(gas_price)
    .with_gas_limit(gas_limit);
```

### 设置

首先我们将设置我们的环境:

我们先定义本地以太坊节点 [Anvil](https://github.com/foundry-rs/foundry/tree/master/crates/anvil) 的 RPC URL。
如果你没有安装 `Anvil`，请参阅 [Foundry](https://github.com/foundry-rs/foundry) 的 [安装指南](https://book.getfoundry.sh/getting-started/installation)。

```rust,ignore
// 启动一个本地 Anvil 节点。
// 确保 `anvil` 在 $PATH 中可用。
let anvil = Anvil::new().try_spawn()?;

// 获取 RPC URL。
let rpc_url = anvil.endpoint().parse()?;
```

```rust,ignore
// 或者你也可以使用 https://chainlist.org/ 上的任何有效 RPC URL。
let rpc_url = "https://eth.merkle.io".parse()?;
```

接下来让我们为 Alice 定义一个 `signer`。默认情况下，`Anvil` 定义了一个助记词："test test test test test test test test test test test junk"。确保不要在测试环境外使用这个助记词。我们将在 [`EthereumWallet`](https://docs.rs/alloy/latest/alloy/network/struct.EthereumWallet.html) 中注册这个 signer，以便在 `Provider` 中签署我们未来的交易。

派生这个助记词的第一个密钥给 Alice：

```rust,ignore
// 设置 signer 来自 Anvil 的第一个默认账户（Alice）。
let signer: PrivateKeySigner = anvil.keys()[0].clone().into();
let wallet = EthereumWallet::from(signer);
```

接下来让我们获取用户 `Alice` 和 `Bob` 的地址：

```rust,ignore
// 创建两个用户，Alice 和 Bob。
let alice = anvil.addresses()[0];
let bob = anvil.addresses()[1];
```

接下来我们可以使用 [`ProviderBuilder`](https://docs.rs/alloy/latest/alloy/providers/struct.ProviderBuilder.html) 来构建 [`Provider`](https://docs.rs/alloy/latest/alloy/providers/trait.Provider.html)。

```rust,ignore
// 使用钱包创建一个 provider。
let provider = ProviderBuilder::new()
    .with_recommended_fillers()
    .wallet(wallet)
    .on_http(rpc_url);
```

注意我们在 [ProviderBuilder](../building-with-alloy/connecting-to-a-blockchain/setting-up-a-provider.md) 上使用了 `.with_recommended_fillers()` 方法来自动[填充字段](../building-with-alloy/understanding-fillers.md)。

让我们修改原来的 `TransactionRequest`，以便使用安装在 `Provider` 上的 [RecommendedFiller](https://docs.rs/alloy/latest/alloy/providers/fillers/type.RecommendedFiller.html) 自动填写交易详情。

`RecommendedFillers` 包括以下填充器：

- [GasFiller](https://docs.rs/alloy/latest/alloy/providers/fillers/struct.GasFiller.html)
- [NonceFiller](https://docs.rs/alloy/latest/alloy/providers/fillers/struct.NonceFiller.html)
- [ChainIdFiller](https://docs.rs/alloy/latest/alloy/providers/fillers/struct.ChainIdFiller.html)

因为我们使用了 `RecommendedFillers`，我们的 `TransactionRequest` 只需要使用原字段的子集：

```diff
// 构建一个从 Alice 发送 100 wei 到 Bob 的交易。
let tx = TransactionRequest::default()
-   .with_from(alice)
    .with_to(bob)
-   .with_nonce(nonce)
-   .with_chain_id(chain_id)
    .with_value(U256::from(100))
-   .with_gas_price(gas_price)
-   .with_gas_limit(gas_limit);
```

变为：

```rust,ignore
// 构建一个从 Alice 发送 100 wei 到 Bob 的交易。
// `from` 字段自动填充为第一个 signer's 地址（Alice）。
let tx = TransactionRequest::default()
    .with_to(bob)
    .with_value(U256::from(100));
```

好多啦！

### 签署并广播交易

鉴于我们在 `Provider` 上配置了一个 signer，我们可以在本地签署交易并在一行中广播：

广播交易后有三种方法监听交易的包含，具体取决于你的需求：

```rust,ignore
// 发送交易并监听交易被广播。
let pending_tx = provider.send_transaction(tx).await?.register().await?;
```

```rust,ignore
// 发送交易并监听交易被包含。
let tx_hash = provider.send_transaction(tx).await?.watch().await?;
```

```rust,ignore
// 发送交易并在交易被包含后获取收据。
let tx_receipt = provider.send_transaction(tx).await?.get_receipt().await?;
```

让我们深入了解我们刚刚做的事情。

通过调用：

```rust,ignore
let tx_builder = provider.send_transaction(tx).await?;
```

[`Provider::send_transaction`](https://docs.rs/alloy/latest/alloy/providers/trait.Provider.html#method.send_transaction) 方法返回一个用于配置等待交易的 [`PendingTransactionBuilder`](https://docs.rs/alloy/latest/alloy/providers/struct.PendingTransactionBuilder.html)。

我们可以在它上面，例如，设置 [`required_confirmations`](https://docs.rs.alloy/latest/alloy/providers/struct.PendingTransactionBuilder.html#method.set_required_confirmations) 或者设置 [`timeout`](https://docs.rs/alloy/latest/alloy/providers/struct.PendingTransactionBuilder.html#method.set_timeout)：

```rust,ignore
// 配置等待交易。
let pending_tx_builder = provider.send_transaction(tx)
    .await?
    .with_required_confirmations(2)
    .with_timeout(Some(std::time::Duration::from_secs(60)));
```

通过传递 `TransactionRequest`，我们填充任何缺失的字段。这涉及填写细节如 nonce、链 ID、gas 价格和 gas 限制：

```diff
// 构建一个从 Alice 发送 100 wei 到 Bob 的交易。
let tx = TransactionRequest::default()
+   .with_from(alice)
    .with_to(bob)
+   .with_nonce(nonce)
+   .with_chain_id(chain_id)
    .with_value(U256::from(100))
+   .with_gas_price(gas_price)
+   .with_gas_limit(gas_limit);
```

作为 `Provider` 上注册的 [钱包的 `fill` 方法](https://docs.rs/alloy/latest/alloy/providers/fillers/trait.TxFiller.html#tymethod.fill) 的一部分，我们使用 Alice 的签名器从填充的 `TransactionRequest` 中构建一个已签名的交易。

此时，`TransactionRequest` 变成一个 `TransactionEnvelope`，准备发送到网络。通过调用 [`register`](https://docs.rs/alloy/latest/alloy/providers/struct.PendingTransactionBuilder.html#method.register), [`watch`](https://docs.rs/alloy/latest/alloy/providers/struct.PendingTransactionBuilder.html#method.watch) 或 [`get_receipt`](https://docs.rs/alloy/latest/alloy/providers/struct.PendingTransactionBuilder.html#method.get_receipt)，我们可以广播交易并跟踪交易状态。

例如：

```rust,ignore
// 发送交易并在交易被包含后获取收据。
let tx_receipt = provider.send_transaction(tx).await?.get_receipt().await?;
```

[`TransactionReceipt`](https://docs.rs/alloy/latest/alloy/rpc/types/struct.TransactionReceipt.html) 提供了交易的完整记录和结果，包括交易哈希、区块详情、使用的 gas 以及涉及的地址。

```rust,ignore
pub struct TransactionReceipt {
    // ...

    /// 交易哈希。
    pub transaction_hash: TxHash,

    /// 在区块中的索引。
    pub transaction_index: Option<TxIndex>,

    /// 包含此交易的区块哈希。
    pub block_hash: Option<BlockHash>,

    /// 包含此交易的区块号。
    pub block_number: Option<BlockNumber>,

    /// 仅此交易使用的 gas。
    pub gas_used: u128,

    /// 发送者地址。
    pub from: Address,

    /// 接收者地址。如果是合约创建交易则为 None。
    pub to: Option<Address>,

    /// 创建的合约地址，若不是部署则为 None。
    pub contract_address: Option<Address>,

    // ...
}
```

这就完成了广播已签署交易的旅程。一旦交易被包含在区块中，它就成为以太坊区块链的一个不可改变的部分，确保 `100 wei` 从 `Alice` 转移到 `Bob` 的记录永久存在。

## 综合起来

```rust,ignore
{{#include ../../lib/examples/examples/transactions/examples/transfer_eth.rs}}
```