# Taking Actions

## Protocol Actions

The main way to interact with the protocol is through a single function called [`operate`](https://github.com/opynfinance/GammaProtocol/blob/386386bac50e24816931190a243e1f220d043c29/contracts/core/Controller.sol#L411). This `operate` function takes in [`actions`](https://github.com/opynfinance/GammaProtocol/blob/master/contracts/libs/Actions.sol) as parameters, where each action specifies what you'd like to do \(eg. add collateral, mint options\). 

This pattern allows you to string together multiple actions into one `operate` call to achieve many interactions with one call \(eg. creating a short position by putting down collateral and minting options\). 

Additionally, collateralization is only checked at the end of an operation, allowing for flash mint functionality, where you mint an option, sell it, and then use the proceeds from selling as collateral. 

For how to sell oTokens once they've been minted, or ways to buy oTokens [see Trading oTokens](https://opyn.gitbook.io/opyn/get-started/trading-otokens). oToken trades are not done through the core protocol, but through any venue that supports ERC20 trading. 

## Action Structure

To use the `operate` function in [Controller](https://github.com/opynfinance/GammaProtocol/blob/master/contracts/core/Controller.sol), you need to input an array of actions. An [`Action struct`](https://github.com/opynfinance/GammaProtocol/blob/386386bac50e24816931190a243e1f220d043c29/contracts/libs/Actions.sol#L29) is defined as follows:

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

* `actionType`: type of action that is being performed on the system. 
* `owner`: vault owner's address 
* `vaultId`: the id of the vault to be modified 
* `secondAddress`: another address field \(usage dependent on action type\)
* `asset`: asset that is being transferred.
* `amount`: amount of asset
* `index`: always 0 for the current version.
  * each vault can hold multiple short / long / collateral assets but we are restricting the scope to only 1 of each in this version. In future versions this would be the index of the short / long / collateral asset that needs to be modified
* `data`: used for arbitrary function calls

## Available Actions

To create an action, you must specify actionType and **the fields required for that specific action**. You can leave the other fields as default values. 

```javascript
//types of actions available 
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
    Liquidate, 
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
* `secondAddress`: the address depositing the collateral assets. It can only be either `msg.sender`or the owner \(if `msg.sender` is an [**operator**](https://opyn.gitbook.io/opyn/get-started/protocol-overview-and-glossary-of-terms#operators) of owner\).
* `asset`: the address of the **collateral** you want to deposit.
* `index`: 0
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
//approve collateral to margin pool
await ERC20(USDC).approve(marginPool.address, 100 * 1e6)
await controller.operate(args)
```
{% endtab %}
{% endtabs %}

### WithdrawCollateral

Withdraw collateral from a vault. \(Opposite of DepositCollateral\)

#### Required Fields:

* `owner` : the vault owner. 
* `vaultId`: the vault id to withdraw collateral from 
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

Deposit an oTokens into a vault. This allows you to create [spreads](https://opyn.gitbook.io/opyn/#what-is-a-spread) \(using an option to collateralize other options\).

#### Required Fields:

* `owner` : the vault owner. 
* `vaultId`: the vault id to deposit into
* `secondAddress`: the address depositing the oTokens. It can only be either `msg.sender`or the owner \(if `msg.sender` is an [**operator**](https://opyn.gitbook.io/opyn/get-started/protocol-overview-and-glossary-of-terms#operators) of owner\).
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
// approve oToken collateral to margin pool
await ERC20(ETHUSD200Put).approve(marginPool.address, 100 * 1e8)
await controller.operate(args)
```
{% endtab %}
{% endtabs %}

### WithdrawLongOption

Withdraw a long oTokens from a vault. \(Opposite of DepositLongOption\)

#### Required Fields:

* `owner` : the vault owner. 
* `vaultId`: the vault id to withdraw from 
* `secondAddress`: destination address to withdraw long oTokens to.
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
* `secondAddress`: destination address that receives the minted oTokens \(most often the owner who is minting\)
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
* `secondAddress`: address from which the oTokens are transferred \(most often owner address\) 
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

Settle a vault after expiry. If the vault contains minted oTokens and collateral, this function will payout the excess collateral based on spot price at expiry. 

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

Redeem expired oTokens for collateral. All oTokens are auto-exercised at expiry, and any oToken holder can use this redeem function to claim the payout if the oToken expires in the money.

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
await ERC20(ETHUSD250Put).approve(marginPool.address, 1e8)
await controller.operate(args)
```
{% endtab %}
{% endtabs %}

### Liquidate 

Liquidate a vault in the danger zone by bringing oTokens \(either by buying or minting\) and redeeming collateral. This is a great use case for [flash-minting](https://opyn.gitbook.io/opyn/#what-is-a-flash-mint). You can view this blog for a [high level overview](https://medium.com/opyn/partially-collateralized-options-now-in-defi-b9d223eb3f4d) of liquidations and this doc for [more specifics](https://www.notion.so/opynopyn/Gamma-Protocol-Liquidations-1ffd204e403245199a433b98c5cc613b). 

To find out if a vault is in the danger zone, you can call `isLiquidatable()` in the [Controller](https://opyn.gitbook.io/opyn/getting-started/abis-smart-contract-addresses), passing in the vault owner, vault id, and the pricer round id. 

* If the vault is not liquidatable it will return false, 0, 0 
* If the vault is liquidatable it will return true, amount of USDC to repay 1 oToken, USDC dust amount 
  * The dust amount is the minimum amount that must remain 

You can find the pricer roundId by calling `latestRoundData()` from the [Chainlink price feed](https://docs.chain.link/docs/get-the-latest-price/). 

#### Required Fields:

* `owner:` the address of the vault owner to liquidate
* `secondAddress`: the liquidator address
* `vaultId`: the vault to liquidate 
* `amount`: the amount of oToken to pay back 
* `data`: the roundId to liquidate with, encoded

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const ZERO_ADDRESS = "0x0000000000000000000000000000000000000000"
const vaultOwner = "0xcc5d905b9c2c8c9329eb4e25dc086369d6c7777c"
const liquidator = "0xC257274276a4E539741Ca11b590B9447B26A8051"
const roundId = "0x0000000000000000000000000000000000000000000000000000000000000003"
const args = [{
    actionType: ActionType.Liquidate,
    owner: vaultOwner,
    secondAddress: liquidator,
    asset: ZERO_ADDRESS,
    vaultId: 0, // liquidate vault 0 
    amount: 1e8, // liquidate 1 oToken
    index: 0,
    data: roundId,
}]
await controller.operate(args)
```
{% endtab %}
{% endtabs %}

### Call

Call an arbitrary contract. The protocol is currently under **restricted mode**, where we maintain a whitelist of Callee addresses that can receive calls from the Controller during an operate function call. In the future, anyone can create arbitrary Callee contracts and call it with other actions in one transaction. 

#### Required fields:

* `secondAddress`: callee receiver contract.

#### Optional fields

* `owner` : the vault owner. 
* `vaultId`: the vault id to mint from
* `amount`: amount of ETH to attach to the call
* `data`: other arbitrary data to call the contract with

**Example using** [**PermitCallee**](https://github.com/opynfinance/GammaProtocol/blob/master/contracts/external/callees/PermitCallee.sol)

* The action arguments will differ based on any given callee's requirements

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const ZERO_ADDRESS = "0x0000000000000000000000000000000000000000"
const owner = "0xcc5d905b9c2c8c9329eb4e25dc086369d6c7777c"
const signedMessage = await signERC2612Permit(wallet.provider, tokenId, owner, spenderAddress.toString(), value, maxDeadline, nonce);
const data = ethers.utils.defaultAbiCoder.encode(
 ['address', 'address', 'address', 'uint256', 'uint256', 'uint8', 'bytes32', 'bytes32'],
 [tokenId, owner, spenderAddress, value.toString(), maxDeadline, signedMessage.v, signedMessage.r, signedMessage.s],
)

const args = [{
    actionType: ActionType.Call,
    owner: owner,
    secondAddress: ZERO_ADDRESS,
    asset: ZERO_ADDRESS,
    vaultId: 0,
    amount: 0,
    index: 0,
    data: data,
}]
await controller.operate(args)       
```
{% endtab %}
{% endtabs %}

## Combining Actions

The [`operate`](https://github.com/opynfinance/GammaProtocol/blob/386386bac50e24816931190a243e1f220d043c29/contracts/core/Controller.sol#L411) function in Controller contract takes in an array of actions. This allows you to combine actions! We'll outline some common combinations here. 

#### Constraints

1. In the first version, we restrict all the actions in a single operate call to have the same `owner` and `vaultId` field if it's an action on a vault, except `SettleVault`. This mean you cannot create multiple vaults nor deposit assets into multiple vaults in a single transaction.
2. Also as mentioned in the Call section, we will start the protocol under restricted mode that only whitelisted callee contracts can be called during an operate call.

### Create Short Position

Combines the [OpenVault](https://opyn.gitbook.io/opyn/get-started/actions#openvault), [DepositCollateral](https://opyn.gitbook.io/opyn/get-started/actions#depositcollateral), and [MintShortOption](https://opyn.gitbook.io/opyn/get-started/actions#mintshortoption) actions to create a short position by opening a vault, supplying collateral and minting options from a vault. 

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const ZERO_ADDRESS = "0x0000000000000000000000000000000000000000"
const USDC = "0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48"
const owner = "0xcc5d905b9c2c8c9329eb4e25dc086369d6c7777c"
const ETHUSD250Put = "0xde99ea535749f02da84d13e6f8253291e32d3a7f"
const args = [
    //Open Vault 
    {
        actionType: ActionType.OpenVault,
        owner: "0xcc5d905b9c2c8c9329eb4e25dc086369d6c7777c",
        secondAddress: ZERO_ADDRESS,
        asset: ZERO_ADDRESS,
        vaultId: 1, // open the first vault
        amount: 0,
        index: 0,
        data: ZERO_ADDRESS,
    },
    
    //Deposit Collateral 
    {
        actionType: ActionType.DepositCollateral,
        owner: owner,
        secondAddress: owner,
        asset: USDC,
        vaultId: 1, // deposit to the first vault
        amount: 100 * 1e6, // deposit 100 USDC
        index: 0,
        data: ZERO_ADDRESS,
    }, 
    
    //Mint oTokens
    {
        actionType: ActionType.MintShortOption,
        owner: owner,
        secondAddress: owner,
        asset: ETHUSD250Put,
        vaultId: 1, // mint from the first vault
        amount: 100 * 1e8, // mint 100 oToken
        index: 0,
        data: ZERO_ADDRESS,
    }
]
//approve collateral to margin pool
await ERC20(USDC).approve(marginPool.address, 100 * 1e6)
await controller.operate(args)
```
{% endtab %}
{% endtabs %}

### Create Spread 

Combines [DepositLongOption](https://opyn.gitbook.io/opyn/get-started/actions#depositlongoption), [DepositCollateral](https://opyn.gitbook.io/opyn/get-started/actions#depositcollateral) \(if needed\), and [MintShortOption](https://opyn.gitbook.io/opyn/get-started/actions#mintshortoption) to create a spread. 

You can learn more about spreads [here](https://opyn.gitbook.io/opyn/#what-is-a-spread). For debit spreads you would not need to deposit collateral beyond the long option, but for credit spreads you would. Below we show a credit spread. 

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const ZERO_ADDRESS = "0x0000000000000000000000000000000000000000"
const USDC = "0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48"
const owner = "0xcc5d905b9c2c8c9329eb4e25dc086369d6c7777c"
const ETHUSD250Put = "0xde99ea535749f02da84d13e6f8253291e32d3a7f"
const args = [
    //Open Vault 
    {
        actionType: ActionType.OpenVault,
        owner: "0xcc5d905b9c2c8c9329eb4e25dc086369d6c7777c",
        secondAddress: ZERO_ADDRESS,
        asset: ZERO_ADDRESS,
        vaultId: 1, // open the first vault
        amount: 0,
        index: 0,
        data: ZERO_ADDRESS,
    },
    
    //Deposit Long Option
    {
        actionType: ActionType.DepositLongOption,
        owner: owner,
        secondAddress: owner,
        asset: ETHUSD200Put,
        vaultId: 1, // deposit some option to the first vault
        amount: 100 * 1e8, // deposit 100 oToken
        index: 0,
        data: ZERO_ADDRESS,
    },
    
    //Deposit Collateral 
    {
        actionType: ActionType.DepositCollateral,
        owner: owner,
        secondAddress: owner,
        asset: USDC,
        vaultId: 1, // deposit to the first vault
        amount: 100 * 1e6, // deposit 100 USDC
        index: 0,
        data: ZERO_ADDRESS,
    }, 
    
    //Mint oTokens
    {
        actionType: ActionType.MintShortOption,
        owner: owner,
        secondAddress: owner,
        asset: ETHUSD250Put,
        vaultId: 1, // mint from the first vault
        amount: 100 * 1e8, // mint 100 oToken
        index: 0,
        data: ZERO_ADDRESS,
    }
]
// approve oToken collateral to margin pool
await ERC20(ETHUSD200Put).approve(marginPool.address, 100 * 1e8)
//approve USDC collateral to margin pool
await ERC20(USDC).approve(marginPool.address, 100 * 1e6)
await controller.operate(args)
```
{% endtab %}
{% endtabs %}

### Close Position

In order to close a position, you must buy back oTokens from an exchange if you previously sold your oTokens \([see Trading oTokens](https://opyn.gitbook.io/opyn/get-started/trading-otokens)\), and then you can combine the [BurnShortOption](https://opyn.gitbook.io/opyn/get-started/actions#burnshortoption) and [WithdrawCollateral](https://opyn.gitbook.io/opyn/get-started/actions#withdrawcollateral) actions to burn those oTokens and redeem collateral.

If you are trying to close out a spread, you will also need to call [WithdrawLongOption](https://opyn.gitbook.io/opyn/get-started/actions#withdrawlongoption) to redeem your long option after you've burned oTokens.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const ZERO_ADDRESS = "0x0000000000000000000000000000000000000000"
const ETHUSD250Put = "0xde99ea535749f02da84d13e6f8253291e32d3a7f"
const owner = "0xcc5d905b9c2c8c9329eb4e25dc086369d6c7777c"
const args = [
    //Burn Short Option 
    {
        actionType: ActionType.BurnShortOption,
        owner: owner,
        secondAddress: owner,
        asset: ETHUSD250Put,
        vaultId: 1, // burn from the first vault
        amount: 100 * 1e8, // burn 100 oToken
        index: 0,
        data: ZERO_ADDRESS,
    }, 
    
    //Withdraw collateral
    {
        actionType: ActionType.WithdrawCollateral,
        owner: owner,
        secondAddress: owner, // withdraw to the owner address
        asset: USDC,
        vaultId: 1,
        amount: 100 * 1e6, // withdraw 100 USDC
        index: 0,
        data: ZERO_ADDRESS,
    }
    
]
await controller.operate(args)
```
{% endtab %}
{% endtabs %}

### Permit, Deposit, and Mint

This is an example of combining the [Call](https://opyn.gitbook.io/opyn/get-started/actions#call) action with other actions, in this case the common [DepositCollateral](https://opyn.gitbook.io/opyn/get-started/actions#depositcollateral) and [MintShortOption](https://opyn.gitbook.io/opyn/get-started/actions#mintshortoption) actions required to create a short position. 

The call action facilitates arbitrary calls to whitelisted callees. This uses the [PermitCallee](https://github.com/opynfinance/GammaProtocol/blob/master/contracts/external/callees/PermitCallee.sol) to [let users sign transactions](https://eips.ethereum.org/EIPS/eip-2612) to allow a contract to spend their ERC20 funds vs. having to send an approve transaction. 

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const ZERO_ADDRESS = "0x0000000000000000000000000000000000000000"
const USDC = "0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48"
const owner = "0xcc5d905b9c2c8c9329eb4e25dc086369d6c7777c"
const ETHUSD250Put = "0xde99ea535749f02da84d13e6f8253291e32d3a7f"
const signedMessage = await signERC2612Permit(wallet.provider, tokenId, owner, spenderAddress.toString(), value, maxDeadline, nonce);
const data = ethers.utils.defaultAbiCoder.encode(
 ['address', 'address', 'address', 'uint256', 'uint256', 'uint8', 'bytes32', 'bytes32'],
 [tokenId, owner, spenderAddress, value.toString(), maxDeadline, signedMessage.v, signedMessage.r, signedMessage.s],
)
const args = [
    //Permit collateral to margin pool  
    {
        actionType: ActionType.Call,
        owner: owner,
        secondAddress: ZERO_ADDRESS,
        asset: ZERO_ADDRESS,
        vaultId: 0,
        amount: 0,
        index: 0,
        data: data,
    },
    
    //Deposit Collateral 
    {
        actionType: ActionType.DepositCollateral,
        owner: owner,
        secondAddress: owner,
        asset: USDC,
        vaultId: 1, // deposit to the first vault
        amount: 100 * 1e6, // deposit 100 USDC
        index: 0,
        data: ZERO_ADDRESS,
    }, 
    
    //Mint oTokens
    {
        actionType: ActionType.MintShortOption,
        owner: owner,
        secondAddress: owner,
        asset: ETHUSD250Put,
        vaultId: 1, // mint from the first vault
        amount: 100 * 1e8, // mint 100 oToken
        index: 0,
        data: ZERO_ADDRESS,
    }
]
await controller.operate(args)
```
{% endtab %}
{% endtabs %}





