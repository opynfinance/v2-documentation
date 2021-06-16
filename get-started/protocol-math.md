# Protocol Math

The protocol uses fixed point 256 bit integers \([FixedPointInt256.sol](https://github.com/opynfinance/GammaProtocol/blob/master/contracts/libs/FixedPointInt256.sol)\) to allow for precision. Numbers are thus scaled to fit this system using a base of 27 to ensure necessary precision. For example:

* `5` gets converted to `5**27` scaling the integer by the base decimals of 27
* USDC has 6 decimals. 5 USDC = `5 * 1e6` USDC, and is converted to `5 * 1e27` as a fixed point int
* cUSDC has 8 decimals. 0.05 cUSDC = `5 * 1e6` cUSDC, and is converted to `5 * 1e25` as a fixed point int

The math library has functions to do these conversions for you 

* [`fromUnscaledInt`](https://github.com/opynfinance/GammaProtocol/blob/0733a4aa2cbc0409488f6fce37b2bcd9aaf66c61/contracts/libs/FixedPointInt256.sol#L33): converts normal integers into scaled fixed point ints
* [`fromScaledUint`](https://github.com/opynfinance/GammaProtocol/blob/0733a4aa2cbc0409488f6fce37b2bcd9aaf66c61/contracts/libs/FixedPointInt256.sol#L48): converts scaled uints into scaled fixed point ints

## oTokens, Underlying, and Collateral Decimals 

_Note that all oTokens have the same number of decimals._

| Token | Decimals |
| :--- | :--- |
| oToken | 8  |
| WETH  | 18 |
| USDC | 6 |
| WBTC | 8 |

