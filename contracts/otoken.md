# oToken

## Overview

**oToken** is the ERC20 compatible contract that each represents an option product. All the oTokens have 8 decimals, and the name and symbol of an oToken is determined by the underlying, strike, collateral, expiry and strike price.

The mint and burn privilege is only granted to the Controller. When a seller wants to sell an option, the controller checks if the seller’s vault is properly collateralized, and then calls **mintOtoken** function on corresponding oToken contract; when sellers close their positions or buyers redeem their oTokens, the Controller will use the **burnOtoken** function.

Otoken is the ERC20 token for an option. The Otoken inherits ERC20Initializable because we need to use the init instead of constructor.

### Functions:

* `init(address _addressBook, address _underlyingAsset, address _strikeAsset, address _collateralAsset, uint256 _strikePrice, uint256 _expiryTimestamp, bool _isPut) (external)`
* `getOtokenDetails() (external)`
* `mintOtoken(address account, uint256 amount) (external)`
* `burnOtoken(address account, uint256 amount) (external)`
* `_getNameAndSymbol() (internal)`
* `_getDisplayedStrikePrice(uint256 _strikePrice) (internal)`
* `_uintTo2Chars(uint256 number) (internal)`
* `_getOptionType(bool _isPut) (internal)`
* `_slice(string _s, uint256 _start, uint256 _end) (internal)`
* `_getMonth(uint256 _month) (internal)`

## Functions

### Function `init(address _addressBook, address _underlyingAsset, address _strikeAsset, address _collateralAsset, uint256 _strikePrice, uint256 _expiryTimestamp, bool _isPut) external`

initialize the oToken

**Parameters:**

* `_addressBook`: addressbook module
* `_underlyingAsset`: asset that the option references
* `_strikeAsset`: asset that the strike price is denominated in
* `_collateralAsset`: asset that is held as collateral against short/written options
* `_strikePrice`: strike price with decimals = 8
* `_expiryTimestamp`: expiration timestamp of the option, represented as a unix timestamp
* `_isPut`: True if a put option, False if a call option

### Function `getOtokenDetails() → address, address, address, uint256, uint256, bool external`

### Function `mintOtoken(address account, uint256 amount) external`

mint oToken for an account

Controller only method where access control is taken care of by \_beforeTokenTransfer hook

**Parameters:**

* `account`: account to mint token to
* `amount`: amount to mint

### Function `burnOtoken(address account, uint256 amount) external`

burn oToken from an account.

Controller only method where access control is taken care of by \_beforeTokenTransfer hook

**Parameters:**

* `account`: account to burn token from
* `amount`: amount to burn





