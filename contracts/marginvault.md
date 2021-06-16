# MarginVault

### Overview

MarginVault is an external library that provides the Controller with a Vault struct and the functions that manipulate vaults.

`vault` is a struct of 6 arrays that describe a position a user has, a user can have multiple vaults. The `vault` structure is defined as follow:

```javascript
struct Vault {
    address[] shortOtokens;
    address[] longOtokens;
    address[] collateralAssets;
    
    uint256[] shortAmounts;
    uint256[] longAmounts;
    uint256[] collateralAmounts;
}
```

### Fields:

* `shortOtokens`: addresses of oTokens a user has shorted \(i.e. written\) against this vault
* `longOtokens`: addresses of oTokens a user has bought and deposited in this vault. User can be long oTokens without opening a vault \(e.g. by buying on a DEX\). generally, long oTokens will be 'deposited' in vaults to act as collateral in order to write oTokens against \(i.e. in spreads\)
* `collateralAssets`: addresses of other ERC-20s a user has deposited as collateral in this vault
* `shortAmounts`: quantity of oTokens minted/written for each oToken address in shortOtokens
* `longAmounts`: quantity of oTokens owned and held in the vault for each oToken address in longOtokens
* `collateralAmounts`: quantity of ERC-20 deposited as collateral in the vault for each ERC-20 address in collateralAssets

### Functions:

#### `addShort(struct MarginVault.Vault _vault, address _shortOtoken, uint256 _amount, uint256 _index)`

increase the short oToken balance in a vault when a new oToken is minted

**Parameters:**

* `_vault`: vault to add or increase the short position in
* `_shortOtoken`: address of the \_shortOtoken being minted from the user's vault
* `_amount`: number of \_shortOtoken being minted from the user's vault
* `_index`: index of \_shortOtoken in the user's vault.shortOtokens array

####  `removeShort(struct MarginVault.Vault _vault, address _shortOtoken, uint256 _amount, uint256 _index)`

decrease the short oToken balance in a vault when an oToken is burned

**Parameters:**

* `_vault`: vault to decrease short position in
* `_shortOtoken`: address of the \_shortOtoken being reduced in the user's vault
* `_amount`: number of \_shortOtoken being reduced in the user's vault
* `_index`: index of \_shortOtoken in the user's vault.shortOtokens array

####  `addLong(struct MarginVault.Vault _vault, address _longOtoken, uint256 _amount, uint256 _index)`

increase the long oToken balance in a vault when an oToken is deposited

**Parameters:**

* `_vault`: vault to add a long position to
* `_longOtoken`: address of the \_longOtoken being added to the user's vault
* `_amount`: number of \_longOtoken the protocol is adding to the user's vault
* `_index`: index of \_longOtoken in the user's vault.longOtokens array

####  `removeLong(struct MarginVault.Vault _vault, address _longOtoken, uint256 _amount, uint256 _index)`

decrease the long oToken balance in a vault when an oToken is withdrawn

**Parameters:**

* `_vault`: vault to remove a long position from
* `_longOtoken`: address of the \_longOtoken being removed from the user's vault
* `_amount`: number of \_longOtoken the protocol is removing from the user's vault
* `_index`: index of \_longOtoken in the user's vault.longOtokens array

####  `addCollateral(struct MarginVault.Vault _vault, address _collateralAsset, uint256 _amount, uint256 _index)` 

increase the collateral balance in a vault

**Parameters:**

* `_vault`: vault to add collateral to
* `_collateralAsset`: address of the \_collateralAsset being added to the user's vault
* `_amount`: number of \_collateralAsset being added to the user's vault
* `_index`: index of \_collateralAsset in the user's vault.collateralAssets array

####  `removeCollateral(struct MarginVault.Vault _vault, address _collateralAsset, uint256 _amount, uint256 _index)` 

decrease the collateral balance in a vault

**Parameters:**

* `_vault`: vault to remove collateral from
* `_collateralAsset`: address of the \_collateralAsset being removed from the user's vault
* `_amount`: number of \_collateralAsset being removed from the user's vault
* `_index`: index of \_collateralAsset in the user's vault.collateralAssets array

