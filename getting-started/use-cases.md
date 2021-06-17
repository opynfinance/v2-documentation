# Use Cases

## Ideas to Build

Options are an incredibly versatile financial instrument - in fact you can create [any financial payoff using just put and call options.](https://www.youtube.com/watch?v=rMsu4v-UlkA&feature=youtu.be&ab_channel=MITOpenCourseWare) See the introduction to learn more about [why people use options](https://opyn.gitbook.io/opyn/getting-started/introduction#why-do-people-use-options). 

We're excited to see what you'll build! Here are some ideas to get started:

{% hint style="info" %}
Join `#dev on` [**Discord**](https://discord.gg/ugAv3SH) to jam on these ideas and more!
{% endhint %}

### **Hedging for Uniswap LPs** <a id="2b8e"></a>

Uniswap LPs can help reduce impermanent loss in ETH:Stablecoin pools using [straddles](https://www.investopedia.com/terms/s/straddle.asp) \(put and call with same strike\) and [strangles](https://www.investopedia.com/terms/s/strangle.asp) \(put and call with different strike\).

### Liquidation Saver 

With the [partial collateralization update](https://medium.com/opyn/partially-collateralized-options-now-in-defi-b9d223eb3f4d), users who choose to partially collateralize positions are at risk of liquidation. Say 10 ETH is required to fully collateralize a position and 4 ETH to partially collateralize. You could create a smart contract into which a user deposits the 10 ETH, where 4 ETH go to collateralizing the options position and the other 6 ETH earn interest \(eg. in Yearn\). If the option position gets close to undercollateralization, the contract can pull some ETH from Yearn to top up the option collateral. 

### Liquidator Bot 

The  [partial collateralization update](https://medium.com/opyn/partially-collateralized-options-now-in-defi-b9d223eb3f4d) includes liquidations, so you can build a liquidator bot to liquidate undercollateralized positions.

### Liquidation Interface 

The  [partial collateralization update](https://medium.com/opyn/partially-collateralized-options-now-in-defi-b9d223eb3f4d) includes liquidations, so you can build an interface that allows anyone to liquidate positions, even if they haven't built a liquidator bot. 

### Rollovers <a id="42fa"></a>

Allow users to rollover their options from one expiry until the next. For example, if a user holds on to an option that expires on Oct. 30, give them the ability to have that option automatically rollover to expire on a date in the future eg. Nov 30. You can accomplish this relatively simply using the new “operator” functionality in Opyn v2, where users can delegate vault actions to another smart contract.

### Portfolio Managers <a id="cca3"></a>

With Opyn v2’s new “operator” functionality, users can delegate out portfolio management to dedicated portfolio managers. These managers could be individuals or smart contracts that employ specific strategies.

### Structured Products <a id="fcfe"></a>

You can use options in combination with other financial primitives to build interesting [structured products](https://www.investopedia.com/articles/optioninvestor/07/structured_products.asp). For example, you could create a[ principal protected note](https://www.investopedia.com/terms/p/principalprotectednote.asp), where you attach a call or put option to an ERC-20. One way this could work to go to a money market \(eg. Compound, Aave\), look at the fixed rate lending rates, and deposit an amount \(say 0.99 USDC\) that yields 1 USDC at expiry. Then you could use the remaining 0.01 USDC to buy a call option. The user’s upside exposure would be based on the 0.01 and the price of a call option. Another strategy could be [the wheel](https://seekingalpha.com/instablog/1046492-markus-heitkoetter/5514813-wheel-option-strategy-example), where you sell puts to collect premium, and if the puts are exercised, sell calls. 

### Physical Settlement Operator 

Opyn v2 options are cash settled in their collateral asset. However, some users prefer to received the underlying asset if they are exercised. This has been a request especially for put sellers who would prefer to receive ETH rather than USDC cash settlement. You can built an operator takes a put seller's USDC and purchases ETH with it, if exercised. 

### Auto Redeem Operator

Currently, Opyn users must return to [Opyn.co](http://opyn.co/) to redeem their collateral after an option expires. Using Opyn's operator function, you can build a smart contract that automatically redeems oTokens for users after expiration. 

### OTC oTokens Interface <a id="fafd"></a>

To avoid slippage, a lot of large oToken users are looking for ways to conduct OTC trades for oTokens. You could facilitate this using 0x as a settlement layer, building a simple interface for parties to interact with eachother while preserving anonymity — this could be something similar to what [Boxswap](https://boxswap.io/) does for OTC NFT trading.

### Simplified Interface for options beginners

Options can be intimidating. You can create an interface that breaks down options into simple steps \(i.e. Do you want to earn income or speculate? → 2 Do you think the price of ETH will be higher or lower on X date, etc.\) to help new options traders better understand how to trade options. This could also be a beginner options interface that has more educational content displaying on the front end. The purpose of this interface could be to make options trading more approachable.

### Advanced Interface for quant traders

Advanced options traders need access to calculations, figures, and charts that might be too confusing for beginner or intermediate traders. Examples of more advanced trading platforms are [Deribit](https://www.deribit.com/), [dYdX](https://dydx.exchange/), [LedgerX](https://www.ledgerx.com/options) or more traditional options trading platforms such as [ThinkorSwim](https://www.tdameritrade.com/tools-and-platforms/thinkorswim/desktop.page).

### Position Builder

To help users better understand options, you could create an options "position builder," by asking basic questions \(e.g. do you think ETH will be above $2000 on March 18\). Once a user has gone through the position builder steps, allow the trader to buy or sell the option by directly linking them to that option options on [Opyn.co](http://opyn.co/)

### Gain & Loss Calculator

To help users better understand options' potential gain / loss under various market conditions, you could create a gain loss calculator / simulator for each option position on [Opyn.co](http://opyn.co/). 

### Options Trading Leaderboard \(e.g. [gamified leaderboards](https://matcha.xyz/moolah)\)

To add a gamification element to options trading, you could create a leaderboard that ranks users' addresses by trading volume, \# of contracts traded, or another metric.

### **SDK for Gamma Protocol**

You could create a python / js / rust library to interact with Gamma Protocol. This could incldue setting up some basic architecture, testing frameworks, and having wrapping functions to batch actions to help other developers integrating with opyn.

### **Volatility Oracle** <a id="e6bb"></a>

Using put and call options you can develop a volatility oracle like the [VIX](https://www.investopedia.com/ask/answers/021015/what-cboe-volatility-index-vix.asp), which tracks volatility in traditional finance.

## Existing Projects 

You can check out the projects that have integrated Opyn here! A number of them have open-source codebases that you can use to learn from as well. 

### **Gamma Portal**

Gamma Portal is an open-sourced alternative front end to interact with [Gamma protocol](https://github.com/opynfinance/GammaProtocol)

* [Site](https://gammaportal.xyz/#/) 
* [Github](https://github.com/antoncoding/gamma-portal) 

### **Ribbon**

Ribbon Finance uses financial engineering to create structured products that deliver sustainable yield

* [Site](https://www.ribbon.finance/) 
* [Github](https://github.com/ribbon-finance)

### **Opeth**

Opeth is a synthetic instrument fusing options with the underlying asset to power liquidation free and capital efficient loans

* [Site](https://opeth.finance/) 
* [Github](https://github.com/defidollar)

### **Fontis**

Fontis enables users to earn a yield by depositing assets into perpetual vaults trading options strategies

* [Site](https://fontis.finance/) 
* [Github](https://github.com/FontisFinance/contracts)

### **Optional**

Optional is creating decentralized social trading for options

* [Site](https://optional.finance/) 
* [Github](https://github.com/Alpha-Serpentis-Developments/Project-Mimic)

### **DEXTF**

DEXTF creates structured tokens making use of oTokens in the set of protocols they integrate from

* [Site](https://dextf.com/) 

