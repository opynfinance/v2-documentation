# oToken

**oToken** is the ERC20 compatible contract that each represents an option product. All the oTokens have 8 decimals, and the name and symbol of an oToken is determined by the underlying, strike, collateral, expiry and strike price.

The mint and burn privilege is only granted to the Controller. When a seller wants to sell an option, the controller checks if the seller’s vault is properly collateralized, and then calls **mintOtoken** function on corresponding oToken contract; when sellers close their positions or buyers redeem their oTokens, the Controller will use the **burnOtoken** function.

Otoken is the ERC20 token for an option. The Otoken inherits ERC20Initializable because we need to use the init instead of constructor.

### Functions

#### `init(address _addressBook, address _underlyingAsset, address _strikeAsset, address _collateralAsset, uint256 _strikePrice, uint256 _expiryTimestamp, bool _isPut)` 

initialize the otoken.

**Parameters:**

* `_underlyingAsset`: asset that the option references
* `_strikeAsset`: asset that the strike price is denominated in
* `_collateralAsset`: asset that is held as collateral against short/written options
* `_strikePrice`: strike price with decimals = 8
* `_expiryTimestamp`: time of the option represented by unix timestamp
* `_isPut`: is this a put option, if not it is a call

#### `mintOtoken(address account, uint256 amount)`

Mint oToken for an account. Can only be called by the [Controller](controller.md).

**Parameters:**

* `account`: the account to mint token to
* `amount`: the amount to mint

#### `burnOtoken(address account, uint256 amount)`

Burn oToken from an account. Can only be called by the [Controller](controller.md).

**Parameters:**

* `account`: the account to burn token from
* `amount`: the amount to burn

#### 



