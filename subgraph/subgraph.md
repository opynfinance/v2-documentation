# Common Subgraph Queries

Opyn uses a subgraph to index data from the Gamma Protocol smart contracts. The subgraph organizes data about user vaults, token balances, oTokens, and oracles. The subgraph updates any time a transaction is made, and it is running on [The Graph](https://thegraph.com/) protocol’s hosted service and can be openly queried. Here we outline common subgraph queries.

## Resources

* Subgraph Explorer: sandbox for querying data and endpoints for developers.
  * [Mainnet](https://thegraph.com/explorer/subgraph/opynfinance/gamma-mainnet) 
  * [Kovan](https://thegraph.com/explorer/subgraph/opynfinance/gamma-kovan)
  * [Ropsten](https://thegraph.com/explorer/subgraph/opynfinance/gamma-ropsten) 
* [Gamma Subgraph Repo](https://github.com/opynfinance/Gamma-Subgraph):  source code for deployed subgraph. 
* [The Graph’s documentation](https://thegraph.com/docs/introduction): documentation to learn about making queries

## Example Queries

### Query Historical oToken Prices 

🔗[Subgraph Explorer Query Link](https://thegraph.com/explorer/subgraph/opynfinance/gamma-mainnet?query=Token%20Historical%20Price)

You can find historical prices by inputting a given oToken address and view the following for past trades: 

* `buyer`: buyer address 
* `seller`: seller address 
* `oTokenAmount`: the amount of oTokens traded \(scale by [appropriate decimals](https://opyn.gitbook.io/opyn/get-started/protocol-math#otokens-underlying-and-collateral-decimals) to get amount\)
* oToken 
  * `symbol`: the oToken's symbol 
* `paymentTokenAmount`: the amount of the payment token \(in this case USDC\) paid for the oToken \(scale by [appropriate decimals](https://opyn.gitbook.io/opyn/get-started/protocol-math#otokens-underlying-and-collateral-decimals) to get amount\)
* `timestamp`: the UTC unix timestamp at which the trade occurred 

{% tabs %}
{% tab title="GraphQL" %}
```graphql
{
  otokenTrades(where: {oToken_contains: "0xbc006932b24011d315759f17cfc8745c9df48c82"}) {
    buyer
    seller
    oTokenAmount
    oToken {
      symbol
    }
    paymentTokenAmount
    timestamp
  }
}
```
{% endtab %}
{% endtabs %}

### Query Vaults by Account 

🔗[Subgraph Explorer Query Link](https://thegraph.com/explorer/subgraph/opynfinance/gamma-mainnet?query=User%20Vaults)

You can search for any given account's vaults and number of vaults by inputting their address as the `id`. For each vault, you can view the following: 

* `id`: the [`vaultId`](https://github.com/opynfinance/GammaProtocol/blob/9a75da2ad8beefdaa4caa97d17799b50552ca450/contracts/libs/Actions.sol#L39) of the queried vault
* collateralAsset
  * `id`: the address of the asset 
  * `symbol`: the asset's symbol 
  * `decimals`: the number of [decimals](https://opyn.gitbook.io/opyn/get-started/protocol-math#otokens-underlying-and-collateral-decimals) for the asset
* `collateralAmount`: the amount of collateral in the vault \(scale by [appropriate decimals](https://opyn.gitbook.io/opyn/get-started/protocol-math#otokens-underlying-and-collateral-decimals) to get amount\)
* shortOToken
  * `id`: the address of the oToken 
  * `symbol`: the oToken's symbol 
  * `decimals`: the number of [decimals](https://opyn.gitbook.io/opyn/get-started/protocol-math#otokens-underlying-and-collateral-decimals) for the oToken
* `shortAmount`: the amount of short oTokens in the vault \(scale by [appropriate decimals](https://opyn.gitbook.io/opyn/get-started/protocol-math#otokens-underlying-and-collateral-decimals) to get amount\)
* longOToken
  * `id`: the address of the oToken 
  * `symbol`: the oToken's symbol 
  * `decimals`: the number of [decimals](https://opyn.gitbook.io/opyn/get-started/protocol-math#otokens-underlying-and-collateral-decimals) for the oToken
* `longAmount`: the amount of long oTokens in the vault \(scale by [appropriate decimals](https://opyn.gitbook.io/opyn/get-started/protocol-math#otokens-underlying-and-collateral-decimals) to get amount\)

{% tabs %}
{% tab title="GraphQL" %}
```graphql
{
    account(id: "0xf668606b896389066a39b132741763e1ca6d76a2") {
      vaultCount
      vaults {
        id
        collateralAsset {
          id
          symbol
          decimals
        }
        collateralAmount
        shortOToken{
          id
          symbol
          decimals
        }
        shortAmount
        longOToken {
          id
          symbol
          decimals
        }
        longAmount
      }
    }
  }
```
{% endtab %}
{% endtabs %}

### Query Buys & Sells by Account 

🔗[Subgraph Explorer Query Link](https://thegraph.com/explorer/subgraph/opynfinance/gamma-mainnet?query=Account%20Buy%20and%20Sell)

You can find all buys or sells for a given address with the below query, giving you the following information: 

* oToken 
  * `symbol`: oToken's symbol 
  * `id`: oToken's address 
* `buyer`: buyer's address
* `oTokenAmount`: the amount of oTokens traded \(scale by [appropriate decimals](https://opyn.gitbook.io/opyn/get-started/protocol-math#otokens-underlying-and-collateral-decimals) to get amount\)
* `paymentTokenAmount`: the amount of the payment token \(in this case USDC\) paid for the oToken \(scale by [appropriate decimals](https://opyn.gitbook.io/opyn/get-started/protocol-math#otokens-underlying-and-collateral-decimals) to get amount\)
* paymentToken 
  * `decimals`: the number of [decimals](https://opyn.gitbook.io/opyn/get-started/protocol-math#otokens-underlying-and-collateral-decimals) for the oToken
  * `id`: payment token address 
* `exchange`: the name of the exchange where the oTokens traded eg. "ZeroX"

{% tabs %}
{% tab title="GraphQL" %}
```graphql
{
  buys: otokenTrades(where: {buyer: "0x200aabfdb21beb86250ffe93cac78dc9b9fa3e7d"}) {
    oToken {
      symbol
      id
    }
    buyer
    oTokenAmount
    paymentTokenAmount
    paymentToken {
      decimals
      id
    }
    exchange
  }
  sells: otokenTrades(where: {seller: "0x200aabfdb21beb86250ffe93cac78dc9b9fa3e7d"}) {
    oToken {
      symbol
      id
    }
    oTokenAmount
    paymentTokenAmount
    paymentToken {
      decimals
      id
    }
    exchange
  }
}

```
{% endtab %}
{% endtabs %}

### Query all oTokens 

🔗[Subgraph Explorer Query Link](https://thegraph.com/explorer/subgraph/opynfinance/gamma-mainnet?query=oTokens)

You can query all oTokens and for each oToken you can view the following: 

* `id`: the oToken's address
* `symbol`: the oToken's symbol
* `name`: the oToken's name 
* StrikeAsset
  * `id`: the address of the asset 
  * `symbol`: the asset's symbol 
  * `decimals`: the number of [decimals](https://opyn.gitbook.io/opyn/get-started/protocol-math#otokens-underlying-and-collateral-decimals) for the asset
* UnderlyingAsset
  * `id`: the address of the asset 
  * `symbol`: the asset's symbol 
  * `decimals`: the number of [decimals](https://opyn.gitbook.io/opyn/get-started/protocol-math#otokens-underlying-and-collateral-decimals) for the asset
* CollateralAsset
  * `id`: the address of the asset 
  * `symbol`: the asset's symbol 
  * `decimals`: the number of [decimals](https://opyn.gitbook.io/opyn/get-started/protocol-math#otokens-underlying-and-collateral-decimals) for the asset
* `strikePrice`: the oToken's strike price 
* `isPut`: true if the oToken is a put option, false if a call
* `expiryTimestamp`: UTC unix timestamp of when the oToken expires

{% tabs %}
{% tab title="GraphQL" %}
```graphql
{
    otokens {
      id
      symbol
      name
      strikeAsset {
        id
        symbol
        decimals
      }
      underlyingAsset {
        id
        symbol
        decimals
      }
      collateralAsset {
        id
        symbol
        decimals
      }
      strikePrice
      isPut
      expiryTimestamp
    }
  }
```
{% endtab %}
{% endtabs %}

### Query all Whitelisted Products

🔗[Subgraph Explorer Query Link](https://thegraph.com/explorer/subgraph/opynfinance/gamma-mainnet?query=Products)

You can query all whitelisted products and for each product you can view the following: 

* `id`: the oToken's address
* Strike
  * `id`: the address of the asset 
  * `symbol`: the asset's symbol 
  * `decimals`: the number of [decimals](https://opyn.gitbook.io/opyn/get-started/protocol-math#otokens-underlying-and-collateral-decimals) for the asset
* Underlying
  * `id`: the address of the asset 
  * `symbol`: the asset's symbol 
  * `decimals`: the number of [decimals](https://opyn.gitbook.io/opyn/get-started/protocol-math#otokens-underlying-and-collateral-decimals) for the asset
* Collateral
  * `id`: the address of the asset 
  * `symbol`: the asset's symbol 
  * `decimals`: the number of [decimals](https://opyn.gitbook.io/opyn/get-started/protocol-math#otokens-underlying-and-collateral-decimals) for the asset
* `isPut`: true if the oToken is a put option, false if a call

{% tabs %}
{% tab title="GraphQL" %}
```graphql
{
    whitelistedProducts (where:{isWhitelisted: true}) {
      id
      strike {
        symbol
        id
        decimals
      }
      underlying {
        symbol
        id
        decimals
      }
      collateral {
        symbol
        id
        decimals
      }
      isPut
    }
  }
```
{% endtab %}
{% endtabs %}



