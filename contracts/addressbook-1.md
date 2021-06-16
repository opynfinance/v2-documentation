---
description: >-
  AddressBook provides addresses for all modules in v2, and also serves as the
  admin of each proxy module, which we interact with to upgrade each module.
---

# AddressBook

### Functions

#### `getOtokenImpl() → address` 

return otoken implementation address

**Return Values:**

* otoken implementation address

#### `getOtokenFactory() → address` 

return otoken factory address

**Return Values:**

* otoken factory address

#### `getWhitelist() → address` 

return whitelist address

**Return Values:**

* whitelist address

#### `getController() → address`

return controller address

**Return Values:**

* controller address

#### `getMarginPool() → address`

return pool address

**Return Values:**

* pool address

#### `getMarginCalculator() → address`

return margin calculator address

**Return Values:**

* margin calculator address

#### Function `getOracle() → address`

return oracle address

**Return Values:**

* oracle address

#### `setOtokenImpl(address _otokenImpl)`

set otoken implementation address. Can only be called by the owner

**Parameters:**

* `_otokenImpl`: otoken implementation address

#### `setOtokenFactory(address _otokenFactory)`

set otoken factory address. Can only be called by the owner

**Parameters:**

* `_otokenFactory`: otoken factory address

#### `setWhitelist(address _whitelist)`

set whitelist address. Can only be called by the owner

**Parameters:**

* `_whitelist`: whitelist address

#### `setController(address _controller)` 

upgrade the controller with a new implementation. Can only be called by the owner

**Parameters:**

* `_controller`: controller address

#### `setMarginPool(address _marginPool) external`

set pool address. Can only be called by the owner

**Parameters:**

* `_marginPool`: pool address

#### `setMarginCalculator(address _marginCalculator)`

set margin calculator address. Can only be called by the owner

**Parameters:**

* `_marginCalculator`: margin calculator address

#### `setOracle(address _oracle)` 

set oracle address. Can only be called by the owner

**Parameters:**

* `_oracle`: oracle address

#### `getAddress(bytes32 _key) → address` 

return an address for specific key

**Parameters:**

* `_key`: key address

#### `setAddress(bytes32 _key, address _address)`

set a specific address for a specific key. Can only be called by the owner

**Parameters:**

* `_key`: key
* `_address`: address

### Events

#### `ProxyCreated(bytes32 id, address proxy)`

event emitted when a new proxy get created

#### `AddressAdded(bytes32 id, address add)`

event emitted when a new address get added

