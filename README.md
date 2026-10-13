# dev-tools-eth

A single HTML page for day-to-day Ethereum scanner work: create an address, build the watch-list payload, and sign a Collect transaction in the browser.

**Live:** [https://afsh7n.github.io/dev-tools-eth/](https://afsh7n.github.io/dev-tools-eth/)

The private key never leaves the tab. Address generation and signing run locally with `ethers` from a CDN. The page does not talk to a node and cannot speak native gRPC, so Scan and Collect only produce the JSON you paste into Postman or grpcurl.

## What you use it for

1. **Address** — generate an Ethereum keypair (`0x…`), reveal the key, or download and load `wallet.json`.
2. **Scan** — build `BatchUpdateAddresses` for `ethereum_scanner.AddressService`, operation `ADDRESS_OPERATION_ADD`.
3. **Sign Collect** — sign one item for `ethereum_scanner.TransactionService/Collect`.

An empty ERC-20 contract field signs native ETH. A contract address signs an ERC-20 `transfer`. `amount` is an atomic-unit integer (wei for ETH).

`signed_tx` is the raw signed transaction hex. Leave `gasPrice` empty for an EIP-1559 type-2 transaction. Set `gasPrice` for a legacy type-0 transaction. That hex is the payload the scanner parses and later broadcasts.

## Sign fields

| Field | Role |
| --- | --- |
| `to_address` | Recipient. `0x` address. |
| `amount` | Atomic units. |
| `nonce` | Sender nonce. Integer `>= 0`. Default `0`. |
| `token_id` | Catalog token id. |
| ERC-20 contract | Empty means native ETH. |
| `chainId` | Chain id. Default `1`. |
| `gasLimit` | Empty means `21000` for ETH and `65000` for ERC-20. |
| `maxFeePerGas` | Wei. EIP-1559. Default `1000000000`. |
| `maxPriorityFeePerGas` | Wei. EIP-1559. Default `1000000000`. |
| `gasPrice` | Wei. Set this to sign a legacy type-0 transaction instead. |

## Open it

Use the [live page](https://afsh7n.github.io/dev-tools-eth/), or open `index.html` locally. Do not commit `wallet.json`.
