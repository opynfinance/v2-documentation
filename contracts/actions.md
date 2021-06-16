---
description: >-
  A library that provides a ActionArgs struct, sub types of Action structs, and
  functions to parse ActionArgs into specific Actions.
---

# Actions

To use the operate function in Controller, you need to put in array of actions as input. An `Action struct` is defined as follow:

```javascript
struct ActionArgs {
    ActionType actionType;
    
    address owner;
    
    address secondAddress;
     
    address asset;
    
    uint256 vaultId;
    
    uint256 amount;
    
    uint256 index;
    
    bytes data;
}
```

* [`actionType`](actions.md#actiontype): type of action that is being performed on the system. To create an action, you put a actionTypes and **the fields required for that specific action**. \(Leave other fields as default.\)
* `owner`: owner of the vault, if it's an action on a vault.
* `vaultId`: the id of the vault, if it's an action on a vault.
* `secondAddress`: another address field \(usage depend on the action type\)
* `asset`: asset that is being transferred.
* `amount`: amount of asset
* `index`: always 0 for the current version.
  * each vault can hold multiple short / long / collateral assets but we are restricting the scope to only 1 of each in this version. in future versions this would be the index of the short / long / collateral asset that needs to be modified
* `data`: used for arbitrary function calls

## ActionType

```javascript
enum ActionType {
    OpenVault,
    MintShortOption,
    BurnShortOption,
    DepositLongOption,
    WithdrawLongOption,
    DepositCollateral,
    WithdrawCollateral,
    SettleVault,
    Redeem,
    Call
}
```

### OpenVault

Open a new vault for an account.

#### Required Fields:

* `owner` : the vault owner. 
* `vaultId`: the new vault Id to open. For the first vault, the vaultId is 1. 

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const ZERO_ADDRESS = "0x0000000000000000000000000000000000000000"
const args = [{
    actionType: ActionType.OpenVault,
    owner: "0xcc5d905b9c2c8c9329eb4e25dc086369d6c7777c",
    secondAddress: ZERO_ADDRESS,
    asset: ZERO_ADDRESS,
    vaultId: 1, // open the first vault
    amount: 0,
    index: 0,
    data: ZERO_ADDRESS,
}]
await controller.operate(args)
```
{% endtab %}
{% endtabs %}

### DepositCollateral

Deposit collateral asset into a vault. This allow you to mint options against the vault.

#### Required Fields:

* `owner` : the vault owner. 
* `vaultId`: the vault id to deposit into
* `secondAddress`: from what address to deposit the collateral assets. It can only be either `msg.sender`or the owner \(if `msg.sender` is an **operator** of owner\).
* `asset`: the address of the **collateral** you want to deposit.
* `index`: 0.
* `amount`: amount to deposit

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const ZERO_ADDRESS = "0x0000000000000000000000000000000000000000"
const USDC = "0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48"
const owner = "0xcc5d905b9c2c8c9329eb4e25dc086369d6c7777c"
const args = [{
    actionType: ActionType.DepositCollateral,
    owner: owner,
    secondAddress: owner,
    asset: USDC,
    vaultId: 1, // deposit to the first vault
    amount: 100 * 1e6, // deposit 100 USDC
    index: 0,
    data: ZERO_ADDRESS,
}]
await ERC20(USDC).approve(marginPool.address, 100 * 1*6)
await controller.operate(args)
```
{% endtab %}
{% endtabs %}

### WithdrawCollateral

Withdraw collateral from a vault. \(Opposite to DepositCollateral\)

#### Required Fields:

* `owner` : the vault owner. 
* `vaultId`: the vault id to withdraw from 
* `secondAddress`: destination address to withdraw collateral to.
* `asset`: the address of the **collateral** you want to withdraw
* `index`: 0.
* `amount`: amount to withdraw

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const ZERO_ADDRESS = "0x0000000000000000000000000000000000000000"
const USDC = "0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48"
const owner = "0xcc5d905b9c2c8c9329eb4e25dc086369d6c7777c"
const args = [{
    actionType: ActionType.WithdrawCollateral,
    owner: owner,
    secondAddress: owner, // withdraw to the owner address
    asset: USDC,
    vaultId: 1,
    amount: 100 * 1e6, // withdraw 100 USDC
    index: 0,
    data: ZERO_ADDRESS,
}]
await controller.operate(args)
```
{% endtab %}
{% endtabs %}

### DepositLongOption

Deposit an oTokens into a vault. This allow you to create spreads \(Use less collateral to mint an option.\)

#### Required Fields:

* `owner` : the vault owner. 
* `vaultId`: the vault id to deposit into
* `secondAddress`: from what address to deposit the oTokens. it can only be either `msg.sender`or the owner \(if `msg.sender` is an **operator** of owner\).
* `asset`: the address of the **oToken** you want to deposit
* `index`: 0.
* `amount`: amount to deposit

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const ZERO_ADDRESS = "0x0000000000000000000000000000000000000000"
const ETHUSD200Put = "0xde99ea535749f02da84d13e6f8253291e32d3a7f"
const owner = "0xcc5d905b9c2c8c9329eb4e25dc086369d6c7777c"
const args = [{
    actionType: ActionType.DepositLongOption,
    owner: owner,
    secondAddress: owner,
    asset: ETHUSD200Put,
    vaultId: 1, // deposit some option to the first vault
    amount: 100 * 1e8, // deposit 100 oToken
    index: 0,
    data: ZERO_ADDRESS,
}]
await ERC20(ETHUSD200Put).approve(marginPool.address, 100 * 1*8)
await controller.operate(args)
```
{% endtab %}
{% endtabs %}

### WithdrawLongOption

Withdraw a long oTokens from a vault. \(Opposite to DepositLongOption\)

#### Required Fields:

* `owner` : the vault owner. 
* `vaultId`: the vault id to withdraw from 
* `secondAddress`: what address to withdraw long oTokens to.
* `asset`: the address of the **oToken** you want to withdraw
* `index`: 0.
* `amount`: amount to withdraw

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const ZERO_ADDRESS = "0x0000000000000000000000000000000000000000"
const ETHUSD200Put = "0xde99ea535749f02da84d13e6f8253291e32d3a7f"
const owner = "0xcc5d905b9c2c8c9329eb4e25dc086369d6c7777c"
const args = [{
    actionType: ActionType.WithdrawLongOption,
    owner: owner,
    secondAddress: owner,
    asset: ETHUSD200Put,
    vaultId: 1,
    amount: 100 * 1e8, // deposit 100 oToken
    index: 0,
    data: ZERO_ADDRESS,
}]
await controller.operate(args)
```
{% endtab %}
{% endtabs %}

### MintShortOption

Mint oTokens from a vault.

#### Required Fields:

* `owner` : the vault owner. 
* `vaultId`: the vault id to mint from 
* `secondAddress`: the receiver address
* `asset`: the address of the oToken you want to mint
* `index`: 0.
* `amount`: amount to mint

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const ZERO_ADDRESS = "0x0000000000000000000000000000000000000000"
const ETHUSD250Put = "0xde99ea535749f02da84d13e6f8253291e32d3a7f"
const owner = "0xcc5d905b9c2c8c9329eb4e25dc086369d6c7777c"
const args = [{
    actionType: ActionType.MintShortOption,
    owner: owner,
    secondAddress: owner,
    asset: ETHUSD250Put,
    vaultId: 1, // mint from the first vault
    amount: 100 * 1e8, // mint 100 oToken
    index: 0,
    data: ZERO_ADDRESS,
}]
await controller.operate(args)
```
{% endtab %}
{% endtabs %}

### BurnShortOption

Burn oTokens from a vault.

#### Required Fields:

* `owner` : the vault owner. 
* `vaultId`: the vault id to mint from 
* `secondAddress`: the receiver address
* `asset`: the address of the oToken you want to burn
* `index`: 0.
* `amount`: amount to burn

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const ZERO_ADDRESS = "0x0000000000000000000000000000000000000000"
const ETHUSD250Put = "0xde99ea535749f02da84d13e6f8253291e32d3a7f"
const owner = "0xcc5d905b9c2c8c9329eb4e25dc086369d6c7777c"
const args = [{
    actionType: ActionType.BurnShortOption,
    owner: owner,
    secondAddress: owner,
    asset: ETHUSD250Put,
    vaultId: 1, // burn from the first vault
    amount: 100 * 1e8, // burn 100 oToken
    index: 0,
    data: ZERO_ADDRESS,
}]
await controller.operate(args)
```
{% endtab %}
{% endtabs %}

### SettleVault

Settle a vault after expiry. If the vault contain short oTokens and collateral, this function will payout the excess collateral based on spot price at expiry. 

#### Required Fields:

* `owner` : the vault owner. 
* `vaultId`: the vault id to mint from 
* `secondAddress`: the address to receive collateral payout.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const ZERO_ADDRESS = "0x0000000000000000000000000000000000000000"
const owner = "0xcc5d905b9c2c8c9329eb4e25dc086369d6c7777c"
const args = [{
    actionType: ActionType.SettleVault,
    owner: owner,
    secondAddress: owner,
    asset: ZERO_ADDRESS,
    vaultId: 1, // settle the first vault
    amount: 0,
    index: 0,
    data: ZERO_ADDRESS,
}]
await controller.operate(args)
```
{% endtab %}
{% endtabs %}

### Redeem

Redeem expired oTokens for collateral. All oTokens are auto-exercised at expiry, and any oTokens holder can use this redeem function to claim the payout if the oToken expires in the money.

#### Required Fields:

* `asset:` the oToken to redeem.
* `secondAddress`: the address to receive collateral payout.
* `amount`: amount of oTokens to redeem

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const ZERO_ADDRESS = "0x0000000000000000000000000000000000000000"
const ETHUSD250Put = "0xde99ea535749f02da84d13e6f8253291e32d3a7f"
const holder = "0xcc5d905b9c2c8c9329eb4e25dc086369d6c7777c"
const args = [{
    actionType: ActionType.Redeem,
    owner: ZERO_ADDRESS,
    secondAddress: holder,
    asset: ZERO_ADDRESS,
    vaultId: 0,
    amount: 1e8, // redeem 1 oToken
    index: 0,
    data: ZERO_ADDRESS,
}]
await ERC20(ETHUSD250Put).approve(marginPool.address, 1*8)
await controller.operate(args)
```
{% endtab %}
{% endtabs %}

### Call

Call an arbitrary contract. The protocol started under **restricted mode**, which we maintain a whitelist of Callee address to receive call from the Controller during an operate function call. In the future, anyone can create arbitrary Callee contracts and call it with other actions in one transaction. 

#### Required fields:

* `secondAddress`: callee receiver contract.

#### Optional fields

* `owner` : the vault owner. 
* `vaultId`: the vault id to mint from
* `amount`: amount of ETH to attach to the call
* `data`: other arbitrary data to call the contract with

## Combining Actions

In the **operate** function in Controller contract takes in an array of actions. This enable some more advance usage to combine all the actions or do some kind of flash trade. 

### Constraints

1. In the first version, we restrict all the actions in a single operate call to have the same `owner` and `vaultId` field if it's an action on a vault, except `SettleVault`. This mean you cannot create multiple vaults nor deposit assets into multiple vaults in a single transaction.
2. Also as mentioned in the Call section, we will start the protocol under restricted mode that only whitelisted contracts can be called during an operate call.







