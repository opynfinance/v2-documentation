# MarginPool

### **Overview**

**MarginPool** is the contract that moves and stores all the ERC20 tokens. Users only need to approve an asset to be used by MarginPool once, then the asset can be used to create multiple different options.

To prevent money getting stuck in the pool due to accidental transfers to the pool, \(or by airdrop like UNI\), there’s a farmer role that has the privilege to distribute these excess tokens. Whenever a transfer is done by the controller, we will modify the assetBalance variable in the contract, and the farmer can take out the amount between actual balance and assetBalance.

### Functions

#### `transferToPool(address _asset, address _user, uint256 _amount)`

transfers an asset from a user to the pool

**Parameters:**

* `_asset`: address of the asset to transfer
* `_user`: address of the user to transfer assets from
* `_amount`: amount of the token to transfer from \_user

#### `transferToUser(address _asset, address _user, uint256 _amount)`

transfers an asset from the pool to a user

**Parameters:**

* `_asset`: address of the asset to transfer
* `_user`: address of the user to transfer assets to
* `_amount`: amount of the token to transfer to \_user

#### `getStoredBalance(address _asset) → uint256`

get the stored balance of an asset

**Parameters:**

* `_asset`: asset address

**Return Values:**

* asset balance

#### `batchTransferToPool(address[] _asset, address[] _user, uint256[] _amount)` 

transfers multiple assets from users to the pool

**Parameters:**

* `_asset`: addresses of the assets to transfer
* `_user`: addresses of the users to transfer assets to
* `_amount`: amount of each token to transfer to pool

#### `batchTransferToUser(address[] _asset, address[] _user, uint256[] _amount)` 

transfers multiple assets from the pool to users

**Parameters:**

* `_asset`: addresses of the assets to transfer
* `_user`: addresses of the users to transfer assets to
* `_amount`: amount of each token to transfer to \_user

#### `farm(address _asset, address _receiver, uint256 _amount)` 

function to collect the excess balance of a particular asset

can only be called by the farmer address

**Parameters:**

* `_asset`: asset address
* `_receiver`: receiver address
* `_amount`: amount to remove from pool

#### `setFarmer(address _farmer)` 

function to set farmer address. Can only be called by MarginPool owner

**Parameters:**

* `_farmer`: farmer address

### Events

#### `FarmerUpdated(address oldAddress, address newAddress)`

emit event after updating the farmer address

#### `AssetFarmed(address asset, address receiver, uint256 _amount)`

emit event when an asset gets harvested from the pool

