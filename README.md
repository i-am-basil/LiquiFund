# Delta neutral strategy: ETH Gamma Scalping Strategy

## Assumptions

1. **No slippage**: The market is efficient enough to allow a slippage of zero.
2. **Liquidity**: Quick entry and exit without significant price impact.
3. **Transaction costs**: Transaction costs associated with DEX fees are set to 0.3%
4. **BS-model assumptions**: We imply that option premiums are calculated using the Black-Scholes model. Thus, we imply all assumptions associated with the BS-model. Note that these assumptions can be relaxed, if option premiums are captured from real-market data 


## Modeling the Strategy

Gamma scalping is a delta-neutral trading strategy that involves dynamically adjusting a position in the underlying asset to maintain a neutral delta while capturing profits from changes in volatility.

At inception:
1. **Short an ETH option**. Every monday at 00:00:00 short an option expiring in 7 days
2. **Calculate delta**. Derive the sensitiviy of an option to an underlying
3. **Buy ETH**. Buy delta worth of ETH

During tenor:
1. **Recalculate delta**. Recalculate the new delta of an option
2. **Adjust ETH position**. Buy/sell ETH in order to perserve delta-neutrality


## Potential Risks

1. **Delta Risk**: Big jumps in delta can lead to deviations from delta neutrality
2. **Execution Risk**: Delays in executing trades can result in slippage and reduced profitability.
4. **Model Risk**: The strategy relies havely on the accuracy of the models used for delta calculations, they might not be effective

## Results

![Delta Neutral Strategy](delta_neutral_strategy.png)

### Highlights

- Average Daily Return: **0.0013**
- Value at Risk (VaR) at 95% Confidence Level: **-0.0023**
- Conditional Value at Risk (CVaR): **-0.0058**
- Hypothesis test against 20% APY:
    - Z-Score for Hypothesis Test: **12.8520**
    - P-Value for Hypothesis Test: **<0.0001**

Based on the results, we can **reject the null hypothesis**, indicating that the strategy does achieve at least **20% APY on average**.


# DEX Yield strategy: WBTC/USDC liquidity provision

## Modeling the Strategy

A yield strategy involves earning returns by placing cryptocurrency assets in liquidity pools or decentralized finance (DeFi) platforms. DEXs use this funds in order to provide the market for traders, which in return pay a fee that is distributed among LPs based on their contribution. 

To calculate the exact fees, I need to obtain total liquidity pool volume, which is not provided by DEXs. However, we can do the following

1. **Scenario Analysis**: I construct a grid of price changes and attainable fees
2. **Impairment loss**: Impairment loss risk occurs when the value of deposited assets declines due to market volatility. Liquidity providers may face losses upon withdrawal, especially with volatile pairs, compounded by impermanent loss. In our case, I model the imparement loss as follows

$$
IL = \frac{2  \sqrt{price\text{ }change}}{1 + price\text{ }change} - 1
$$

3. **Return Calculation**: The final investment value is calculated based on the initial investment, adjusted for trading fees and price changes.


## Results

![Yield Strategy Results](yield_strategy.png)

As anticipated, greater deviations from the initial price lead to a more significant reduction in earned fees due to impermanent loss.

However, the key factor in this strategy is the liquidity pool pair. The [WBTC/USDC](https://app.uniswap.org/explore/pools/polygon/0xeEF1A9507B3D505f0062f2be9453981255b503c8) pair offers the highest yield on Uniswap, making it a strong choice.