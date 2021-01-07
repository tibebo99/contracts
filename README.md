# `contracts`

> TODO: description

## Usage

```
const contracts = require('contracts');

// TODO: DEMONSTRATE API
```

## Addresses

### Arbitrum

#### L1

_Pre Deployed_
* L1 DAI (Pool Token): `0x7d669A64deb8a4A51eEa755bb0E19FD39CE25Ae9`
* L1 Messenger: `0xE681857DEfE8b454244e701BA63EfAa078d7eA85`

_Bridge System_
* L1 Bridge: `0xC9898E162b6a43dc665B033F1EF6b2bc7B0157B4`
* L1 Bridge Wrapper: `0xb5cAC377180fcE007664Cc65ff044d685e0F1A3b`


#### L2

_Pre Deployed_
* L2 DAI (oDAI): `0x7d669a64deb8a4a51eea755bb0e19fd39ce25ae9`
* L2 Messenger: `0x0000000000000000000000000000000000000064`

_Bridge System_
* L2 Bridge: `0xf8E96392b1Ba3B2FD88041894a93e089E93C0dcd`
* L2 Uniswap Factory: `0xEaAec7a29B6ccE9e831C8d07e989fa4163026177`
* L2 Uniswap Router: `0xBae19197DFa25105E832b8fAfeAB88aCa275385F`
* L2 Uniswap Exchange: `0xea535dF09be62d5542161D1a4A429A831d329638`

## Scripts

### Deploy and setup arbitrum

* npx hardhat run scripts/arbitrum/deployArbitrumL1.ts --network kovan
* npx hardhat run scripts/arbitrum/deployArbitrumL2.ts --network arbitrum
* npx hardhat run scripts/arbitrum/setupArbitrumL1.ts --network kovan
* npx hardhat run scripts/arbitrum/setupArbitrumL2.ts --network arbitrum

## Definitions
* **Transfer** - The data for a transfer from one chain to another.
* **TransferHash** - The hash of a single transfer's data.
* **TransferRoot** - The merkle root of a tree of TransferHashs and associated metadata such as the destination chainIds and totals for each chain.
* **Bridge** - A hop bridge contracts on L1 or L2 ("L1 Bridge", "Hop Bridge", "Arbitrum Bridge", "Optimism Bridge")
* **Canonical Token Bridge** - A Rollup's own token bridge. ("Canonical Arbitrum Bridge", "Canonical Optimism Bridge")

#### Tokens

* **Canonical L1 Token** - The layer 1 token that is being bridged.
  ("Canonical L1 ETH", "Canonical L1 DAI", "DAI", "ETH")
* **hToken** - Exists on L2 and represents the right to 1 Token deposited in the L1 bridge.
  hToken's can be converted to their Canonical L1 Token or vice versa at a 1:1 rate.
  ("hDAI", "hETH")
* **Canonical L2 Token** - The primary L2 representation of a Canonical L1 Token. This is the
  token you get from depositing into a rollup's Canonical Token Bridge.

#### Token Path
On Hop, tokens are always converted along the following path. To convert DAI to Arbitrum DAI, DAI (on L1) is first converted to hDAI (on L2) using the L1 Hop Bridge. Then the hDAI is swapped for Arbitrum DAI through the Uniswap market. This can be done in one transaction by calling `sendToL2AndAttemptSwap`.

```
      Layer 1          |      Layer 2
                       |
Canonical L1 Token <---|---> hToken <--(Uniswap)--> Canonical L2 Token
                       |
```

e.g.

```
      Layer 1          |      Layer 2
                       |
DAI <--------------<---|---> hDAI <----(Uniswap)--> Arbitrum DAI
                       |
```

## FAQ

* What are the relevant `messageId`s?

    * Arbitrum = `0x9186606d55c571b43a756333453d90ab5653c483deb4980cda697bfa36fba5de`
      Optimism = `0x09d0f27659ee556a8134fa56941e42400e672aecc2d4cfc61cdb0fcea4590e05`