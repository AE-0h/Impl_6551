# ERC-6551: Non-fungible Token Bound Accounts

**An interface and registry for smart contract accounts owned by non-fungible tokens.**

Overview:

The system outlined in this proposal has two main components:

- A singleton registry for token bound accounts
- A common interface for token bound account implementations
  The following diagram illustrates the relationship between NFTs, NFT holders, token bound accounts, and the Registry:

![ERC-6551](https://eips.ethereum.org/assets/eip-6551/diagram.png)

# Registry

The registry is a singleton contract that serves as the entry point for all token bound account address queries. It has two functions:

- `createAccount` - creates the token bound account for an NFT given an `[implementation]` address
- `account` - computes the token bound account address for an NFT given an `[implementation]` address

The registry is permissionless, immutable, and has no owner. The complete source code for the registry can be found in the Registry Implementation section. The registry MUST be deployed at address `(0x00000000006551f9487184612e58Fe0681377558)` using Nick's Factory `(0x4e59b48447b37957858992ca078fbF26c0b4956c)` with salt `(0x0000000000000000000000000000000000000000fde8e4e1dca713016c518e31)`.

The registry can be deployed to any EVM-compatible chain using the following transaction:

```json
{
  "to": "0x4e59b48447b37957858992ca078fbF26c0b4956c",
  "value": "0x0",
  "data": "0x0000000000000000000000000000000000000000000000000000000000000000fde8e4e1dca713016c518e31608606405234801651001657060080fd5b5061"
}
```

The registry MUST deploy each token bound account as an ERC-1167 minimal proxy with immutable constant data appended to the bytecode.

The deployed bytecode of each token bound account MUST have the following structure:

ERC-1167 Header (10 bytes)
<implementation (address)> (20 bytes)
ERC-1167 Footer (15 bytes)
<salt (bytes32)> (32 bytes)
<chainId (uint256)> (32 bytes)
<tokenContract (address)> (32 bytes)
<tokenId (uint256)> (32 bytes)
For example, the token bound account with implementation address (0xbebebebebebebebebebebebebebebebebebebebe) salt (0), chain ID (1), token contract (0xcfccfccfccfccfccfccfccfccfccfccfccfccfcc) and token ID (123) would have the following deployed bytecode:

```
ebebebebebebebebebebebebe5af43d8230e930d91602b57fd5bf300000000000000000000000000

```

### Build

```shell
$ forge build
```

### Test

```shell
$ forge test
```
