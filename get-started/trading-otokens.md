# Trading oTokens

Opyn options \(oTokens\) are ERC20s, so they can be traded on any decentralized exchange that follows the ERC20 standard.

oTokens on [opyn.co](http://opyn.co/) currently trade on [0x](https://0x.org/), but [v1.opyn.co](https://v1.opyn.co/#/) oTokens traded on [Uniswap](http://uniswap.org/), and projects integrating with Opyn have traded oTokens on [Balancer](https://twitter.com/aaveaave/status/1286371566708826112?lang=en) and [Airswap](https://trader.airswap.io/).  

## Trade on 0x 

oTokens on [opyn.co](http://opyn.co/) currently trade on [0x](https://0x.org/). You can view existing bids + asks, and buy / sell oTokens using [0x API](https://0x.org/docs/api). 

You can check out a full example implementation using 0x API at [Gamma Portal's open source codebase. ](https://github.com/antoncoding/gamma-portal/blob/master/src/utils/0x-utils.ts)

### Get bids and asks 

See [full example on Gamma Portal](https://github.com/antoncoding/gamma-portal/blob/master/src/utils/0x-utils.ts)

{% tabs %}
{% tab title="TypeScript" %}
```typescript
/**
 * get oToken:WETH stats (v1) for all options
 * @param {Array<{addr:string, decimals:number}>} options
 * @param {{addr:string, decimals:number}} quoteAsset
 * @return {Promise<Array<
 * >}
 */
export async function getBasePairAskAndBids(
  oTokens: OToken[],
  networkId: SupportedNetworks,
): Promise<OTokenOrderBook[]> {
  const filteredOTokens = oTokens // await filter0xAvailablePairs(networkId, oTokens);
  // 0x has rate limit of 6 request / 10 sec, will need to chuck array into 6 each
  const BATCH_REQUEST = 6
  const COOLDOWN = networkId === 1 ? 0.5 : 2

  const batchOTokens = filteredOTokens.reduce(
    (prev: OToken[][], curr) => {
      if (prev.length > 0 && prev[prev.length - 1].length >= BATCH_REQUEST) {
        return [...prev, [curr]]
      } else {
        const copy = [...prev]
        copy[copy.length - 1].push(curr)
        return copy
      }
    },
    [[]],
  )

  let final: OTokenOrderBook[] = []

  for (const batch of batchOTokens) {
    const [bestAskAndBids] = await Promise.all([
      Promise.map(batch, async ({ id: oTokenAddr }: OToken) => {
        const { asks, bids } = await getOTokenUSDCOrderBook(networkId, oTokenAddr)
        return {
          id: oTokenAddr,
          asks: asks.filter(record => isValidAsk(record)),
          bids: bids.filter(record => isValidBid(record)),
        }
      }),
      sleep(COOLDOWN * 1000),
    ])
    final = final.concat(bestAskAndBids)
  }

  return final
}
```
{% endtab %}
{% endtabs %}

### Buy or sell oTokens 

See [full example on Gamma Portal](https://github.com/antoncoding/gamma-portal/blob/ec2e8c87c31bb8b4d62d8d99c96b3c3cb75f1e5a/src/hooks/use0xExchange.tsx#L20)

{% tabs %}
{% tab title="TypeScript" %}
```typescript
//approve oToken to 0x if selling, USDC to 0x if buying 

//fill orders  
const fillOrders = useCallback(
    async (orders: SignedOrder[], amounts: BigNumber[]) => {
      if (!orders.length) return toast.error('No Orders selected')
      const exchange = new web3.eth.Contract(abi, addresses[networkId].zeroxExchange)

      const signatures = orders.map(order => order.signature)

      const gasPrice = getGasPriceForOrders(orders)
      const feeInEth = getProtocolFee(orders).toString()
      const amountsStr = amounts.map(amount => amount.toString())

      console.log(`only filling first order la, orders[0]`, orders[0])

      await exchange.methods
        .batchFillLimitOrders(orders, signatures, amountsStr, false)
        .send({
          from: user,
          value: web3.utils.toWei(feeInEth, 'ether'),
          gasPrice: web3.utils.toWei(gasPrice.toString(), 'gwei'),
        })
        .on('transactionHash', notifyCallback)

      track('fill-order')
    },
    [networkId, getProtocolFee, getGasPriceForOrders, notifyCallback, toast, user, web3, track],
  )
```
{% endtab %}
{% endtabs %}

### Limit Orders

See[ full example on Gamma Portal](https://github.com/antoncoding/gamma-portal/blob/ec2e8c87c31bb8b4d62d8d99c96b3c3cb75f1e5a/src/hooks/use0xExchange.tsx#L20)

{% tabs %}
{% tab title="TypeScript" %}
```typescript
 //Create limit order
 const createOrder = useCallback(
    async (
      makerToken: string,
      takerToken: string,
      makerAmount: BigNumber,
      takerAmount: BigNumber,
      expiry: number,
      takerAddress?: string,
    ) => {
      const salt = new BigNumber(Date.now()).integerValue().toString()
      const taker = takerAddress ? takerAddress : '0x0000000000000000000000000000000000000000'
      const order = new v4orderUtils.LimitOrder({
        chainId: networkId,
        makerToken,
        takerToken,
        makerAmount,
        takerAmount,
        maker: user,
        taker,
        sender: '0x0000000000000000000000000000000000000000',
        expiry: new BigNumber(expiry).integerValue(),
        salt,
      })
      track('create-order')
      const signature = await order.getSignatureWithProviderAsync(
        web3.currentProvider as any,
        v4orderUtils.SignatureType.EIP712,
      )

      return {
        ...order,
        signature,
      }
    },
    [networkId, user, web3, track],
  )
  
//Broadcast limit order 
 const broadcastOrder = useCallback(
  async (order: SignedOrder) => {
    const url = `${httpEndpoint}sra/v4/order`
    const res = await fetch(url, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(order),
    })
    if (res.status === 200) return toast.success('Order successfully broadcasted')
    const jsonRes = await res.json()
    if (jsonRes.validationErrors) return toast.error(jsonRes.validationErrors[0].reason)

    toast.error(JSON.stringify(jsonRes))
  },
  [httpEndpoint, toast],
)

//Cancel limit orders 
const cancelOrders = useCallback(
  async (orders: SignedOrder[], callback: Function) => {
    const exchange = new web3.eth.Contract(abi, addresses[networkId].zeroxExchange)

    await exchange.methods
      .batchCancelLimitOrders(orders)
      .send({
        from: user,
      })
      .on('transactionHash', notifyCallback)
    track('cancel-order')
    callback()
  },
  [web3, notifyCallback, networkId, track, user],
)
```
{% endtab %}
{% endtabs %}

### Additional Notes 

Some other factors to keep in mind when integrating with 0x: 

* Since the 0x protocol fee is determined by the gas price, you must set the gas price in the frontend for your users and warn them not to change the gas price. If they change the gas price, it will change the 0x protocol fee and cause the transaction to revert. 
* 0x orders typically expire in less than 5 minutes. To prevent users from getting reverts due to orders expiring, we recommend filtering out orders that will expire in one minute or less, and setting the default gas price to fastest. 

## Trade on Airswap 

Many projects integrating with Opyn \(eg. [Ribbon](https://ribbon.finance/), [Fontis](http://fontis.finance/), [Optional](http://optional.finance/)\) have chosen to use Airswap for making OTC oToken trades with market makers.

These projects run options strategies where they buy or sell options in bulk at predetermined times, making OTC trading a better choice. 

You can check out an example implementation using Airswap at [Ribbon's open source codebase](https://github.com/ribbon-finance/structured-products/blob/e66d36b8ef22bcfbc00f20bdd7031a6d418f5221/contracts/instruments/RibbonThetaVault.sol#L472). 

### Create and Sign Airswap Order

Use [Airswap utils](https://www.npmjs.com/package/@airswap/utils) to create an order. 

{% tabs %}
{% tab title="JavaScript" %}
```javascript
import {createOrder, signTypedDataOrder} from '@airswap/utils';

const createSignedOrder = async (
    ) => {
    const order = createOrder({
      signer: {
        wallet: "0x2343...a456", //buyer address
        token: ethers.constants.AddressZero, //payment token address eg. ETH 
        amount: 100000000000000000, //payment amount 
      },
      sender: {
        wallet: "0x234...A3", //seller address
        token: "0x2343...a456", //oToken address
        amount: 100000000000000000, //amount of oTokens to sell
      },
      affiliate: {
        wallet: ethers.constants.AddressZero, //can set a fee here if desired, here it is 0
      },
    });
    const signedOrder = await signTypedDataOrder(order, "0x243...452", "0xw423...234f");
    return signedOrder;
};
```
{% endtab %}
{% endtabs %}

### Execute Trade

Use [Airswap's swap function](https://docs.airswap.io/reference/swap#solidity), submitting the order from above

{% tabs %}
{% tab title="Solidity" %}
```javascript
AIRSWAP_CONTRACT.swap(signedOrder)
```
{% endtab %}
{% endtabs %}



