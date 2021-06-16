# Subgraph

This section explains the Gamma Subgraph and how to interact with it. The Gamma subgraph indexes data from the Gamma contracts over time. It organizes data about user vaults, tokens balances, and oTokens as a whole, and more. The subgraph updates any time a transaction is made, and it is running on [The Graph](https://thegraph.com/) protocol’s hosted service and can be openly queried.

### Resources <a id="resources"></a>

1. Subgraph Explorer:  sandbox for querying data and endpoints for developers.
   * [Mainnet](https://thegraph.com/explorer/subgraph/opynfinance/gamma-mainnet) 
   * [Kovan](https://thegraph.com/explorer/subgraph/antoncoding/gamma-kovan-new)
   * [Ropsten](https://thegraph.com/explorer/subgraph/opynfinance/gamma-ropsten)
2. [Gamma Subgraph Repo ](https://github.com/opynfinance/Gamma-Subgraph):  source code for deployed subgraph.

### Example Queries

#### Query Account Vault 

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

#### Query all oTokens 

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

#### Query all whitelisted product

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

To learn more about querying a subgraph refer to [The Graph’s documentation](https://thegraph.com/docs/introduction).

