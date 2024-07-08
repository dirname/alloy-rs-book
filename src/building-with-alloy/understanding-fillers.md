## 理解 `Fillers`

[Fillers](https://docs.rs/alloy/latest/alloy/providers/fillers/index.html) 装饰了一个 [`Provider`](https://docs.rs/alloy/latest/alloy/providers/trait.Provider.html)，在交易发送到网络之前填充交易细节。Fillers 用于设置 nonce、gas 价格、gas 限制和其他交易细节，它们在任何其他层调用之前被调用。

{{#include ../examples/fillers/recommended_fillers.md}}

{{#include ../examples/fillers/gas_filler.md}}

{{#include ../examples/fillers/nonce_filler.md}}

{{#include ../examples/fillers/wallet_filler.md}}