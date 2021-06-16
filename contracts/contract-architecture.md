# Contract Architecture

## Architecture

![](https://lh5.googleusercontent.com/J1Swof5gogAGacykAbdFy8fSkv5dxqC9Pb4qkwB_gjuIZMM9KQH7laJBKeYSj8zejB1oBW7SGB8QqtGYBTQDsSic_rEidMrTNsAGYE_aPZF2Cdlj4qCLg0BsNND_9CWlOe0bDY3v)

\*\*\*\*[**AddressBook**](addressbook-1.md) is an address discovery module that keeps track of all the contract addresses in the v2 system. 

\*\*\*\*[**Whitelist**](whitelist-1.md) is a module that manages all restrictions set by the admin. 

\*\*\*\*[**OToken**](otoken.md) is the ERC20 contract that each represents an option product\(see glossary for definition\). Users can easily transfer any oToken just like any other ERC20 tokens, but only the Controller has the privilege to mint or burn otokens. 

\*\*\*\*[**OTokenFactory**](otokenfactory.md) is a factory contract to clone Otoken contracts, anyone can create options with arbitrary strike price and expiry, if the asset pair is whitelisted.  

\*\*\*\*[**Controller**](controller.md) is the entry point for all users, it manages all the opened vaults for all sellers, and also takes care of the exercise operation for buyers. Users can do several operations in a single transaction through the operate function in Controller, and Controller will mint / burn otokens \(interact with OToken contracts\), move funds \(interact with MarginPool\).

\*\*\*\*[**MarginCalculator**](margincalculator.md) is a very specific math library for our vaults. It only does 2 jobs: determine if a vault is valid \(properly collateralized\), and calculate the expected payout for a oToken based on oracle price.

\*\*\*\*[**Oracle**](oracle.md) is the oracle module for the whole v2 system, the MarginCalculator will constantly read from oracle to determine if a vault is properly collateralized. The pricer roles are defined to determine who is in charge of submitting prices for each asset. MarginPool is the contract that stores all the funds, and only listens to transfer commands from Controller.

[**MarginPool**](marginpool.md) is the contract that stores all the funds, and can only be called by the Controller. To open a vault, a user need to approve MarginPool contract to move its ERC20 tokens to deposit collateral and long oTokens.

