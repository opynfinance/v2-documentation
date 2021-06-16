# Introduction

## What are Options 

Options give option buyers the right, but not the obligation to buy or sell the option's underlying asset at the strike price by the expiry date. Options sellers earn a premium in return for taking on this obligation to either buy the underlying from or selling the underlying to the option buyer.

* An option that gives the option buyer the right to buy the underlying asset is called a _call option_
* An option that gives the option buyer the right to sell the underlying asset is called a _put option_

Here are some resources to learn more about options:

* [Khan Academy](https://www.khanacademy.org/economics-finance-domain/core-finance/derivative-securities)
* [OptionAlpha](https://optionalpha.com/members/video-tutorials/options-basics)
* [Investopedia](https://www.investopedia.com/options-basics-tutorial-4583012)

There are also lots of great conversations on our [discord](https://tiny.cc/opyndiscord), and we’re happy to answer any questions :\)

## Why do people use options 

People often use options for the following three uses cases: 

* Hedging 
* Leverage 
* Yield 

Let's break down how. 

**Hedging** 

People often buy put options in order to hedge downside risk. 

For example if ETH is currently $2000 and you're worried about ETH price crashing, you can buy a put option with a strike price of $1800. Then say ETH crashes to $1000, you can still sell your ETH for $1800 using your put option. While you own the put option, you've basically created a price floor of $1800 as the lowest amount you'll sell your ETH at.

**Leverage** 

People often buy call options to gain leverage without having any liquidation risk. 

For example if ETH is currently at $2000 and you think ETH price is going to increase, you could buy a call option with a strike price of $2500. This call option would likely cost less than $100 compared to buying 1 ETH at $2000. Then say ETH runs up to $4000, you buy ETH at $2500 using your call option. There is no liquidation risk, because you are not putting down any collateral to borrow - you are simply paying a premium to buy an option. 

**Yield** 

People often sell put or call options to earn "high yield". 

In order to sell a put or call option, you put down collateral, mint an option, and sell that option. When you sell the option you earn a premium. Because selling options is higher risk than say lending in a money market, since you could be exercised on if the option hits the strike price, you earn a higher premium for taking on that risk, which people often view as "high yield". 

For example if you think ETH isn't going to move too much, you could sell a covered call on ETH earning the premium \("high yield" on ETH\), and if you aren't exercised, can leave with your original ETH collateral and your premium. However, it is important to remember the additional risk that comes with these positions. 

## Protocol Introduction

Opyn V2 \(Gamma Protocol\), allows anyone to buy, sell, and create options on any ERC20 asset. 

Gamma Protocol is the most capital efficient options protocol allowing for: 

* [Partially collateralized](https://medium.com/opyn/partially-collateralized-options-now-in-defi-b9d223eb3f4d) options 
* [Spreads](https://opyn.gitbook.io/opyn/#what-is-a-spread) 
* [Flash Minting](https://opyn.gitbook.io/opyn/#what-is-a-flash-mint) oTokens 
* [Operators](https://opyn.gitbook.io/opyn/#what-is-an-operator) \(useful for rolling over vaults, creating perpetual positions, and more\)
* Option strategies 
  * In addition to the basics outlines above you can also hedge / leverage / earn yield with complex positions like [straddles & strangles](https://www.investopedia.com/ask/answers/05/052805.asp), [the wheel](https://seekingalpha.com/instablog/1046492-markus-heitkoetter/5514813-wheel-option-strategy-example), [principal protected notes](https://www.investopedia.com/terms/p/principalprotectednote.asp) and more

Opyn v2 options are **cash settled, European** options. 

* Cash settled: options are settled in the collateral asset, and option holders receive the difference between the price of the underlying asset at expiry and the strike price from option sellers.
* European: option holders can exercise options only upon expiry

{% hint style="info" %}
Please join the `#dev` room in the Opyn community [**Discord**](https://discord.gg/ugAv3SH) server - we're hyped to help you build on Opyn!
{% endhint %}

## Resources 

**Dev** 

* [Github](https://github.com/opynfinance/GammaProtocol)
* [Discord](https://discordapp.com/invite/2NFdXaE)
* [Ideas on what to build](https://opyn.gitbook.io/opyn/getting-started/use-cases)
* ETHOnline Dev Workshop
  * [ETHOnline Workshop](https://youtu.be/QygsAIhxj1Y)
  * [Slides](https://drive.google.com/file/d/1w79tyX6cuORniTt0qspACFPX75FwSLrG/view?usp=sharing)
  * [Code and Links from Demo ](https://www.dropbox.com/scl/fi/wzqtyzv0gqjizibl0me20/Opyn-V2-Workshop.paper?dl=0&rlkey=qmk5wreoacstk6rrkzeus56fa)

**General** 

* [Website](http://www.opyn.co/)
* [Twitter](https://twitter.com/opyn_)
* [Email](mailto:hello@opyn.co)
* [Grants](https://www.notion.so/opynopyn/Opyn-Ecosystem-Grants-627f298f1eec49c1914172b929a03932)

