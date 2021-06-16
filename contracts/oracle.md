# Oracle

**Oracle** is the oracle contract for the whole v2 system, [MarginCalculator](margincalculator.md) will constantly read token price from oracle to determine if a vault is properly collateralized. 

The oracle has 2 jobs: 

1. Return real time price for an asset.
2. Return stored price for an asset at specific timestamp.

The pricer roles are defined for each asset type. A Pricer is an interface that Oracle can just query and get the live Price, and also responsible for submitting the price for a specific timestamp to Oracle.  

We define a **locking period** for each Pricer, which determines how long the oracle will wait after expiry to start receiving price updates. During the locking period, the oracle will not accept any price submission.

After the price is submitted by the pricer, there’s a **dispute period**, in which the **disputer** can come and override the price. After the dispute period, the price is considered finalized, and can be used to calculate the payout for buyers and sellers.

All the prices in the Oracle are denominated in USD, and have 8 decimal places

### Functions

#### `getPrice(address _asset) → uint256`

get a live asset price from the asset's pricer contract

**Parameters:**

* `_asset`: asset address

**Return Values:**

* price scaled by 1e8, denominated in USD

e.g. 17368900000 =&gt; 175.689 USD

#### `getExpiryPrice(address _asset, uint256 _expiryTimestamp) → uint256, bool` 

get the asset price at specific expiry timestamp

**Parameters:**

* `_asset`: asset address
* `_expiryTimestamp`: expiry timestamp

**Return Values:**

* price scaled by 1e8, denominated in USD
* isFinalized True, if the price is finalized, False if not

#### `getPricer(address _asset) → address`

get the pricer for an asset

**Parameters:**

* `_asset`: asset address

**Return Values:**

* pricer address

#### `getDisputer() → address`

get the disputer address

**Return Values:**

* disputer address

#### `getPricerLockingPeriod(address _pricer) → uint256`

get a pricer's locking period. during the locking period an expiry price can not be submitted to this contract

**Parameters:**

* `_pricer`: pricer address

**Return Values:**

* locking period

#### `getPricerDisputePeriod(address _pricer) → uint256`

get a pricer's dispute period. During the dispute period, the disputer can dispute the submitted price and modify it

**Parameters:**

* `_pricer`: pricer address

**Return Values:**

* dispute period

#### `isLockingPeriodOver(address _asset, uint256 _expiryTimestamp) → bool`

check if the locking period is over for setting the asset price at a particular expiry timestamp

**Parameters:**

* `_asset`: asset address
* `_expiryTimestamp`: expiry timestamp

**Return Values:**

* True if locking period is over, False if not

#### `isDisputePeriodOver(address _asset, uint256 _expiryTimestamp) → bool`

check if the dispute period is over

**Parameters:**

* `_asset`: asset address
* `_expiryTimestamp`: expiry timestamp

**Return Values:**

* True if dispute period is over, False if not

#### `setAssetPricer(address _asset, address _pricer)` 

sets the pricer for an asset

can only be called by the owner

**Parameters:**

* `_asset`: asset address
* `_pricer`: pricer address

#### `setLockingPeriod(address _pricer, uint256 _lockingPeriod) external`

sets the locking period for a pricer. Can only be called by the owner

**Parameters:**

* `_pricer`: pricer address
* `_lockingPeriod`: locking period

#### `setDisputePeriod(address _pricer, uint256 _disputePeriod) external`

sets the dispute period for a pricer. Can only be called by the owner.

for a composite pricer \(ie CompoundPricer\) that depends on or calls other pricers, ensure that the dispute period for the composite pricer is longer than the dispute period for the asset pricer that it calls to ensure safe usage as a dispute in the other pricer will cause the need for a dispute with the composite pricer's price

**Parameters:**

* `_pricer`: pricer address
* `_disputePeriod`: dispute period

#### `setDisputer(address _disputer) external`

set the disputer address. Can only be called by the owner

**Parameters:**

* `_disputer`: disputer address

#### `setStablePrice(address _asset, uint256 _price) external` 

sets stable asset price. Can only be called by the owner.

price should be scaled by 1e8.

**Parameters:**

* `_asset`: asset address
* `_price`: price

#### `disputeExpiryPrice(address _asset, uint256 _expiryTimestamp, uint256 _price)`

dispute an asset price during the dispute period. Only the owner can dispute a price during the dispute period, by setting a new one

**Parameters:**

* `_asset`: asset address
* `_expiryTimestamp`: expiry timestamp
* `_price`: the correct price

#### `setExpiryPrice(address _asset, uint256 _expiryTimestamp, uint256 _price)` 

submits the expiry price to the oracle, can only be set from the pricer. Asset price can only be set after the locking period is over and before the dispute period has started

**Parameters:**

* `_asset`: asset address
* `_expiryTimestamp`: expiry timestamp
* `_price`: asset price at expiry

### Events

#### `DisputerUpdated(address newDisputer)`

emits an event when the disputer is updated

#### `PricerUpdated(address asset, address pricer)`

emits an event when the pricer is updated for an asset

#### `PricerLockingPeriodUpdated(address pricer, uint256 lockingPeriod)`

emits an event when the locking period is updated for a pricer

#### `PricerDisputePeriodUpdated(address pricer, uint256 disputePeriod)`

emits an event when the dispute period is updated for a pricer

#### `ExpiryPriceUpdated(address asset, uint256 expirtyTimestamp, uint256 price, uint256 onchainTimestamp)`

emits an event when an expiry price is updated for a specific asset

#### `ExpiryPriceDisputed(address asset, uint256 expiryTimestamp, uint256 disputedPrice, uint256 newPrice, uint256 disputeTimestamp)`

emits an event when the disputer disputes a price during the dispute period

