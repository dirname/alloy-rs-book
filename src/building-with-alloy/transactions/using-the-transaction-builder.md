## 使用 `TransactionBuilder`

[`TransactionBuilder`](https://docs.rs/alloy/latest/alloy/network/trait.TransactionBuilder.html) 是一个用于特定网络的交易构建器，可通过 `.with_*` 方法进行配置。

常见的可配置字段包括：

- [with_from](https://docs.rs/alloy/latest/alloy/network/trait.TransactionBuilder.html#method.with_from)
- [with_to](https://docs.rs/alloy/latest/alloy/network/trait.TransactionBuilder.html#method.with_to)
- [with_nonce](https://docs.rs/alloy/latest/alloy/network/trait.TransactionBuilder.html#method.with_nonce)
- [with_chain_id](https://docs.rs/alloy/latest/alloy/network/trait.TransactionBuilder.html#method.with_chain_id)
- [with_value](https://docs.rs/alloy/latest/alloy/network/trait.TransactionBuilder.html#method.with_value)
- [with_gas_limit](https://docs.rs.alloy/latest/alloy/network/trait.TransactionBuilder.html#method.with_gas_limit)
- [with_max_priority_fee_per_gas](https://docs.rs/alloy/latest/alloy/network/trait.TransactionBuilder.html#method.with_max_priority_fee_per_gas)
- [with_max_fee_per_gas](https://docs.rs/alloy/latest/alloy/network/trait.TransactionBuilder.html#method.with_max_fee_per_blob_gas)

通常推荐使用构建者模式，如下所示，而不是直接设置值（相比于 `set_to`，使用 `with_to`）。

```rust,ignore
//! 展示如何使用 `TransactionBuilder` 构建交易的示例
{{#include ../../../lib/examples/examples/transactions/examples/send_eip1559_transaction.rs:2:}}
```

建议在 [ProviderBuilder](../connecting-to-a-blockchain/setting-up-a-provider.md) 上使用 `.with_recommended_fillers()` 方法，该方法会自动为你[填充字段](../understanding-fillers.md)。

{{#include ../../examples/fillers/recommended_fillers.md}}