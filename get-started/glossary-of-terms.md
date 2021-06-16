# Glossary of Terms

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

