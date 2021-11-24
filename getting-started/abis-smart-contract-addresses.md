---
description: >-
  Gamma protocol is currently live on Ethereum Mainnet, Ropsten testnet and
  Kovan testnet.
---

# Smart Contract Addresses

{% hint style="info" %}
Github release of deployed code: [https://github.com/opynfinance/GammaProtocol/releases/tag/v2.0.0](https://github.com/opynfinance/GammaProtocol/releases/tag/v2.0.0)
{% endhint %}

## Mainnet

#### Core Modules

| Module                   | Address                                                                                                               | ABI                                                                                                                    |
| ------------------------ | --------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| AddressBook              | [0x1E31F2DCBad4dc572004Eae6355fB18F9615cBe4](https://etherscan.io/address/0x1E31F2DCBad4dc572004Eae6355fB18F9615cBe4) | [ABI](https://api.etherscan.io/api?module=contract\&action=getabi\&address=0x1E31F2DCBad4dc572004Eae6355fB18F9615cBe4) |
| OtokenFactory            | [0x7C06792Af1632E77cb27a558Dc0885338F4Bdf8E](https://etherscan.io/address/0x7C06792Af1632E77cb27a558Dc0885338F4Bdf8E) | [ABI](https://api.etherscan.io/api?module=contract\&action=getabi\&address=0x7C06792Af1632E77cb27a558Dc0885338F4Bdf8E) |
| Otoken                   | [0x7C91794b65eB573c3702229009AcD3CDe712146D](https://etherscan.io/address/0x7C91794b65eB573c3702229009AcD3CDe712146D) | [ABI](https://api.etherscan.io/api?module=contract\&action=getabi\&address=0x7C91794b65eB573c3702229009AcD3CDe712146D) |
| Whitelist                | [0xa5EA18ac6865f315ff5dD9f1a7fb1d41A30a6779](https://etherscan.io/address/0xa5EA18ac6865f315ff5dD9f1a7fb1d41A30a6779) | [ABI](https://api.etherscan.io/api?module=contract\&action=getabi\&address=0xa5EA18ac6865f315ff5dD9f1a7fb1d41A30a6779) |
| Oracle                   | [0x789cD7AB3742e23Ce0952F6Bc3Eb3A73A0E08833](https://etherscan.io/address/0x789cD7AB3742e23Ce0952F6Bc3Eb3A73A0E08833) | [ABI](https://api.etherscan.io/api?module=contract\&action=getabi\&address=0xc497f40D1B7db6FA5017373f1a0Ec6d53126Da23) |
| MarginPool               | [0x5934807cC0654d46755eBd2848840b616256C6Ef](https://etherscan.io/address/0x5934807cC0654d46755eBd2848840b616256C6Ef) | [ABI](https://api.etherscan.io/api?module=contract\&action=getabi\&address=0x5934807cC0654d46755eBd2848840b616256C6Ef) |
| MarginCalculator         | [0xfaa67e3736572645B38AF7410B3E1006708e13F4](https://etherscan.io/address/0xfaa67e3736572645B38AF7410B3E1006708e13F4) | [ABI](https://api.etherscan.io/api?module=contract\&action=getabi\&address=0x7A48d10f372b3D7c60f6c9770B91398e4ccfd3C7) |
| Controller proxy         | [0x4ccc2339F87F6c59c6893E1A678c2266cA58dC72](https://etherscan.io/address/0x4ccc2339F87F6c59c6893E1A678c2266cA58dC72) | [ABI](https://api.etherscan.io/api?module=contract\&action=getabi\&address=0xae1e3ac020ddec3d20c8da5532002fdac62e9f86) |
| Payable Proxy (operator) | [0x8f7Dd610c457FC7Cb26B0f9Db4e77581f94F70aC](https://etherscan.io/address/0x8f7Dd610c457FC7Cb26B0f9Db4e77581f94F70aC) | [ABI](https://api.etherscan.io/api?module=contract\&action=getabi\&address=0x8f7Dd610c457FC7Cb26B0f9Db4e77581f94F70aC) |

#### External Contract, Pricers, Callees

| Contract             | Address                                                                                                                            | ABI                                                                                                                    |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| USDC                 | [0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48](https://etherscan.io/token/0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48)                | [ABI](https://api.etherscan.io/api?module=contract\&action=getabi\&address=0xa2327a938febf5fec13bacfb16ae10ecbc4cbdcf) |
| WETH                 | [0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2](https://etherscan.io/token/0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2)                | [ABI](https://api.etherscan.io/api?module=contract\&action=getabi\&address=0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2) |
| WBTC                 | [0x2260fac5e5542a773aa44fbcfedf7c193bc2c599](https://etherscan.io/token/0x2260fac5e5542a773aa44fbcfedf7c193bc2c599)                | [ABI](https://api.etherscan.io/api?module=contract\&action=getabi\&address=0x2260fac5e5542a773aa44fbcfedf7c193bc2c599) |
| WETH Pricer          | [0x128cE9B4D97A6550905dE7d9Abc2b8C747b0996C](https://etherscan.io/address/0x128cE9B4D97A6550905dE7d9Abc2b8C747b0996C)              | -                                                                                                                      |
| WBTC Pricer          | [0x32363cAB91EEaad10BFdd0b6Cd013CAF2595E85d](https://etherscan.io/address/0x32363cAB91EEaad10BFdd0b6Cd013CAF2595E85d)              | -                                                                                                                      |
| Permit Callee        | [0x3c0638bb4b2bec6d89c09ab4a7f9e21e4586189b](https://etherscan.io/address/0x3c0638bb4b2bec6d89c09ab4a7f9e21e4586189b#code)         | [ABI](https://api.etherscan.io/api?module=contract\&action=getabi\&address=0x3c0638bb4b2bec6d89c09ab4a7f9e21e4586189b) |
| wstETH Pricer        | [0x4661951D252993AFa69b36bcc7Ba7da4a48813bF](https://etherscan.io/address/0x4661951d252993afa69b36bcc7ba7da4a48813bf#code)         |                                                                                                                        |
| sdecrv Pricer        | [0x27a8ee0Eb39AEe580490da00ab60eCfAB2a02C40](https://etherscan.io/address/0x27a8ee0Eb39AEe580490da00ab60eCfAB2a02C40)              |                                                                                                                        |
| yUSDC (old) Pricerv  | [0x7494Ec6D7A9a9e67774Cc9B8d3aEA26A8EB59db3](https://etherscan.io/address/0x7494Ec6D7A9a9e67774Cc9B8d3aEA26A8EB59db3)              |                                                                                                                        |
| yUSDC (new) Pricer   | [0x11Ac0c63d64cDd95C593322B8381AaFF9c086a04](https://etherscan.io/address/0x11Ac0c63d64cDd95C593322B8381AaFF9c086a04#code)         |                                                                                                                        |
| sdFrax3Crv Pricer    | [0xAF751EDCbB35Beb33c945BD625eB008Cd37b35d3](https://etherscan.io/address/0xaf751edcbb35beb33c945bd625eb008cd37b35d3)              |                                                                                                                        |
| sdWbtc Pricer        | [0x4c65680554C35c27DDDb2F276f95225953513401](https://etherscan.io/address/0x4c65680554C35c27DDDb2F276f95225953513401)              |                                                                                                                        |
| AAVE Pricer          | [0x204e2F3264B5200BCF0d9AC1c466CafcFa5df182](https://etherscan.io/address/0x204e2F3264B5200BCF0d9AC1c466CafcFa5df182#readContract) |                                                                                                                        |
| SUSHI Pricer         | [0xE81462E3A2dC9696F678FcCF3402ec135b5E6AB3](https://etherscan.io/address/0xe81462e3a2dc9696f678fccf3402ec135b5e6ab3)              |                                                                                                                        |
| UNI Pricer           | [0xb05Eaa89915Ce22F35eF44bf9A9e168DF5a51fa4](https://etherscan.io/address/0xb05Eaa89915Ce22F35eF44bf9A9e168DF5a51fa4)              |                                                                                                                        |
| DPI Pricer           | [0x6dEc830C44de056041Fa0616C068FE5dde385416](https://etherscan.io/address/0x6dEc830C44de056041Fa0616C068FE5dde385416)              |                                                                                                                        |

## Arbitrum One

Gamma on Arbitrum One is currently in beta release. Use at your own risk!&#x20;

#### Core Modules

| Module                   | Address                                                                                                              | ABI                                                                                                                    |
| ------------------------ | -------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| AddressBook              | [0x9a33230f59Cc7Cc9A084E0098A2b2934FC7BF7c0](https://arbiscan.io/address/0x9a33230f59Cc7Cc9A084E0098A2b2934FC7BF7c0) | [ABI](https://api.etherscan.io/api?module=contract\&action=getabi\&address=0x1E31F2DCBad4dc572004Eae6355fB18F9615cBe4) |
| OtokenFactory            | [0x4D3a52A0e98144CAf46Ac226d83e8f144b5c654D](https://arbiscan.io/address/0x4D3a52A0e98144CAf46Ac226d83e8f144b5c654D) | [ABI](https://api.etherscan.io/api?module=contract\&action=getabi\&address=0x7C06792Af1632E77cb27a558Dc0885338F4Bdf8E) |
| Otoken                   | [0xCFbaaF567b7B64bF129f02Db7360eCd795B67F4A](https://arbiscan.io/address/0xCFbaaF567b7B64bF129f02Db7360eCd795B67F4A) | [ABI](https://api.etherscan.io/api?module=contract\&action=getabi\&address=0x7C91794b65eB573c3702229009AcD3CDe712146D) |
| Whitelist                | [0xB8f0AC1Ab70643ebE8103Db3618EA5eD6901B458](https://arbiscan.io/address/0xB8f0AC1Ab70643ebE8103Db3618EA5eD6901B458) | [ABI](https://api.etherscan.io/api?module=contract\&action=getabi\&address=0xa5EA18ac6865f315ff5dD9f1a7fb1d41A30a6779) |
| Oracle                   | [0x7A1E6f0f07Ee2DddE14Cd4B8Eb582BAd065357C5](https://arbiscan.io/address/0x7A1E6f0f07Ee2DddE14Cd4B8Eb582BAd065357C5) | [ABI](https://api.etherscan.io/api?module=contract\&action=getabi\&address=0xc497f40D1B7db6FA5017373f1a0Ec6d53126Da23) |
| MarginPool               | [0x63d8d20606c048B9B79A30ea45Ca6787F8aEB051](https://arbiscan.io/address/0x63d8d20606c048B9B79A30ea45Ca6787F8aEB051) | [ABI](https://api.etherscan.io/api?module=contract\&action=getabi\&address=0x5934807cC0654d46755eBd2848840b616256C6Ef) |
| MarginCalculator         | [0xC9F007D6F0aa2b6C5f0E4c0Ff79273227C2100A9](https://arbiscan.io/address/0xC9F007D6F0aa2b6C5f0E4c0Ff79273227C2100A9) | [ABI](https://api.etherscan.io/api?module=contract\&action=getabi\&address=0x7A48d10f372b3D7c60f6c9770B91398e4ccfd3C7) |
| Controller proxy         | [0xeE30f92cc9Bf896679567d1aCD551f0E179756fC](https://arbiscan.io/address/0xeE30f92cc9Bf896679567d1aCD551f0E179756fC) | [ABI](https://api.etherscan.io/api?module=contract\&action=getabi\&address=0xae1e3ac020ddec3d20c8da5532002fdac62e9f86) |
| Payable Proxy (operator) | [0x91332064B2aB742eFBB0Ee416895dffB5fA85053](https://arbiscan.io/address/0x91332064B2aB742eFBB0Ee416895dffB5fA85053) | [ABI](https://api.etherscan.io/api?module=contract\&action=getabi\&address=0x8f7Dd610c457FC7Cb26B0f9Db4e77581f94F70aC) |

#### External Contract, Pricers, Callees

No pricer has been deployed with Arbitrum One! The protocol might not be able to settle options on Arbitrum One at the moment. Contact us if you want to start building on Arbitrum One.&#x20;

| Contract   | Address                                                                                                              | ABI                                                                                                                    |
| ---------- | -------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| USDC       | [0xff970a61a04b1ca14834a43f5de4533ebddb5cc8](https://arbiscan.io/address/0xff970a61a04b1ca14834a43f5de4533ebddb5cc8) | [ABI](https://api.etherscan.io/api?module=contract\&action=getabi\&address=0xa2327a938febf5fec13bacfb16ae10ecbc4cbdcf) |
| WETH       | [0x82af49447d8a07e3bd95bd0d56f35241523fbab1](https://arbiscan.io/address/0x82af49447d8a07e3bd95bd0d56f35241523fbab1) | [ABI](https://api.etherscan.io/api?module=contract\&action=getabi\&address=0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2) |
| WBTC       | [0x2f2a2543b76a4166549f7aab2e75bef0aefc5b0f](https://arbiscan.io/address/0x2f2a2543b76a4166549f7aab2e75bef0aefc5b0f) | [ABI](https://api.etherscan.io/api?module=contract\&action=getabi\&address=0x2260fac5e5542a773aa44fbcfedf7c193bc2c599) |
| WETHPricer | [0x52b93393BE3600D489c7d3AA74f78516713CD40A](https://arbiscan.io/address/0x52b93393BE3600D489c7d3AA74f78516713CD40A) | -                                                                                                                      |

## Kovan

_Note that 0x is deprecated on Kovan, so our kovan testnet won't allow creating, buying, or selling orders. We recommend testing with Ropsten if you need 0x functionality. _

_If you don't need 0x functionality, Kovan is a better choice, since Chainlink (our oracle source) is on Kovan and will automatically report prices._

🚰[**Testnet Faucet**](https://gammaportal.xyz/#/protocol/faucet/)

#### Core Modules

| Module                   | Address                                                                                                                      | ABI                                                                                                                            |
| ------------------------ | ---------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| AddressBook              | [0x8812f219f507e8cfe9d2f1e790164714c5e06a73](https://kovan.etherscan.io/address/0x8812f219f507e8cfe9d2f1e790164714c5e06a73)  | [ABI](https://api-kovan.etherscan.io/api?module=contract\&action=getabi\&address=0x8812f219f507e8cfe9d2f1e790164714c5e06a73)   |
| OtokenFactory            | [0xb9d17ab06e27f63d0fd75099d5874a194ee623e2](https://kovan.etherscan.io/address/0xb9d17ab06e27f63d0fd75099d5874a194ee623e2)  | [ABI](https://api-kovan.etherscan.io/api?module=contract\&action=getabi\&address=0xb9d17ab06e27f63d0fd75099d5874a194ee623e2)   |
| Otoken                   | [0xc68125707dd556975ee9d266E1844624f3128e77](https://kovan.etherscan.io/address/0xc68125707dd556975ee9d266E1844624f3128e77)  | [ABI](https://api-kovan.etherscan.io/api?module=contract\&action=getabi\&address=0xc68125707dd556975ee9d266E1844624f3128e77)   |
| Whitelist                | [0x9164eb40a1b59512f1803ab4c2d1de4b89627a93](https://kovan.etherscan.io/address/0x9164eb40a1b59512f1803ab4c2d1de4b89627a93)  | [ABI](https://api-kovan.etherscan.io/api?module=contract\&action=getabi\&address=0x9164eb40a1b59512f1803ab4c2d1de4b89627a93)   |
| Oracle                   | [0x32724C61e948892A906f5EB8892B1E7e6583ba1f](https://kovan.etherscan.io/address/0x32724C61e948892A906f5EB8892B1E7e6583ba1f)  | [ABI](https://api-kovan.etherscan.io/api?module=contract\&action=getabi\&address=0x32724C61e948892A906f5EB8892B1E7e6583ba1f)   |
| MarginPool               | [0x8c7c60d766951c5c570bbb7065c993070061b795](https://kovan.etherscan.io/address/0x8c7c60d766951c5c570bbb7065c993070061b795)  | [ABI](https://api-kovan.etherscan.io/api?module=contract\&action=getabi\&address=0x8c7c60d766951c5c570bbb7065c993070061b795)   |
| MarginCalculator         | [0xFf8efB964Fa3219D1563Dd83e7253FC8d2B9c405](https://kovan.etherscan.io/address/0xFf8efB964Fa3219D1563Dd83e7253FC8d2B9c405)  | [ABI](https://api-kovan.etherscan.io/api?module=contract\&action=getabi\&address=0xFf8efB964Fa3219D1563Dd83e7253FC8d2B9c405)   |
| Controller proxy         | [0xdee7d0f8ccc0f7ac7e45af454e5e7ec1552e8e4e](https://kovan.etherscan.io/address/0xdee7d0f8ccc0f7ac7e45af454e5e7ec1552e8e4e)  | [ABI](https://api-ropsten.etherscan.io/api?module=contract\&action=getabi\&address=0xd37752fd2976335fddb2e6a2cf5ffbfa88bf5f05) |
| Payable Proxy (operator) | [0x5957A413f5Ac4Bcf2ba7c5c461a944b548ADB1A5](https://kovan.etherscan.io/address/0x5957A413f5Ac4Bcf2ba7c5c461a944b548ADB1A5)  | [ABI](https://api.etherscan.io/api?module=contract\&action=getabi\&address=0x8f7Dd610c457FC7Cb26B0f9Db4e77581f94F70aC)         |

#### External Contract and Pricers

| Contract    | Address                                                                                                                     | ABI                                                                                                                          |
| ----------- | --------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| USDC        | [0x7e6edA50d1c833bE936492BF42C1BF376239E9e2](https://kovan.etherscan.io/address/0x7e6edA50d1c833bE936492BF42C1BF376239E9e2) | [ABI](https://api-kovan.etherscan.io/api?module=contract\&action=getabi\&address=0x7e6edA50d1c833bE936492BF42C1BF376239E9e2) |
| WETH        | [0xd0a1e359811322d97991e03f863a0c30c2cf029c](https://kovan.etherscan.io/address/0xd0a1e359811322d97991e03f863a0c30c2cf029c) | [ABI](https://api-kovan.etherscan.io/api?module=contract\&action=getabi\&address=0xd0a1e359811322d97991e03f863a0c30c2cf029c) |
| WBTC        | [0x50570256f0da172a1908207aAf0c80d4b279f303](https://kovan.etherscan.io/address/0x50570256f0da172a1908207aAf0c80d4b279f303) | [ABI](https://api-kovan.etherscan.io/api?module=contract\&action=getabi\&address=0x50570256f0da172a1908207aAf0c80d4b279f303) |
| WETH Pricer | [0xE21eAD94B274719203F2945C46586359bdcd0F83](https://kovan.etherscan.io/address/0xE21eAD94B274719203F2945C46586359bdcd0F83) | -                                                                                                                            |
| WBTC Pricer | [0x5224ACaA8d841e6182C15E42F04c2a10fA7B8aBe](https://kovan.etherscan.io/address/0x5224ACaA8d841e6182C15E42F04c2a10fA7B8aBe) | -                                                                                                                            |

## Ropsten&#x20;

_If you need to trade oTokens using 0x, we recommend using Ropsten, since that is the only testnet 0x supports. _

_Note that Chainlink (our oracle source) doesn't support Ropsten, so if you need a historical price for a specific oToken, please reach out to us on _[_Discord_](https://discord.com/invite/2NFdXaE)_._

_Also note that the Ropsten release currently doesn't have partial collateralization as it is not yet updated to the _[_v2.0.0 release_](https://github.com/opynfinance/GammaProtocol/releases/tag/v2.0.0)_. It will be updated in the coming weeks. _

🚰[**Testnet Faucet**](https://gammaportal.xyz/#/protocol/faucet/)

#### Core Modules

| Module                   | Address                                                                                                                        | ABI                                                                                                                            |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------ |
| AddressBook              | [0xE71417EEfC794C9B83Fc494861981721e26db0E9](https://ropsten.etherscan.io/address/0xE71417EEfC794C9B83Fc494861981721e26db0E9)  | [ABI](https://api-ropsten.etherscan.io/api?module=contract\&action=getabi\&address=0xE71417EEfC794C9B83Fc494861981721e26db0E9) |
| OtokenFactory            |  [0x8d6994b701F480C27757c5FE2bd93d5352160081](https://ropsten.etherscan.io/address/0x8d6994b701f480c27757c5fe2bd93d5352160081) | [ABI](https://api-ropsten.etherscan.io/api?module=contract\&action=getabi\&address=0x8d6994b701f480c27757c5fe2bd93d5352160081) |
| Otoken                   | [0x2E03749e6eA3Cb3E5bddf22975d93f2F081c9aE3](https://ropsten.etherscan.io/address/0x2E03749e6eA3Cb3E5bddf22975d93f2F081c9aE3)  | [ABI](https://api-ropsten.etherscan.io/api?module=contract\&action=getabi\&address=0x2E03749e6eA3Cb3E5bddf22975d93f2F081c9aE3) |
| Whitelist                | [0x5faCA6DF39c897802d752DfCb8c02Ea6959245Fc](https://ropsten.etherscan.io/address/0x5faCA6DF39c897802d752DfCb8c02Ea6959245Fc)  | [ABI](https://api-ropsten.etherscan.io/api?module=contract\&action=getabi\&address=0x2E03749e6eA3Cb3E5bddf22975d93f2F081c9aE3) |
| Oracle                   | [0x55a50C75c7f82943DC4755B2964f4F3F6aB5d5AF](https://ropsten.etherscan.io/address/0x55a50C75c7f82943DC4755B2964f4F3F6aB5d5AF)  | [ABI](https://api-ropsten.etherscan.io/api?module=contract\&action=getabi\&address=0x55a50C75c7f82943DC4755B2964f4F3F6aB5d5AF) |
| MarginPool               |  [0x3C325EeBB64495665F5376930d30151C1075bFD8](https://ropsten.etherscan.io/address/0x3C325EeBB64495665F5376930d30151C1075bFD8) | [ABI](https://api-ropsten.etherscan.io/api?module=contract\&action=getabi\&address=0x3C325EeBB64495665F5376930d30151C1075bFD8) |
| MarginCalculator         |  [0x48DAd1a9e38Ff941429F1542e1Cf552e647306bB](https://ropsten.etherscan.io/address/0x48DAd1a9e38Ff941429F1542e1Cf552e647306bB) | [ABI](https://api-ropsten.etherscan.io/api?module=contract\&action=getabi\&address=0x48DAd1a9e38Ff941429F1542e1Cf552e647306bB) |
| Controller proxy         | [0x7e9beaccdccee88558aaa2dc121e52ec6226864e](https://ropsten.etherscan.io/address/0x7e9beaccdccee88558aaa2dc121e52ec6226864e)  | [ABI](https://api-ropsten.etherscan.io/api?module=contract\&action=getabi\&address=0xd37752fd2976335fddb2e6a2cf5ffbfa88bf5f05) |
| Payable Proxy (operator) | [0x0Da6280D0837292B7A1f27FC602C7e0bD3ce0b66](https://ropsten.etherscan.io/address/0x0Da6280D0837292B7A1f27FC602C7e0bD3ce0b66)  | [ABI](https://api.etherscan.io/api?module=contract\&action=getabi\&address=0x8f7Dd610c457FC7Cb26B0f9Db4e77581f94F70aC)         |

#### External Contract and Pricers

| Contract | Address                                                                                                                       | ABI                                                                                                                            |
| -------- | ----------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| USDC     | [0x27415c30d8c87437BeCbd4f98474f26E712047f4](https://ropsten.etherscan.io/address/0x27415c30d8c87437BeCbd4f98474f26E712047f4) | [ABI](https://api-ropsten.etherscan.io/api?module=contract\&action=getabi\&address=0x27415c30d8c87437BeCbd4f98474f26E712047f4) |
| WETH     | [0xc778417e063141139fce010982780140aa0cd5ab](https://ropsten.etherscan.io/token/0xc778417e063141139fce010982780140aa0cd5ab)   | [ABI](https://api-ropsten.etherscan.io/api?module=contract\&action=getabi\&address=0xc778417e063141139fce010982780140aa0cd5ab) |
| WBTC     | [0xe477d1FFC1e5eA6a577846a4699617997315B4ee](https://ropsten.etherscan.io/address/0xe477d1FFC1e5eA6a577846a4699617997315B4ee) | [ABI](https://api-ropsten.etherscan.io/api?module=contract\&action=getabi\&address=0xe477d1FFC1e5eA6a577846a4699617997315B4ee) |

##
