# Uniswap Triangular Arbitrage Python
## Intro
- The code is similar to [poloniex triangular arbitrage](https://github.com/chickensmitten/poloniex-triangular-arbitrage). Refer to it for more information
- This code only gets and sort the data into JSON. It requires [Uniswap Triangular Arbitrage JS](https://github.com/chickensmitten/uniswap-triangular-arbitrage) to get the final calculation for profitability
- Because the prices from uniswap are already inverted, there is no need to do it in the code. Refer to 
`swap_1_rate = 1 / a_ask` in poloniex triangular arbitrage vs `swap_1_rate = a_token_1_price` in this codebase.
- [Uniswap contracts and their addresses](https://docs.uniswap.org/contracts/v3/reference/deployments)
- Liquidity of some tokens in Uniswap is generally low, making it unprofitable to try triangular arbitrage
- [Uniswap GraphQL playground](https://thegraph.com/hosted-service/subgraph/uniswap/uniswap-v3). The code below is to get the first 500 tokens ordered by Total Value Locked in ETH.
```
  query = """
    {
      pools (orderBy: totalValueLockedETH, 
        orderDirection: desc,
        first:500) 
        {
          id
          totalValueLockedETH
          token0Price
          token1Price
          feeTier
          token0 {id symbol name decimals}
          token1 {id symbol name decimals}
        }
    }
  """
```

## Instructions
1. Run main.py with `python3 main.py`
2. Once the json file is saved, run `uniswap_depth.py`
   - JSON file contains the tokens and the respective addresses for the token
   ```
   {
      "swap1":"WETH",
      "swap2":"USDC",
      "swap3":"ADS",
      "poolContract1":"0x88e6a0c2ddd26feeb64f039a2c41296fcb3f5640",
      "poolContract2":"0xc0b51c7042be877bedeee2103f3f6667e32bee97",
      "poolContract3":"0xb9044f46dcdea7ecebbd918a9659ba8239bd9f37",
      "poolDirectionTrade1":"quoteToBase",
      "poolDirectionTrade2":"baseToQuote",
      "poolDirectionTrade3":"quoteToBase",
      "startingAmount":1,
      "acquiredCoinT1":1704.0208988544275,
      "acquiredCoinT2":1469.1941764835817,
      "acquiredCoinT3":1.9397143550135487,
      "swap1Rate":1704.0208988544275,
      "swap2Rate":0.862192580778373,
      "swap3Rate":0.001320257312519524,
      "profitLoss":0.9397143550135487,
      "profitLossPerc":93.97143550135488,
      "direction":"reverse",
      "tradeDesc1":"Start with WETH of 1. Swap at 1704.0208988544275 for USDC acquiring 1704.0208988544275.",
      "tradeDesc2":"Swap 1704.0208988544275 of USDC at 0.862192580778373 for ADS acquiring 1469.1941764835817.",
      "tradeDesc3":"Swap 1469.1941764835817 of ADS at 0.001320257312519524 for WETH acquiring 1.9397143550135487."
    }
   ```