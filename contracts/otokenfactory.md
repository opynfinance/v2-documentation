# oTokenFactory

**oTokenFactory** is the factory contract that creates clones of oToken contracts with EIP1167 minimal proxy. The factory is also responsible for registering the valid oToken to the whitelist module after each creation.

This module also keep track of all the oToken addresses it creates, enable everyone to query oToken addresses based on the product parameters. 

The function getTargetOtokenAddress can enable more advanced usage by calculating the oToken address before the oToken is deployed.

### Functions

####  `createOtoken(address _underlyingAsset, address _strikeAsset, address _collateralAsset, uint256 _strikePrice, uint256 _expiry, bool _isPut) → address` 

Create new otokens. This function deploys an EIP minimal proxy with CREATE2 and register it to the whitelist module.

**Parameters:**

* `_underlyingAsset`: asset that the option references
* `_strikeAsset`: asset that the strike price is denominated in
* `_collateralAsset`: asset that is held as collateral against short/written options
* `_strikePrice`: strike price with decimals = 8
* `_expiry`: expiration timestamp in second
* `_isPut`: is this a put option, if not it is a call

**Return Values:**

* newOtoken address of the newly created option

#### `getOtokensLength() → uint256`

Get the total otokens created by the factory.

**Return Values:**

* length of the otokens array.

#### `getOtoken(address _underlyingAsset, address _strikeAsset, address _collateralAsset, uint256 _strikePrice, uint256 _expiry, bool _isPut) → address` 

get the otoken address. If no token has been created with these parameters, will return address\(0\).

**Parameters:**

* `_underlyingAsset`: asset that the option references
* `_strikeAsset`: asset that the strike price is denominated in
* `_collateralAsset`: asset that is held as collateral against short/written options
* `_strikePrice`: strike price with decimals = 8
* `_expiry`: expiration timestamp in second
* `_isPut`: is this a put option, if not it is a call

**Return Values:**

* otoken the address of target otoken.

#### `getTargetOtokenAddress(address _underlyingAsset, address _strikeAsset, address _collateralAsset, uint256 _strikePrice, uint256 _expiry, bool _isPut) → address` 

get the address at which a new otoken with these parameters will be deployed

return the exact address that will be deployed at with \_computeAddress

**Parameters:**

* `_underlyingAsset`: asset that the option references
* `_strikeAsset`: asset that the strike price is denominated in
* `_collateralAsset`: asset that is held as collateral against short/written options
* `_strikePrice`: strike price with decimals = 8
* `_expiry`: expiration timestamp in second
* `_isPut`: is this a put option, if not it is a call

**Return Values:**

* targetAddress the address this otoken will be deployed at.

#### 

### Events

#### `OtokenCreated(address tokenAddress, address creator, address underlying, address strike, address collateral, uint256 strikePrice, uint256 expiry, bool isPut)`

emitted when factory create a new Option

#### 

\`\`

