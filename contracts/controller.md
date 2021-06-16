# Controller

### Overview

**Controller** is the entry point for all users, it manages all the opened vaults for all sellers, and also takes care of the redeem operation for buyers. 

The main function for users on Controller is the [operate](controller.md#operate-struct-actions-actionargs-_actions) function. Users can do several operations in a single transaction, and only the final state of a vault will be checked at the end of the transaction. This enables users to do something like flash mint, which mint oTokens first, sell it and then use the profit as part of collateral in a vault.

Controller contract also determines the status of the whole v2 protocol. The **partial pauser** role can partially pause the system and disable creating positions / burning but still enable redeeming oTokens and settling vault. And the **full pauser** can shutdown the system completely, which disables the whole operate function, and can only wait for either a un-shutdown or controller contract to be upgraded. 

Contract that controls the Gamma Protocol and the interaction of all sub contracts

### Functions

#### `setSystemPartiallyPaused(bool _partiallyPaused)` 

allows the partialPauser to toggle the systemPartiallyPaused variable and partially pause or partially unpause the system. Can only be called by the partialPauser

**Parameters:**

* `_partiallyPaused`: new boolean value to set systemPartiallyPaused to

#### `setSystemFullyPaused(bool _fullyPaused)`

allows the fullPauser to toggle the systemFullyPaused variable and fully pause or fully unpause the system. Can only be called by the fullPauser

**Parameters:**

* `_fullyPaused`: new boolean value to set systemFullyPaused to

#### `setFullPauser(address _fullPauser)`

allows the owner to set the fullPauser address. Can only be called by the owner

**Parameters:**

* `_fullPauser`: new fullPauser address

#### `setPartialPauser(address _partialPauser)`

allows the owner to set the partialPauser address. Can only be called by the owner

**Parameters:**

* `_partialPauser`: new partialPauser address

#### `setCallRestriction(bool _isRestricted)` 

allows the owner to toggle the restriction on whitelisted call actions and only allow whitelisted

call addresses or allow any arbitrary call addresses. Can only be called by the owner

**Parameters:**

* `_isRestricted`: new call restriction state

#### `setOperator(address _operator, bool _isOperator)`

allows a user to give or revoke privileges to an operator which can act on their behalf on their vaults. Can only be updated by the vault owner. See [System Role](../get-started/protocol-overview-and-glossary-of-terms.md#operators) page to learn more about operators.

**Parameters:**

* `_operator`: operator that the sender wants to give privileges to or revoke them from
* `_isOperator`: new boolean value that expresses if the sender is giving or revoking privileges for \_operator

#### `refreshConfiguration()` 

updates the configuration of the controller. can only be called by the owner

#### `operate(struct Actions.ActionArgs[] _actions)` 

execute a number of actions on specific vaults. Can only be called when the system is not fully paused

**Parameters:**

* `_actions`: array of actions arguments. Refers to [Actions](../get-started/actions.md) page for more detail.

#### `isOperator(address _owner, address _operator) → bool`

check if a specific address is an [operator](../get-started/protocol-overview-and-glossary-of-terms.md#operators) for an owner account

**Parameters:**

* `_owner`: account owner address
* `_operator`: account operator address

**Return Values:**

* True if the \_operator is an approved operator for the \_owner account

#### Function `getConfiguration() → address, address, address, address` 

returns the current controller configuration

**Return Values:**

* the address of the whitelist module
* the address of the oracle module
* the address of the calculator module
* the address of the pool module

#### `getProceed(address _owner, uint256 _vaultId) → uint256` 

return a vault's proceeds pre or post expiry, the amount of collateral that can be removed from a vault

**Parameters:**

* `_owner`: account owner of the vault
* `_vaultId`: vaultId to return balances for

**Return Values:**

* amount of collateral that can be taken out

#### `getPayout(address _otoken, uint256 _amount) → uint256 public`

get an oToken's payout/cash value after expiry, in the collateral asset

**Parameters:**

* `_otoken`: oToken address
* `_amount`: amount of the oToken to calculate the payout for, always represented in 1e8

**Return Values:**

* amount of collateral to pay out

#### `isSettlementAllowed(address _otoken) → bool`

return if an expired oToken contract’s settlement price has been finalized

**Parameters:**

* `_otoken`: address of the oToken

**Return Values:**

* True if the oToken has expired AND all oracle prices at the expiry timestamp have been finalized, False if not

#### `getAccountVaultCounter(address _accountOwner) → uint256`

get the number of vaults for a specified account owner

**Parameters:**

* `_accountOwner`: account owner address

**Return Values:**

* number of vaults

#### `hasExpired(address _otoken) → bool`

check if an oToken has expired

**Parameters:**

* `_otoken`: oToken address

**Return Values:**

* True if the otoken has expired, False if not

#### `getVault(address _owner, uint256 _vaultId) → struct MarginVault.Vault` 

return a specific vault

**Parameters:**

* `_owner`: account owner
* `_vaultId`: vault id of vault to return

**Return Values:**

* Vault struct that corresponds to the \_vaultId of \_owner

### Events

#### `AccountOperatorUpdated(address accountOwner, address operator, bool isSet)`

emits an event when an account operator is updated for a specific account owner

#### `VaultOpened(address accountOwner, uint256 vaultId)`

emits an event when a new vault is opened

#### `LongOtokenDeposited(address otoken, address accountOwner, address from, uint256 vaultId, uint256 amount)`

emits an event when a long oToken is deposited into a vault

#### `LongOtokenWithdrawed(address otoken, address AccountOwner, address to, uint256 vaultId, uint256 amount)`

emits an event when a long oToken is withdrawn from a vault

#### `CollateralAssetDeposited(address asset, address accountOwner, address from, uint256 vaultId, uint256 amount)`

emits an event when a collateral asset is deposited into a vault

#### `CollateralAssetWithdrawed(address asset, address AccountOwner, address to, uint256 vaultId, uint256 amount)`

emits an event when a collateral asset is withdrawn from a vault

#### `ShortOtokenMinted(address otoken, address AccountOwner, address to, uint256 vaultId, uint256 amount)`

emits an event when a short oToken is minted from a vault

#### `ShortOtokenBurned(address otoken, address AccountOwner, address from, uint256 vaultId, uint256 amount)`

emits an event when a short oToken is burned

#### `Redeem(address otoken, address redeemer, address receiver, address collateralAsset, uint256 otokenBurned, uint256 payout)`

emits an event when an oToken is redeemed

#### `VaultSettled(address AccountOwner, address to, uint256 vaultId, uint256 payout)`

emits an event when a vault is settled

#### `CallExecuted(address from, address to, address vaultOwner, uint256 vaultId, bytes data)`

emits an event when a call action is executed

#### `FullPauserUpdated(address oldFullPauser, address newFullPauser)`

emits an event when the fullPauser address changes

#### `PartialPauserUpdated(address oldPartialPauser, address newPartialPauser)`

emits an event when the partialPauser address changes

#### `SystemPartiallyPaused(bool isActive)`

emits an event when the system partial paused status changes

#### `SystemFullyPaused(bool isActive)`

emits an event when the system fully paused status changes

#### `CallRestricted(bool isRestricted)`

emits an event when the call action restriction changes

