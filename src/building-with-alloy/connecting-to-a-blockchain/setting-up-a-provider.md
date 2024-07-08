## 设置 `Provider`

一个 [`Provider`](https://docs.rs/alloy/latest/alloy/providers/trait.Provider.html) 是对 Ethereum 网络连接的抽象，提供一个简洁、一致的接口来使用标准的 Ethereum 节点功能。

### Builder

创建 [`Provider`](https://docs.rs/alloy/latest/alloy/providers/trait.Provider.html) 的正确方式是通过 [`ProviderBuilder`](https://docs.rs/alloy/latest/alloy/providers/struct.ProviderBuilder.html)，这是一个 [builder](https://rust-unofficial.github.io/patterns/patterns/creational/builder.html)。

Alloy 提供了具体的传输实现，比如 [`HTTP`](./http-provider.md)、[`WS` (WebSockets)](./ws-provider.md) 和 [`IPC` (进程间通信)](./ipc-provider.md)，以及封装单个或多个传输的高级传输方式。

```rust,ignore
//! 使用 `on_http` 方法的 HTTP provider 示例。

use alloy::providers::{Provider, ProviderBuilder};
use eyre::Result;

#[tokio::main]
async fn main() -> eyre::Result<()> {
    // 设置由 RPC 客户端使用的 HTTP 传输。
    let rpc_url = "https://eth.merkle.io".parse()?;

    // 使用 `reqwest` crate 创建一个带有 HTTP 传输的 provider。
    let provider = ProviderBuilder::new().on_http(rpc_url);

    Ok(())
}
```

接下来，让我们看看 [HTTP Provider](./http-provider.md)。