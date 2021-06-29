# Oracle

## Overview

**Oracle** is the oracle contract for the whole v2 system, [MarginCalculator](margincalculator.md) will constantly read token price from oracle to determine if a vault is properly collateralized. 

The oracle has 3 jobs: 

1. Return real time price for an asset.
2. Return stored price for an asset at specific timestamp \(for settlement\).
3. Route request to Chainlink aggregator to get price at specific timestamp, this is only used for liquidation feature. 

The pricer roles are defined for each asset type. A Pricer is an interface that Oracle can just query and get the live Price, and also responsible for submitting the price for a specific timestamp to Oracle.  

We define a **locking period** for each Pricer, which determines how long the oracle will wait after expiry to start receiving price updates. During the locking period, the oracle will not accept any price submission.

After the price is submitted by the pricer, there’s a **dispute period**, in which the **disputer** can come and override the price. After the dispute period, the price is considered finalized, and can be used to calculate the payout for buyers and sellers.

All the prices in the Oracle are denominated in USD, and have 8 decimal places

### Functions:

* `migrateOracle(address _asset, uint256[] _expiries, uint256[] _prices) (external)`
* `endMigration() (external)`
* `setAssetPricer(address _asset, address _pricer) (external)`
* `setLockingPeriod(address _pricer, uint256 _lockingPeriod) (external)`
* `setDisputePeriod(address _pricer, uint256 _disputePeriod) (external)`
* `setDisputer(address _disputer) (external)`
* `setStablePrice(address _asset, uint256 _price) (external)`
* `disputeExpiryPrice(address _asset, uint256 _expiryTimestamp, uint256 _price) (external)`
* `setExpiryPrice(address _asset, uint256 _expiryTimestamp, uint256 _price) (external)`
* `getPrice(address _asset) (external)`
* `getExpiryPrice(address _asset, uint256 _expiryTimestamp) (external)`
* `getPricer(address _asset) (external)`
* `getDisputer() (external)`
* `getPricerLockingPeriod(address _pricer) (external)`
* `getPricerDisputePeriod(address _pricer) (external)`
* `getChainlinkRoundData(address _asset, uint80 _roundId) (external)`
* `isLockingPeriodOver(address _asset, uint256 _expiryTimestamp) (public)`
* `isDisputePeriodOver(address _asset, uint256 _expiryTimestamp) (public)`

### Events:

* `DisputerUpdated(address newDisputer)`
* `PricerUpdated(address asset, address pricer)`
* `PricerLockingPeriodUpdated(address pricer, uint256 lockingPeriod)`
* `PricerDisputePeriodUpdated(address pricer, uint256 disputePeriod)`
* `ExpiryPriceUpdated(address asset, uint256 expiryTimestamp, uint256 price, uint256 onchainTimestamp)`
* `ExpiryPriceDisputed(address asset, uint256 expiryTimestamp, uint256 disputedPrice, uint256 newPrice, uint256 disputeTimestamp)`
* `StablePriceUpdated(address asset, uint256 price)`

## Functions

### Function `migrateOracle(address _asset, uint256[] _expiries, uint256[] _prices) external`

function to mgirate asset prices from old oracle to new deployed oracle

this can only be called by owner, should be used at the deployment time before setting Oracle module into AddressBook

**Parameters:**

* `_asset`: asset address
* `_expiries`: array of expiries timestamps
* `_prices`: array of prices

### Function `endMigration() external`

end migration process

can only be called by owner, should be called before setting Oracle module into AddressBook

### Function `setAssetPricer(address _asset, address _pricer) external`

sets the pricer for an asset

can only be called by the owner

**Parameters:**

* `_asset`: asset address
* `_pricer`: pricer address

### Function `setLockingPeriod(address _pricer, uint256 _lockingPeriod) external`

sets the locking period for a pricer

can only be called by the owner

**Parameters:**

* `_pricer`: pricer address
* `_lockingPeriod`: locking period

### Function `setDisputePeriod(address _pricer, uint256 _disputePeriod) external`

sets the dispute period for a pricer

can only be called by the owner

for a composite pricer \(ie CompoundPricer\) that depends on or calls other pricers, ensure

that the dispute period for the composite pricer is longer than the dispute period for the

asset pricer that it calls to ensure safe usage as a dispute in the other pricer will cause

the need for a dispute with the composite pricer's price

**Parameters:**

* `_pricer`: pricer address
* `_disputePeriod`: dispute period

### Function `setDisputer(address _disputer) external`

set the disputer address

can only be called by the owner

**Parameters:**

* `_disputer`: disputer address

### Function `setStablePrice(address _asset, uint256 _price) external`

set stable asset price

price should be scaled by 1e8

**Parameters:**

* `_asset`: asset address
* `_price`: price

### Function `disputeExpiryPrice(address _asset, uint256 _expiryTimestamp, uint256 _price) external`

dispute an asset price during the dispute period

only the disputer can dispute a price during the dispute period, by setting a new one

**Parameters:**

* `_asset`: asset address
* `_expiryTimestamp`: expiry timestamp
* `_price`: the correct price

### Function `setExpiryPrice(address _asset, uint256 _expiryTimestamp, uint256 _price) external`

submits the expiry price to the oracle, can only be set from the pricer

asset price can only be set after the locking period is over and before the dispute period has started

**Parameters:**

* `_asset`: asset address
* `_expiryTimestamp`: expiry timestamp
* `_price`: asset price at expiry

### Function `getPrice(address _asset) → uint256 external`

get a live asset price from the asset's pricer contract

**Parameters:**

* `_asset`: asset address

**Return Values:**

* price scaled by 1e8, denominated in USD

e.g. 17568900000 =&gt; 175.689 USD

### Function `getExpiryPrice(address _asset, uint256 _expiryTimestamp) → uint256, bool external`

get the asset price at specific expiry timestamp

**Parameters:**

* `_asset`: asset address
* `_expiryTimestamp`: expiry timestamp

**Return Values:**

* price scaled by 1e8, denominated in USD
* isFinalized True, if the price is finalized, False if not

### Function `getPricer(address _asset) → address external`

get the pricer for an asset

**Parameters:**

* `_asset`: asset address

**Return Values:**

* pricer address

### Function `getDisputer() → address external`

get the disputer address

**Return Values:**

* disputer address

### Function `getPricerLockingPeriod(address _pricer) → uint256 external`

get a pricer's locking period

locking period is the period of time after the expiry timestamp where a price can not be pushed

during the locking period an expiry price can not be submitted to this contract

**Parameters:**

* `_pricer`: pricer address

**Return Values:**

* locking period

### Function `getPricerDisputePeriod(address _pricer) → uint256 external`

get a pricer's dispute period

dispute period is the period of time after an expiry price has been pushed where a price can be disputed

during the dispute period, the disputer can dispute the submitted price and modify it

**Parameters:**

* `_pricer`: pricer address

**Return Values:**

* dispute period

### Function `getChainlinkRoundData(address _asset, uint80 _roundId) → uint256, uint256 external`

get historical asset price and timestamp

if asset is a stable asset, will return stored price and timestamp equal to now

**Parameters:**

* `_asset`: asset address to get it's historical price
* `_roundId`: chainlink round id

**Return Values:**

* price and round timestamp

### Function `isLockingPeriodOver(address _asset, uint256 _expiryTimestamp) → bool public`

check if the locking period is over for setting the asset price at a particular expiry timestamp

**Parameters:**

* `_asset`: asset address
* `_expiryTimestamp`: expiry timestamp

**Return Values:**

* True if locking period is over, False if not

### Function `isDisputePeriodOver(address _asset, uint256 _expiryTimestamp) → bool public`

check if the dispute period is over

**Parameters:**

* `_asset`: asset address
* `_expiryTimestamp`: expiry timestamp

**Return Values:**

* True if dispute period is over, False if not

## Events

### Event `DisputerUpdated(address newDisputer)`

emits an event when the disputer is updated

### Event `PricerUpdated(address asset, address pricer)`

emits an event when the pricer is updated for an asset

### Event `PricerLockingPeriodUpdated(address pricer, uint256 lockingPeriod)`

emits an event when the locking period is updated for a pricer

### Event `PricerDisputePeriodUpdated(address pricer, uint256 disputePeriod)`

emits an event when the dispute period is updated for a pricer

### Event `ExpiryPriceUpdated(address asset, uint256 expiryTimestamp, uint256 price, uint256 onchainTimestamp)`

emits an event when an expiry price is updated for a specific asset

### Event `ExpiryPriceDisputed(address asset, uint256 expiryTimestamp, uint256 disputedPrice, uint256 newPrice, uint256 disputeTimestamp)`

emits an event when the disputer disputes a price during the dispute period

### Event `StablePriceUpdated(address asset, uint256 price)`

emits an event when a stable asset price changes

