## 使用大数

以太坊使用大数（也称为“bignums”或“任意精度整数”）来表示其代码库中的某些值以及区块链交易中的一些值。这是必要的，因为 [EVM](https://ethereum.org/en/developers/docs/evm) 在 256 位字大小上运行，这与现代机器通常的 32 位或 64 位不同。这种选择是为了方便使用 256 位密码学（例如 [Keccak-256](https://github.com/ethereum/eth-hash) 哈希或 [secp256k1](https://en.wikipedia.org/wiki/Elliptic_Curve_Digital_Signature_Algorithm) 签名）。

值得注意的是，不仅仅是以太坊这种区块链或加密货币使用大数。许多其他区块链和加密货币也使用大数来表示其各自系统中的值。

### 实用工具

为了创建应用程序，通常需要在容易让人理解的值（例如 ether）和合约及数学函数使用的机器可读形式（例如 wei）之间进行转换。这在以太坊工作中尤为重要，因为某些数值，如余额和 gas 价格，在发送交易时必须以 wei 表示，即使它们以其他格式（如 ether 或 gwei）显示给用户。为了帮助进行这种转换，`alloy::primitives::utils` 提供了两个函数，[`parse_units`](https://github.com/alloy-rs/core/blob/main/crates/primitives/src/utils/units.rs) 和 [`format_units`](https://github.com/alloy-rs/core/blob/main/crates/primitives/src/utils/units.rs)，它们可以轻松地在人类可读和机器可读的值形式之间转换。parse_units 可以将表示 ether 值的字符串（例如 "1.1"）转换为可以在合约和数学函数中使用的 wei 大数。format_units 可以将大数值转换为人类可读的字符串，这对于向用户显示值非常有用。

{{#include ../../examples/big-numbers/math_operations.md}}

{{#include ../../examples/big-numbers/math_utilities.md}}