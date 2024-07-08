## 起步

Alloy 允许应用程序通过使用提供者（providers）来连接区块链。提供者充当应用程序和以太坊节点之间的接口，使您能够通过 JSON-RPC 消息发送请求和接收响应。

使用提供者可以执行的一些常见操作包括：

- 获取当前区块号
- 获取以太坊地址的余额
- 向区块链发送交易
- 调用智能合约函数
- 订阅日志和智能合约事件
- 获取地址的交易历史记录

在[安装](./installation.md)了 `alloy` 之后，让我们创建一个使用 HTTP 提供者并获取最新区块号的示例。

安装 [`tokio`](https://crates.io/crates/tokio) 和 [`eyre`](https://crates.io/crates/eyre) 作为依赖项，并按如下定义主体：

```rust,ignore
//! 使用 `ProviderBuilder` 的 `on_http` 方法创建 HTTP 提供者的示例。

use alloy::providers::{Provider, ProviderBuilder};
use eyre::Result;

#[tokio::main]
async fn main() -> Result<()> {
    // ...

    Ok(())
}
```

接下来，向主体中添加以下部分，以使用 HTTP 传输创建提供者：

```rust,ignore
// 设置由 RPC 客户端使用的 HTTP 传输。
let rpc_url = "https://eth.merkle.io".parse()?;

// 使用 `reqwest` crate 创建带有 HTTP 传输的提供者。
let provider = ProviderBuilder::new().on_http(rpc_url);
```

最后，我们使用提供者获取最新的区块号：

```rust,ignore
// 获取最新区块号。
let latest_block = provider.get_block_number().await?;

// 打印区块号。
println!("Latest block number: {latest_block}");
```

可以在[这里](https://github.com/alloy-rs/examples/blob/main/examples/providers/examples/http.rs)找到完整且可运行的示例，这是[许多 Alloy 可运行示例](https://github.com/alloy-rs/examples/blob/main/README.md#overview)中的一个，供您探索。