# System Roles & Terms

## Admin Roles 

| **Role** | **Action** |
| :--- | :--- |
| Owner | Can whitelist/blacklist collateral and oToken addresses, update/upgrade modules, set addresses for other roles in the system, and manage oracle parameters. |
| Partial Pauser | Can pause all actions other than redeem oToken and settle vault  |
| Full Pauser | Can fully pause the system in case of an emergency.  |
| Disputer | Price disputer for the oracle contract that can dispute the price submitted by Pricer contracts, and has the privilege to override with a new price. |
| Pricer | Can submit oracle prices  |
| Farmer | Can withdraw any token excess balance in the pool. |

## Operators

Every account can define multiple operators. An operator is an address that can update the account's positions on behalf of the account. That means, an operator has the privilege to move an ERC20 token from the account's address into a vault, if the account has approved the MarginPool contracts beforehand, or it can also take out excess collateral from a vault to any address. Usually you won't want to add any EOA as operator.

Operator is a very critical role in Gamma. With this design, we can enable lots of innovation like rollover contract, auto-minting AMM to be built on top of Gamma.

Operators have full control over user funds and can take any action on behalf of a user. The only action operators cannot take is to add new operators or remove new operators.

## Glossary of Terms

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Name</b>
      </th>
      <th style="text-align:left"><b>Description</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">oToken</td>
      <td style="text-align:left">An ERC20 representation of option token.</td>
    </tr>
    <tr>
      <td style="text-align:left">Strike Price</td>
      <td style="text-align:left">Strike price with 8 decimals</td>
    </tr>
    <tr>
      <td style="text-align:left">Strike</td>
      <td style="text-align:left">Asset that the strike price is denominated in</td>
    </tr>
    <tr>
      <td style="text-align:left">Collateral</td>
      <td style="text-align:left">Asset that is held as collateral against short/written options</td>
    </tr>
    <tr>
      <td style="text-align:left">Underlying</td>
      <td style="text-align:left">Asset that the option references</td>
    </tr>
    <tr>
      <td style="text-align:left">Product</td>
      <td style="text-align:left">A unique combination of underlying, strike, collateral and Put/Call.
        <br
        />Ex: ETH:USDC:USDC:PUT</td>
    </tr>
    <tr>
      <td style="text-align:left">Put Option</td>
      <td style="text-align:left">A put option gives the buyer the right but not the obligation to sell
        the underlying ERC20 at strike price.</td>
    </tr>
    <tr>
      <td style="text-align:left">Call Option</td>
      <td style="text-align:left">A call option gives the buyer the right but not the obligation to buy
        the underlying ERC20 at strike price.</td>
    </tr>
    <tr>
      <td style="text-align:left">Expiry</td>
      <td style="text-align:left">The timestamp that option payout is determined.
        <br />After the expiry timestamp, the system will enable buyers and sellers
        to withdraw their expected payout.</td>
    </tr>
    <tr>
      <td style="text-align:left">Vault</td>
      <td style="text-align:left">
        <p>An option seller needs to open a vault to mint option tokens. The options
          minted are saved as &#x201C;short&#x201D; otokens in a vault.</p>
        <p>A seller can deposit collateral assets into a vault to mint a oToken,
          or add some long oTokens to reduce the amount of collateral needed for
          creating the short position.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">oToken ID</td>
      <td style="text-align:left">Each oToken has a unique ID which is represented by Hash (underlyingAsset,
        strikeAsset, collateralAsset, expiry, strikePrice, isPut).</td>
    </tr>
  </tbody>
</table>

