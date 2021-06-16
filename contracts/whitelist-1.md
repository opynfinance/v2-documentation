---
description: Whitelist is a module that manages all restrictions set by the system admin.
---

# Whitelist

Whitelist is a module that manages all restrictions set by the system admin. These restrictions include:

1. [Product type](../get-started/protocol-overview-and-glossary-of-terms.md#glossary-of-terms): manages if a unique combination of strike - asset - collateral - put/call type is valid to be created and register in the system
2. Collateral type: manages if a collateral asset is whitelisted to be used in the system.
3. oToken address: manages if a oToken contract is created by our factory and valid to be used as collateral \(or longOToken\) in a position.
4. Callee address: in the [**operate**](controller.md#operate-struct-actions-actionargs-_actions) function of **Controller**, users can pass in multiple actions and do all in one single transaction. The Call action type allows the user to do arbitrary contract calls to external contracts. In the early stage of the protocol we will maintain a whitelisted callee address mapping that determines if the external contract is safe to call. This restriction can later be removed by turning off the restricted mode in the Controller.

### Functions

#### `isWhitelistedProduct(address _underlying, address _strike, address _collateral, bool _isPut) → bool`

check if a product is whitelisted.

**Parameters:**

* `_underlying`: asset that the option references
* `_strike`: asset that the strike price is denominated in
* `_collateral`: asset that is held as collateral against short/written options
* `_isPut`: is this a put option, if not it is a call

**Return Values:**

* true if product is whitelisted

#### `isWhitelistedCollateral(address _collateral) → bool`

check if the collateral is whitelisted

**Parameters:**

* `_collateral`: asset that is held as collateral against short/written options

**Return Values:**

* true if the collateral is whitelisted

#### `isWhitelistedOtoken(address _otoken) → bool`

check if an otoken is whitelisted

**Parameters:**

* `_otoken`: otoken address

**Return Values:**

* true if otoken is whitelisted

#### `isWhitelistedCallee(address _callee) → bool`

check if a callee address is whitelisted for call acton

**Parameters:**

* `_callee`: destination address

**Return Values:**

* true if address is whitelisted

#### `whitelistProduct(address _underlying, address _strike, address _collateral, bool _isPut)` 

allow owner to whitelist product. Can only be called from owner address

**Parameters:**

* `_underlying`: asset that the option references
* `_strike`: asset that the strike price is denominated in
* `_collateral`: asset that is held as collateral against short/written options
* `_isPut`: is this a put option, if not it is a call

#### `blacklistProduct(address _underlying, address _strike, address _collateral, bool _isPut)`

allow owner to blacklist product a product. Can only be called from owner address

**Parameters:**

* `_underlying`: asset that the option references
* `_strike`: asset that the strike price is denominated in
* `_collateral`: asset that is held as collateral against short/written options
* `_isPut`: is this a put option, if not it is a call

#### Function `whitelistCollateral(address _collateral)` 

whitelist a collateral address, can only be called by owner. Can only be called by owner

**Parameters:**

* `_collateral`: collateral asset address

#### `blacklistCollateral(address _collateral)`

whitelist a collateral address, can only be called by owner. Can only be called by owner

**Parameters:**

* `_collateral`: collateral asset address

#### `whitelistOtoken(address _otokenAddress)`

allow Otoken Factory to whitelist a new option. can only be called from the Otoken Factory address

**Parameters:**

* `_otokenAddress`: otoken

#### `blacklistOtoken(address _otokenAddress)` 

allow owner to blacklist a new option. can only be called from the owner's address

**Parameters:**

* `_otokenAddress`: otoken

#### `whitelisteCallee(address _callee)`

allow Owner to whitelisted a callee address. can only be called from the owner address

**Parameters:**

* `_callee`: callee address

#### `blacklistCallee(address _callee)`

allow owner to blacklist a destination address for call action

can only be called from the owner's address

**Parameters:**

* `_callee`: callee address

###  Events

#### `ProductWhitelisted(bytes32 productHash, address underlying, address strike, address collateral, bool isPut)`

emitted when owner whitelist a product

####  `ProductBlacklisted(bytes32 productHash, address underlying, address strike, address collateral, bool isPut)`

emitted when owner blacklist a product

#### `CollateralWhitelisted(address collateral)`

emits an event when a collateral address is whitelisted by the owner address

#### `CollateralBlacklisted(address collateral)`

emits an event when a collateral address is blacklist by the owner address

#### `OtokenWhitelisted(address otoken)`

emitted when Otoken Factory module whitelist an otoken

#### `OtokenBlacklisted(address otoken)`

emitted when owner blacklist an otoken

#### `CalleeWhitelisted(address _callee)`

emitted when owner whitelist a callee address

#### `CalleeBlacklisted(address _callee)`

emitted when owner blacklist a callee address

