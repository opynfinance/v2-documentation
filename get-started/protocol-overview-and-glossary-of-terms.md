# System Roles

## Admin Roles 

| **Role** | **Action** |
| :--- | :--- |
| Owner | Can whitelist/blacklist collateral and oToken addresses, update/upgrade modules, set addresses for other roles in the system, and manage oracle parameters. |
| Partial Pauser | Can pause all actions other than redeem oToken and settle vault  |
| Full Pauser | Can fully pause the system in case of an emergency.  |
| Disputer | Price disputer for the oracle contract that can dispute the price submitted by Pricer contracts, and has the privilege to override with a new price. |
| Pricer | Can submit oracle prices  |
| Farmer | Can withdraw any token excess balance in the pool. |

## Operators

Every account can define multiple operators. An operator is an address that can update the account's positions on behalf of the account. That means, an operator has the privilege to move an ERC20 token from the account's address into a vault, if the account has approved the MarginPool contracts beforehand, or it can also take out excess collateral from a vault to any address. Usually you won't want to add any EOA as operator.

Operator is a very critical role in Gamma. With this design, we can enable lots of innovation like rollover contract, auto-minting AMM to be built on top of Gamma.

Operators have full control over user funds and can take any action on behalf of a user. The only action operators cannot take is to add new operators or remove new operators.

