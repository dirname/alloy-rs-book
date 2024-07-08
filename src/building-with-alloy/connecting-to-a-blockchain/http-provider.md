## HTTP `Provider`

`Http` 提供者建立与节点的 HTTP 连接，允许你发送 JSON-RPC 请求到节点以获取数据、模拟调用、发送交易等等。

### 初始化 Http 提供者

推荐的初始化 `Http` 提供者的方法是使用 [`on_http`](https://docs.rs/alloy/latest/alloy/providers/struct.ProviderBuilder.html#method.on_http) 方法在 [`ProviderBuilder`](https://docs.rs/alloy/latest/alloy/providers/struct.ProviderBuilder.html) 上。

```rust,ignore
//! 使用 `ProviderBuilder` 上的 `on_http` 方法创建 HTTP 提供者的示例。

use alloy::providers::{Provider, ProviderBuilder};
use eyre::Result;

#[tokio::main]
async fn main() -> eyre::Result<()> {
    // 设置由 RPC 客户端使用的 HTTP 传输。
    let rpc_url = "https://eth.merkle.io".parse()?;

    // 使用 `reqwest` crate 创建一个带有 HTTP 传输的提供者。
    let provider = ProviderBuilder::new().on_http(rpc_url);

    Ok(())
}
```

另一种初始化方法是使用 [`on_builtin`](https://docs.rs/alloy/latest/alloy/providers/struct.ProviderBuilder.html#method.on_builtin) 方法在 [`ProviderBuilder`](https://docs.rs/alloy/latest/alloy/providers/struct.ProviderBuilder.html) 上。根据 URL 的格式，该方法会自动确定连接类型（`Http`、`Ws` 或 `Ipc`）。如果你需要一个装箱的传输方法，该方法特别有用。

```rust,ignore
//! 使用 `ProviderBuilder` 上的 `on_builtin` 方法创建 HTTP 提供者的示例。

use alloy::providers::{Provider, ProviderBuilder};
use eyre::Result;

#[tokio::main]
async fn main() -> eyre::Result<()> {
    // 使用 `reqwest` crate 创建一个带有 HTTP 传输的提供者。
    let provider = ProviderBuilder::new().on_builtin("https://eth.merkle.io").await?;

    Ok(())
}
```

{{#include ../../examples/providers/http.md}}