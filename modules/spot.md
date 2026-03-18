# Binance Spot Module

Spot trading with `MainClient` from the `binance` package.

## Client Setup

```typescript
import { MainClient } from 'binance';

const client = new MainClient({
  api_key: process.env.BINANCE_API_KEY,
  api_secret: process.env.BINANCE_API_SECRET,
  beautifyResponses: true,
  testnet: false,  // or true for testnet
});
```

## Common Operations

### Market Order

```typescript
await client.submitNewOrder({
  symbol: 'BTCUSDT',
  side: 'BUY',   // or 'SELL'
  type: 'MARKET',
  quantity: 0.01,  // base asset (e.g. BTC amount for BTCUSDT)
});
```

### Limit Order

```typescript
await client.submitNewOrder({
  symbol: 'BTCUSDT',
  side: 'BUY',
  type: 'LIMIT',
  quantity: 0.01,
  price: 50000,
  timeInForce: 'GTC',  // or 'IOC', 'FOK'
});
```

### Cancel Order

```typescript
await client.cancelOrder({ symbol: 'BTCUSDT', orderId: orderId });
// or by clientOrderId:
await client.cancelOrder({ symbol: 'BTCUSDT', origClientOrderId: clientOrderId });
```

### Get Orders

```typescript
const open = await client.getOpenOrders({ symbol: 'BTCUSDT' });
const all = await client.getAllOrders({ symbol: 'BTCUSDT' });
```

### Get Balance

```typescript
const balances = await client.getBalances();
const usdt = balances.find(b => b.coin === 'USDT');
```

## Workflow: Market Buy with Quote Amount (e.g. "buy $500 of BTC")

1. `getSymbolPriceTicker({ symbol: 'BTCUSDT' })` → price
2. `quantity = quoteAmount / price` (e.g. 500 / 95000 ≈ 0.00526)
3. Round to symbol precision from `getExchangeInfo()` (lotSize, stepSize)
4. `submitNewOrder({ symbol, side: 'BUY', type: 'MARKET', quantity })`

## Key Rules

- `quantity` for spot is in **base asset** (e.g. BTC for BTCUSDT)
- For market buy with quote amount: compute `quantity = quoteAmount / price`, then round to lot size
- Use `client.getExchangeInfo()` for `lotSize`, `minQty`, `stepSize` when rounding
