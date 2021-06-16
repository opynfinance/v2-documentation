# Introduction

### Protocol Introduction 

Opyn builds protocols on Ethereum that allows users to buy, sell, and create options on  ERC20s. DeFi enthusiasts and projects alike rely on Opyn’s smart contracts and interface to hedge themselves against DeFi risks and take positions on different cryptocurrencies.

**Gamma** is a capital efficient option protocol that enables sellers to create spreads and other combinations, trade atomically, flash loan mint otokens, assign operators to roll over vaults, create perpetual instruments, and more.

Just like Convexity Protocol\(Opyn v1\), **Gamma protocol** enables any user to create option tokens, that represent the right to buy or sell a certain asset in a ~~~~predefined price \(strike price\) at expiry. 

#### Compare to Convexity

As the option seller in Gamma, you can reduce the amount of capital locked in the system by creating spreads: e.g. Instead of putting down 100 USDC and mint 1 ETH-USDC-100 Put, you can buy a ETH-USDC-50 Put from someone else, and only deposit 50 USDC as collateral.

The oTokens created by Gamma are **cash settled European option**, means that all options are automatically exercised at expiry, and when the holder redeem an option, he or she only need to send the oTokens back, the system will pay the holder the cash value base on strike price and underlying spot price at expiry, instead of exchanging the underlying asset and the strike asset.

Please join the `#dev` room in the Opyn community [**Discord**](https://discord.gg/ugAv3SH) server; our team, and members of the community, look forward to helping you build an application on top of Opyn. We're always happy to help, so don't hesitate to ask questions! 

## Resources 

* [Website](http://www.opyn.co/)
* [Github](https://github.com/opynfinance/GammaProtocol)
* [Twitter](https://twitter.com/opyn_)
* [Discord](https://discordapp.com/invite/2NFdXaE)
* [Email](mailto:hello@opyn.co)
* [Ideas on what to build](https://medium.com/opyn/buidling-with-options-otokens-in-defi-pt-2-f561eb67f4af)
* ETHOnline Dev Workshop
  * [ETHOnline Workshop](https://youtu.be/QygsAIhxj1Y)
  * [Slides](https://drive.google.com/file/d/1w79tyX6cuORniTt0qspACFPX75FwSLrG/view?usp=sharing)
  * [Code and Links from Demo ](https://www.dropbox.com/scl/fi/wzqtyzv0gqjizibl0me20/Opyn-V2-Workshop.paper?dl=0&rlkey=qmk5wreoacstk6rrkzeus56fa)
  * [V2 Portal](https://opynv2-portal.netlify.app/)

