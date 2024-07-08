## IPC `Provider`

[IPC（进程间通信）](https://en.wikipedia.org/wiki/Inter-process_communication) 传输允许我们的程序通过本地 [Unix 域 socket](https://en.wikipedia.org/wiki/Unix_domain_socket) 或 [Windows 命名管道](https://learn.microsoft.com/en-us/windows/win32/ipc/named-pipes)与一个节点进行通信。

使用 IPC 传输让 ethers 库可以向 Ethereum 客户端发送 JSON-RPC 请求并接收响应，而无需网络连接或 HTTP 服务器。这对于与运行在同一网络上的本地 Ethereum 节点进行交互非常有用。使用 IPC [比 RPC 更快](https://github.com/0xKitsune/geth-ipc-rpc-bench)，但是你需要能够连接到一个本地节点。

### 初始化一个 `Ipc` 提供者

初始化 `Ipc` 提供者的推荐方式是使用 [`ProviderBuilder`](https://docs.rs/alloy/latest/alloy/providers/struct.ProviderBuilder.html) 上的 [`on_ipc`](https://docs.rs/alloy/latest/alloy/providers/struct.ProviderBuilder.html#method.on_ipc) 方法，并配置 [`IpcConnect`](https://docs.rs/alloy/latest/alloy/providers/struct.IpcConnect.html)。

```rust,ignore
//! 使用 `ProviderBuilder` 上的 `on_ipc` 方法创建一个 IPC 提供者的示例。

use alloy::providers::{IpcConnect, Provider, ProviderBuilder};
use eyre::Result;

#[tokio::main]
async fn main() -> eyre::Result<()> {
    // 设置由 RPC 客户端使用的 IPC 传输。
    let ipc_path = "/tmp/reth.ipc";

    // 创建提供者。
    let ipc = IpcConnect::new(ipc_path.to_string());
    let provider = ProviderBuilder::new().on_ipc(ipc).await?;

    Ok(())
}
```

另一种初始化方式是使用 [`ProviderBuilder`](https://docs.rs/alloy/latest/alloy/providers/struct.ProviderBuilder.html) 上的 [`on_builtin`](https://docs.rs/alloy/latest/alloy/providers/struct.ProviderBuilder.html#method.on_builtin) 方法。此方法会根据 URL 的格式自动确定连接类型（`Http`、`Ws` 或 `Ipc`）。如果你需要一个盒装传输，此方法特别有用。

```rust,ignore
//! 使用 `ProviderBuilder` 上的 `on_builtin` 方法创建一个 IPC 提供者的示例。

use alloy::providers::{Provider, ProviderBuilder};
use eyre::Result;

#[tokio::main]
async fn main() -> eyre::Result<()> {
    // 使用 IPC 传输创建提供者。
    let provider = ProviderBuilder::new().on_builtin("/tmp/reth.ipc").await?;

    Ok(())
}
```

{{#include ../../examples/providers/ipc.md}}