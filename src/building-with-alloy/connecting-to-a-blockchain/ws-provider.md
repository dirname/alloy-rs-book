## WS `Provider`

`Ws` 提供者与节点建立 WebSocket 连接，允许你发送 JSON-RPC 请求到节点以获取数据、模拟调用、发送交易等功能。`Ws` 提供者可以用于任何支持 WebSocket 连接的以太坊节点。这使得程序可以实时与网络进行交互，而不需要通过 HTTP 轮询来获取诸如新区块头和过滤日志之类的信息。

### 初始化 `Ws` 提供者

推荐的方法是使用 [`ProviderBuilder`](https://docs.rs/alloy/latest/alloy/providers/struct.ProviderBuilder.html) 的 [`on_ws`](https://docs.rs/alloy/latest/alloy/providers/struct.ProviderBuilder.html#method.on_ws) 方法与 [`WsConnect`](https://docs.rs/alloy/latest/alloy/providers/struct.WsConnect.html) 配置来初始化 `Ws` 提供者。

```rust,ignore
//! 使用 `ProviderBuilder` 的 `on_ws` 方法创建一个 WS 提供者的示例。

use alloy::providers::{Provider, ProviderBuilder, WsConnect};
use eyre::Result;

#[tokio::main]
async fn main() -> eyre::Result<()> {
    // 设置由 RPC 客户端消费的 WS 传输。
    let rpc_url = "wss://eth-mainnet.g.alchemy.com/v2/your-api-key";

    // 创建提供者。
    let ws = WsConnect::new(rpc_url);
    let provider = ProviderBuilder::new().on_ws(ws).await?;

    Ok(())
}
```

另一种初始化方法是使用 [`ProviderBuilder`](https://docs.rs/alloy/latest/alloy/providers/struct.ProviderBuilder.html) 的 [`on_builtin`](https://docs.rs/alloy/latest/alloy/providers/struct.ProviderBuilder.html#method.on_builtin) 方法。该方法将根据 URL 的格式自动确定连接类型（`Http`、`Ws` 或 `Ipc`）。如果你需要一个封装的 transport，这个方法特别有用。

```rust,ignore
//! 使用 `ProviderBuilder` 的 `on_builtin` 方法创建一个 WS 提供者的示例。

use alloy::providers::{Provider, ProviderBuilder};
use eyre::Result;

#[tokio::main]
async fn main() -> eyre::Result<()> {
    // 使用 WS 传输创建一个提供者。
    let provider = ProviderBuilder::new().on_builtin("wss://eth-mainnet.g.alchemy.com/v2/your-api-key").await?;

    Ok(())
}
```

与其他提供者类似，你也可以通过 WebSockets 与节点建立授权连接。

{{#include ../../examples/providers/ws.md}}

{{#include ../../examples/providers/ws_with_auth.md}}