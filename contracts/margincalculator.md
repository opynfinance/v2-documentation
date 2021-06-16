# MarginCalculator

### **Overview**

**MarginCalculator** is a calculation module for our vaults. It only does 2 jobs: 

1. determine if a vault is valid \(properly collateralized\), 
2. calculate the expected payout for a oToken based on oracle price at expiry.

Calculator module that checks if a given vault is valid, calculates margin requirements, and settlement proceeds

### Functions

#### `getExpiredPayoutRate(address _otoken) → uint256` 

return the cash value of an expired oToken, denominated in collateral

**Parameters:**

* `_otoken`: oToken address

**Return Values:**

* how much collateral can be taken out by 1 otoken unit, scaled by 1e8, or how much collateral can be taken out for 1 \(1e8\) oToken



#### `getExcessCollateral(struct MarginVault.Vault _vault) → uint256, bool`

returns the amount of collateral that can be removed from an actual or a theoretical vault. Returned amount is denominated in the collateral asset for the oToken in the vault, or the collateral asset in the vault

**Parameters:**

* `_vault`: theoretical vault that needs to be checked

**Return Values:**

* `excessCollateral` the amount by which the margin is above or below the required amount
* `isExcess` True if there is excess margin in the vault, False if there is a deficit of margin in the vault

if True, collateral can be taken out from the vault, if False, additional collateral needs to be added to vault



