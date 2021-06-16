---
description: >-
  Gamma protocol is currently Live on Ethereum Mainnet, Ropsten testnet and
  Kovan testnet.
---

# Smart Contract Addresses

## Which Testnet should I use? 

The core protocol is on both Ropsten and Kovan, and we will deploy our own mintable testnet ERC20 tokens for all the assets we're whitelisting, so you don't have to worry about "where to get USDC?".

There are some differences between the Ropsten and Kovan now: We have a more robust pricer system setup on Kovan since Chainlink\(our oracle source\) is there, so the price of each asset is reported automatically by our bot on a daily basis. 

Ropsten on the other hand, doesn't have a pricer system setup, we will need to manually set expiry price to allow "settlements". If you need a historical price for a specific otoken and it's not written in the Oracle, reach out to us on Discord and we will have you fill in the blank. 

But the great thing about Ropsten is it supports 0x APIs, and we're running market making program to provide liquidity through 0x API. So if you're project require sourcing liquidity from the market, Ropsten may be a better choice.

## Mainnet

#### Core Modules

| Module | Address |
| :--- | :--- |
| AddressBook | [0x1E31F2DCBad4dc572004Eae6355fB18F9615cBe4](https://etherscan.io/address/0x1E31F2DCBad4dc572004Eae6355fB18F9615cBe4) |
| OtokenFactory | [0x7C06792Af1632E77cb27a558Dc0885338F4Bdf8E](https://etherscan.io/address/0x7C06792Af1632E77cb27a558Dc0885338F4Bdf8E) |
| Otoken | [0x7C91794b65eB573c3702229009AcD3CDe712146D](https://etherscan.io/address/0x7C91794b65eB573c3702229009AcD3CDe712146D) |
| Whitelist | [0xa5EA18ac6865f315ff5dD9f1a7fb1d41A30a6779](https://etherscan.io/address/0xa5EA18ac6865f315ff5dD9f1a7fb1d41A30a6779) |
| Oracle | [0xc497f40D1B7db6FA5017373f1a0Ec6d53126Da23](https://etherscan.io/address/0xc497f40D1B7db6FA5017373f1a0Ec6d53126Da23) |
| MarginPool | [0x5934807cC0654d46755eBd2848840b616256C6Ef](https://etherscan.io/address/0x5934807cC0654d46755eBd2848840b616256C6Ef) |
| MarginCalculator | [0x7A48d10f372b3D7c60f6c9770B91398e4ccfd3C7](https://etherscan.io/address/0x7A48d10f372b3D7c60f6c9770B91398e4ccfd3C7) |
| Controller | [0x4ccc2339F87F6c59c6893E1A678c2266cA58dC72](https://etherscan.io/address/0x4ccc2339F87F6c59c6893E1A678c2266cA58dC72) |
| Payable Proxy \(operator\) | [0x8f7Dd610c457FC7Cb26B0f9Db4e77581f94F70aC](https://etherscan.io/address/0x8f7Dd610c457FC7Cb26B0f9Db4e77581f94F70aC) |

#### External Contract, Pricers, Callees

| Contract | Address |
| :--- | :--- |
| USDC | [0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48](https://etherscan.io/token/0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48) |
| WETH | [0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2](https://etherscan.io/token/0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2) |
| WBTC | [0x2260fac5e5542a773aa44fbcfedf7c193bc2c599](https://etherscan.io/token/0x2260fac5e5542a773aa44fbcfedf7c193bc2c599) |
| WETH Pricer | [0x4fb83a2e169c9bca5504709ccf97679822f1469f](https://etherscan.io/address/0x4fb83a2e169c9bca5504709ccf97679822f1469f) |
| WBTC Pricer | [0x5faca6df39c897802d752dfcb8c02ea6959245fc](https://etherscan.io/address/0x5faca6df39c897802d752dfcb8c02ea6959245fc#readContract) |
| Permit Callee | [0x3c0638bb4b2bec6d89c09ab4a7f9e21e4586189b](https://etherscan.io/address/0x3c0638bb4b2bec6d89c09ab4a7f9e21e4586189b#code) |

## Ropsten 

#### Core Modules

| Module | Address |
| :--- | :--- |
| AddressBook | [0xE71417EEfC794C9B83Fc494861981721e26db0E9](https://ropsten.etherscan.io/address/0xE71417EEfC794C9B83Fc494861981721e26db0E9) |
| OtokenFactory |  [0x8d6994b701F480C27757c5FE2bd93d5352160081](https://ropsten.etherscan.io/address/0x8d6994b701f480c27757c5fe2bd93d5352160081) |
| Otoken | [0x2E03749e6eA3Cb3E5bddf22975d93f2F081c9aE3](https://ropsten.etherscan.io/address/0x2E03749e6eA3Cb3E5bddf22975d93f2F081c9aE3) |
| Whitelist | [0x5faCA6DF39c897802d752DfCb8c02Ea6959245Fc](https://ropsten.etherscan.io/address/0x5faCA6DF39c897802d752DfCb8c02Ea6959245Fc) |
| Oracle | [0x55a50C75c7f82943DC4755B2964f4F3F6aB5d5AF](https://ropsten.etherscan.io/address/0x55a50C75c7f82943DC4755B2964f4F3F6aB5d5AF) |
| MarginPool |  [0x3C325EeBB64495665F5376930d30151C1075bFD8](https://ropsten.etherscan.io/address/0x3C325EeBB64495665F5376930d30151C1075bFD8) |
| MarginCalculator |  [0x48DAd1a9e38Ff941429F1542e1Cf552e647306bB](https://ropsten.etherscan.io/address/0x48DAd1a9e38Ff941429F1542e1Cf552e647306bB) |
| Controller | [0x7e9beaccdccee88558aaa2dc121e52ec6226864e](https://ropsten.etherscan.io/address/0x7e9beaccdccee88558aaa2dc121e52ec6226864e) |
| Payable Proxy \(operator\) | [0x0Da6280D0837292B7A1f27FC602C7e0bD3ce0b66](https://ropsten.etherscan.io/address/0x0Da6280D0837292B7A1f27FC602C7e0bD3ce0b66) |

#### External Contract and Pricers

| Contract | Address |
| :--- | :--- |
| USDC | [0x27415c30d8c87437BeCbd4f98474f26E712047f4](https://ropsten.etherscan.io/address/0x27415c30d8c87437BeCbd4f98474f26E712047f4) |
| WETH | [0xc778417e063141139fce010982780140aa0cd5ab](https://ropsten.etherscan.io/token/0xc778417e063141139fce010982780140aa0cd5ab) |
| WBTC | [0xe477d1FFC1e5eA6a577846a4699617997315B4ee](https://ropsten.etherscan.io/address/0xe477d1FFC1e5eA6a577846a4699617997315B4ee) |
|  |  |



## Kovan

_Note that 0x is deprecated on Kovan, so our kovan testnet won't allow creating, buying, or selling orders. We recommend testing with Ropsten._

#### Core Modules

| Module | Address |
| :--- | :--- |
| AddressBook | [0x8812f219f507e8cfe9d2f1e790164714c5e06a73](https://kovan.etherscan.io/address/0x8812f219f507e8cfe9d2f1e790164714c5e06a73) |
| OtokenFactory | [0xb9d17ab06e27f63d0fd75099d5874a194ee623e2](https://kovan.etherscan.io/address/0xb9d17ab06e27f63d0fd75099d5874a194ee623e2)  |
| Otoken | [0x5799d2f95e113806dd5b6e8c41a5be9d7f6a72b3](https://kovan.etherscan.io/address/0x5799d2f95e113806dd5b6e8c41a5be9d7f6a72b3) |
| Whitelist | [0x9164eb40a1b59512f1803ab4c2d1de4b89627a93](https://kovan.etherscan.io/address/0x9164eb40a1b59512f1803ab4c2d1de4b89627a93)  |
| Oracle | [0x853546c909ec0b9443e922be414042cbd8087b93](https://kovan.etherscan.io/address/0x853546c909ec0b9443e922be414042cbd8087b93) |
| MarginPool | [0x8c7c60d766951c5c570bbb7065c993070061b795](https://kovan.etherscan.io/address/0x8c7c60d766951c5c570bbb7065c993070061b795)  |
| MarginCalculator | [0xd5b384081dbc4e65e69ebf25ed70f522e35fa740](https://kovan.etherscan.io/address/0xd5b384081dbc4e65e69ebf25ed70f522e35fa740)  |
| Controller | [0xdee7d0f8ccc0f7ac7e45af454e5e7ec1552e8e4e](https://kovan.etherscan.io/address/0xdee7d0f8ccc0f7ac7e45af454e5e7ec1552e8e4e) |
| Payable Proxy \(operator\) | [0x5957A413f5Ac4Bcf2ba7c5c461a944b548ADB1A5](https://kovan.etherscan.io/address/0x5957A413f5Ac4Bcf2ba7c5c461a944b548ADB1A5) |

#### External Contract and Pricers

| Contract | Address |
| :--- | :--- |
| USDC | [0x7e6edA50d1c833bE936492BF42C1BF376239E9e2](https://kovan.etherscan.io/address/0x7e6edA50d1c833bE936492BF42C1BF376239E9e2) |
| WETH | [0xd0a1e359811322d97991e03f863a0c30c2cf029c](https://kovan.etherscan.io/address/0xd0a1e359811322d97991e03f863a0c30c2cf029c) |
| WBTC | [0x50570256f0da172a1908207aAf0c80d4b279f303](https://kovan.etherscan.io/address/0x50570256f0da172a1908207aAf0c80d4b279f303) |
| WETH Pricer | [0x3c4E935298509BE85470d60E961e29D83221d6Ae](https://kovan.etherscan.io/address/0x3c4E935298509BE85470d60E961e29D83221d6Ae) |
| WBTC Pricer | [0xc802D58aA02fD9d305107d94c92C3E4438867329](https://kovan.etherscan.io/address/0xc802D58aA02fD9d305107d94c92C3E4438867329) |


