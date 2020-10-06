# Configuration
- pricing.ts
  - Whitelist: WETH, DAI, USDC, USDT, TUSD, cDAI, cUSDC, TAKO, MXX, EBASE, sUSD, MKR, COMP, LINK, ANT, SNX, YFI, yCurv
  - MINIMUM_USD_THRESHOLD_NEW_PAIRS: 5000 USD
  - MINIMUM_LIQUIDITY_THRESHOLD_ETH: 0.5 ETH


# Deployment instruction
1. Grant access to TheGraph  => `graph auth https://api.thegraph.com/deploy/ <ACCESS_TOKEN>`
2. Update subgraph name in package.json `graph deploy <subgraph_name> ...`
3. `yarn codegen`
4. `yarn build`
5. `yarn deploy`

# TakoSwap V2 Subgraph

[TakoSwap](https://uniswap.org/) is a decentralized protocol for automated token exchange on Ethereum.

This subgraph dynamically tracks any pair created by the uniswap factory. It tracks of the current state of TakoSwap contracts, and contains derived stats for things like historical data and USD prices.

- aggregated data across pairs and tokens,
- data on individual pairs and tokens,
- data on transactions
- data on liquidity providers
- historical data on TakoSwap, pairs or tokens, aggregated by day



## Running Locally

Make sure to update package.json settings to point to your own graph account.

## Queries

Below are a few ways to show how to query the uniswap-subgraph for data. The queries show most of the information that is queryable, but there are many other filtering options that can be used, just check out the [querying api](https://thegraph.com/docs/graphql-api). These queries can be used locally or in The Graph Explorer playground.

## Key Entity Overviews

#### TakoSwapFactory

Contains data across all of TakoSwap V2. This entity tracks important things like total liquidity (in ETH and USD, see below), all time volume, transaction count, number of pairs and more.

#### Token

Contains data on a specific token. This token specific data is aggregated across all pairs, and is updated whenever there is a transaction involving that token.

#### Pair

Contains data on a specific pair.

#### Transaction

Every transaction on TakoSwap is stored. Each transaction contains an array of mints, burns, and swaps that occured within it.

#### Mint, Burn, Swap

These contain specifc information about a transaction. Things like which pair triggered the transaction, amounts, sender, recipient, and more. Each is linked to a parent Transaction entity.

## Example Queries

### Querying Aggregated TakoSwap Data

This query fetches aggredated data from all uniswap pairs and tokens, to give a view into how much activity is happening within the whole protocol.

```graphql
{
  uniswapFactories(first: 1) {
    pairCount
    totalVolumeUSD
    totalLiquidityUSD
  }
}
```
