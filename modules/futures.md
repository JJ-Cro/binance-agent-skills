# Binance Futures Module

USD-M and COIN-M futures with `USDMClient` and `CoinMClient`.

## Client Setup

```typescript
// USD-M (USDT-margined) futures
import { USDMClient } from 'binance';
const usdm = new USDMClient({
  api_key: process.env.BINANCE_API_KEY,
  api_secret: process.env.BINANCE_API_SECRET,
  testnet: false,
});

// COIN-M (coin-margined) futures
import { CoinMClient } from 'binance';
const coinm = new CoinMClient({
  api_key: process.env.BINANCE_API_KEY,
  api_secret: process.env.BINANCE_API_SECRET,
  testnet: false,
});
```

## USD-M Futures

### Place Order (market)

```typescript
await usdm.submitNewOrder({
  symbol: 'BTCUSDT',
  side: 'BUY',   // BUY = long, SELL = short
  type: 'MARKET',
  quantity: 0.001,  // contracts (not USDT amount)
});
```

### Leverage

```typescript
await usdm.setLeverage({ symbol: 'BTCUSDT', leverage: 10 });
const brackets = await usdm.getNotionalAndLeverageBrackets({ symbol: 'BTCUSDT' });
```

### Positions

```typescript
const positions = await usdm.getPositions();
// Filter non-zero: positions.filter(p => Number(p.positionAmt) !== 0)
```

### Close Position (market)

Place opposite order with `reduceOnly: true`, or use quantity = position size with opposite side.

```typescript
// If long 0.001 BTC, sell to close:
await usdm.submitNewOrder({
  symbol: 'BTCUSDT',
  side: 'SELL',
  type: 'MARKET',
  quantity: 0.001,
  reduceOnly: true,
});
```

### TP/SL (stop orders)

```typescript
await usdm.submitNewOrder({
  symbol: 'BTCUSDT',
  side: 'SELL',
  type: 'TAKE_PROFIT_MARKET',  // or STOP_MARKET, TRAILING_STOP_MARKET
  quantity: 0.001,
  stopPrice: 95000,
  reduceOnly: true,
});
```

## COIN-M Futures

Same pattern; `quantity` is in contracts. Settlement and margin are in the base coin (e.g. BTC for BTCUSD_PERP).

## Key Rules

- `quantity` for futures = **number of contracts**, not USDT
- Use `usdm.getExchangeInfo()` for `contractSize`, `minQty`, `lotSize`
- For "open long with $X USDT": `quantity = usdtAmount / (markPrice * contractSize)` then round to lot size
