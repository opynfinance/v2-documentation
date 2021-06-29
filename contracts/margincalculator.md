# MarginCalculator

## **Overview**

**MarginCalculator** is a calculation module for our vaults. It only does 2 jobs: 

1. determine if a vault is valid \(properly collateralized\), 
2. calculate the expected payout for a oToken based on oracle price at expiry.

Calculator module that checks if a given vault is valid, calculates margin requirements, and settlement proceeds.

### Functions:

* `constructor(address _oracle) (public)`
* `setCollateralDust(address _collateral, uint256 _dust) (external)`
* `setUpperBoundValues(address _underlying, address _strike, address _collateral, bool _isPut, uint256[] _timesToExpiry, uint256[] _values) (external)`
* `updateUpperBoundValue(address _underlying, address _strike, address _collateral, bool _isPut, uint256 _timeToExpiry, uint256 _value) (external)`
* `setSpotShock(address _underlying, address _strike, address _collateral, bool _isPut, uint256 _shockValue) (external)`
* `setOracleDeviation(uint256 _deviation) (external)`
* `getCollateralDust(address _collateral) (external)`
* `getTimesToExpiry(address _underlying, address _strike, address _collateral, bool _isPut) (external)`
* `getMaxPrice(address _underlying, address _strike, address _collateral, bool _isPut, uint256 _timeToExpiry) (external)`
* `getSpotShock(address _underlying, address _strike, address _collateral, bool _isPut) (external)`
* `getOracleDeviation() (external)`
* `getNakedMarginRequired(address _underlying, address _strike, address _collateral, uint256 _shortAmount, uint256 _strikePrice, uint256 _underlyingPrice, uint256 _shortExpiryTimestamp, uint256 _collateralDecimals, bool _isPut) (external)`
* `getExpiredPayoutRate(address _otoken) (external)`
* `isLiquidatable(struct MarginVault.Vault _vault, uint256 _vaultType, uint256 _vaultLatestUpdate, uint256 _roundId) (external)`
* `getMarginRequired(struct MarginVault.Vault _vault, uint256 _vaultType) (external)`
* `getExcessCollateral(struct MarginVault.Vault _vault, uint256 _vaultType) (public)`
* `_getExpiredCashValue(address _underlying, address _strike, uint256 _expiryTimestamp, uint256 _strikePrice, bool _isPut) (internal)`
* `_getMarginRequired(struct MarginVault.Vault _vault, struct MarginCalculator.VaultDetails _vaultDetails) (internal)`
* `_getNakedMarginRequired(bytes32 _productHash, struct FixedPointInt256.FixedPointInt _shortAmount, struct FixedPointInt256.FixedPointInt _underlyingPrice, struct FixedPointInt256.FixedPointInt _strikePrice, uint256 _shortExpiryTimestamp, bool _isPut) (internal)`
* `_findUpperBoundValue(bytes32 _productHash, uint256 _expiryTimestamp) (internal)`
* `_getPutSpreadMarginRequired(struct FixedPointInt256.FixedPointInt _shortAmount, struct FixedPointInt256.FixedPointInt _longAmount, struct FixedPointInt256.FixedPointInt _shortStrike, struct FixedPointInt256.FixedPointInt _longStrike) (internal)`
* `_getCallSpreadMarginRequired(struct FixedPointInt256.FixedPointInt _shortAmount, struct FixedPointInt256.FixedPointInt _longAmount, struct FixedPointInt256.FixedPointInt _shortStrike, struct FixedPointInt256.FixedPointInt _longStrike) (internal)`
* `_convertAmountOnLivePrice(struct FixedPointInt256.FixedPointInt _amount, address _assetA, address _assetB) (internal)`
* `_convertAmountOnExpiryPrice(struct FixedPointInt256.FixedPointInt _amount, address _assetA, address _assetB, uint256 _expiry) (internal)`
* `_getDebtPrice(struct FixedPointInt256.FixedPointInt _vaultCollateral, struct FixedPointInt256.FixedPointInt _vaultDebt, struct FixedPointInt256.FixedPointInt _cashValue, struct FixedPointInt256.FixedPointInt _spotPrice, uint256 _auctionStartingTime, uint256 _collateralDecimals, bool _isPut) (internal)`
* `_getVaultDetails(struct MarginVault.Vault _vault, uint256 _vaultType) (internal)`
* `_getExpiredSpreadCashValue(struct FixedPointInt256.FixedPointInt _shortAmount, struct FixedPointInt256.FixedPointInt _longAmount, struct FixedPointInt256.FixedPointInt _shortCashValue, struct FixedPointInt256.FixedPointInt _longCashValue) (internal)`
* `_isNotEmpty(address[] _assets) (internal)`
* `_checkIsValidVault(struct MarginVault.Vault _vault, struct MarginCalculator.VaultDetails _vaultDetails) (internal)`
* `_isMarginableLong(struct MarginVault.Vault _vault, struct MarginCalculator.VaultDetails _vaultDetails) (internal)`
* `_isMarginableCollateral(struct MarginVault.Vault _vault, struct MarginCalculator.VaultDetails _vaultDetails) (internal)`
* `_getProductHash(address _underlying, address _strike, address _collateral, bool _isPut) (internal)`
* `_getCashValue(struct FixedPointInt256.FixedPointInt _strikePrice, struct FixedPointInt256.FixedPointInt _underlyingPrice, bool _isPut) (internal)`
* `_getOtokenDetails(address _otoken) (internal)`

### Events:

* `CollateralDustUpdated(address collateral, uint256 dust)`
* `TimeToExpiryAdded(bytes32 productHash, uint256 timeToExpiry)`
* `MaxPriceAdded(bytes32 productHash, uint256 timeToExpiry, uint256 value)`
* `MaxPriceUpdated(bytes32 productHash, uint256 timeToExpiry, uint256 oldValue, uint256 newValue)`
* `SpotShockUpdated(bytes32 product, uint256 spotShock)`
* `OracleDeviationUpdated(uint256 oracleDeviation)`

## Functions

### Function `constructor(address _oracle) public`

constructor

**Parameters:**

* `_oracle`: oracle module address

### Function `setCollateralDust(address _collateral, uint256 _dust) external`

set dust amount for collateral asset

can only be called by owner

**Parameters:**

* `_collateral`: collateral asset address
* `_dust`: dust amount, should be scaled by collateral asset decimals

### Function `setUpperBoundValues(address _underlying, address _strike, address _collateral, bool _isPut, uint256[] _timesToExpiry, uint256[] _values) external`

set product upper bound values

can only be called by owner

**Parameters:**

* `_underlying`: otoken underlying asset
* `_strike`: otoken strike asset
* `_collateral`: otoken collateral asset
* `_isPut`: otoken type
* `_timesToExpiry`: array of times to expiry timestamp
* `_values`: upper bound values array

### Function `updateUpperBoundValue(address _underlying, address _strike, address _collateral, bool _isPut, uint256 _timeToExpiry, uint256 _value) external`

set option upper bound value for specific time to expiry \(1e27\)

can only be called by owner

**Parameters:**

* `_underlying`: otoken underlying asset
* `_strike`: otoken strike asset
* `_collateral`: otoken collateral asset
* `_isPut`: otoken type
* `_timeToExpiry`: option time to expiry timestamp
* `_value`: upper bound value

### Function `setSpotShock(address _underlying, address _strike, address _collateral, bool _isPut, uint256 _shockValue) external`

set spot shock value, scaled to 1e27

can only be called by owner

**Parameters:**

* `_underlying`: otoken underlying asset
* `_strike`: otoken strike asset
* `_collateral`: otoken collateral asset
* `_isPut`: otoken type
* `_shockValue`: spot shock value

### Function `setOracleDeviation(uint256 _deviation) external`

set oracle deviation \(1e27\)

can only be called by owner

**Parameters:**

* `_deviation`: deviation value

### Function `getCollateralDust(address _collateral) → uint256 external`

get dust amount for collateral asset

**Parameters:**

* `_collateral`: collateral asset address

**Return Values:**

* dust amount

### Function `getTimesToExpiry(address _underlying, address _strike, address _collateral, bool _isPut) → uint256[] external`

get times to expiry for a specific product

**Parameters:**

* `_underlying`: otoken underlying asset
* `_strike`: otoken strike asset
* `_collateral`: otoken collateral asset
* `_isPut`: otoken type

**Return Values:**

* array of times to expiry

### Function `getMaxPrice(address _underlying, address _strike, address _collateral, bool _isPut, uint256 _timeToExpiry) → uint256 external`

get option upper bound value for specific time to expiry

**Parameters:**

* `_underlying`: otoken underlying asset
* `_strike`: otoken strike asset
* `_collateral`: otoken collateral asset
* `_isPut`: otoken type
* `_timeToExpiry`: option time to expiry timestamp

**Return Values:**

* option upper bound value \(1e27\)

### Function `getSpotShock(address _underlying, address _strike, address _collateral, bool _isPut) → uint256 external`

get spot shock value

**Parameters:**

* `_underlying`: otoken underlying asset
* `_strike`: otoken strike asset
* `_collateral`: otoken collateral asset
* `_isPut`: otoken type

**Return Values:**

* \_shockValue spot shock value \(1e27\)

### Function `getOracleDeviation() → uint256 external`

get oracle deviation

**Return Values:**

* oracle deviation value \(1e27\)

### Function `getNakedMarginRequired(address _underlying, address _strike, address _collateral, uint256 _shortAmount, uint256 _strikePrice, uint256 _underlyingPrice, uint256 _shortExpiryTimestamp, uint256 _collateralDecimals, bool _isPut) → uint256 external`

return the collateral required for naked margin vault, in collateral asset decimals

\_shortAmount, \_strikePrice and \_underlyingPrice should be scaled by 1e8

**Parameters:**

* `_underlying`: underlying asset address
* `_strike`: strike asset address
* `_collateral`: collateral asset address
* `_shortAmount`: amount of short otoken
* `_strikePrice`: otoken strike price
* `_underlyingPrice`: otoken underlying price
* `_shortExpiryTimestamp`: otoken expiry timestamp
* `_collateralDecimals`: otoken collateral asset decimals
* `_isPut`: otoken type

**Return Values:**

* collateral required for a naked margin vault, in collateral asset decimals

### Function `getExpiredPayoutRate(address _otoken) → uint256 external`

return the cash value of an expired oToken, denominated in collateral

**Parameters:**

* `_otoken`: oToken address

**Return Values:**

* how much collateral can be taken out by 1 otoken unit, scaled by 1e8,

or how much collateral can be taken out for 1 \(1e8\) oToken

### Function `isLiquidatable(struct MarginVault.Vault _vault, uint256 _vaultType, uint256 _vaultLatestUpdate, uint256 _roundId) → bool, uint256, uint256 external`

check if a specific vault is undercollateralized at a specific chainlink round

if the vault is of type 0, the function will revert

**Parameters:**

* `_vault`: vault struct
* `_vaultType`: vault type \(0 for max loss/spread and 1 for naked margin vault\)
* `_vaultLatestUpdate`: vault latest update \(timestamp when latest vault state change happened\)
* `_roundId`: chainlink round id

**Return Values:**

* true if vault is undercollateralized, liquidation price and collateral dust amount

### Function `getMarginRequired(struct MarginVault.Vault _vault, uint256 _vaultType) → struct FixedPointInt256.FixedPointInt, struct FixedPointInt256.FixedPointInt external`

calculate required collateral margin for a vault

**Parameters:**

* `_vault`: theoretical vault that needs to be checked
* `_vaultType`: vault type

**Return Values:**

* the vault collateral amount, and marginRequired the minimal amount of collateral needed in a vault, scaled to 1e27

### Function `getExcessCollateral(struct MarginVault.Vault _vault, uint256 _vaultType) → uint256, bool public`

returns the amount of collateral that can be removed from an actual or a theoretical vault

return amount is denominated in the collateral asset for the oToken in the vault, or the collateral asset in the vault

**Parameters:**

* `_vault`: theoretical vault that needs to be checked
* `_vaultType`: vault type \(0 for spread/max loss, 1 for naked margin\)

**Return Values:**

* excessCollateral the amount by which the margin is above or below the required amount
* isExcess True if there is excess margin in the vault, False if there is a deficit of margin in the vault

if True, collateral can be taken out from the vault, if False, additional collateral needs to be added to vault



## Events 

### Event `CollateralDustUpdated(address collateral, uint256 dust)`

emits an event when collateral dust is updated

### Event `TimeToExpiryAdded(bytes32 productHash, uint256 timeToExpiry)`

emits an event when new time to expiry is added for a specific product

### Event `MaxPriceAdded(bytes32 productHash, uint256 timeToExpiry, uint256 value)`

emits an event when new upper bound value is added for a specific time to expiry timestamp

### Event `MaxPriceUpdated(bytes32 productHash, uint256 timeToExpiry, uint256 oldValue, uint256 newValue)`

emits an event when updating upper bound value at specific expiry timestamp

### Event `SpotShockUpdated(bytes32 product, uint256 spotShock)`

emits an event when spot shock value is updated for a specific product

### Event `OracleDeviationUpdated(uint256 oracleDeviation)`

emits an event when oracle deviation value is updated

