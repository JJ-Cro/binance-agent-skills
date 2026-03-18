# Binance Market Data Module

Public market data — no auth required for most calls. Use `MainClient` or `USDMClient` (can omit credentials for public endpoints).

## Spot (MainClient)

```typescript
import { MainClient } from 'binance';
const client = new MainClient();  // no key needed for public

const price = await client.getSymbolPriceTicker({ symbol: 'BTCUSDT' });
const depth = await client.getOrderBook({ symbol: 'BTCUSDT', limit: 10 });
const klines = await client.getKlines({ symbol: 'BTCUSDT', interval: '1h', limit: 100 });
const ticker24h = await client.get24hrTickerStatistics({ symbol: 'BTCUSDT' });
```

## USD-M Futures

```typescript
import { USDMClient } from 'binance';
const client = new USDMClient();  // no key for public

const price = await client.getMarkPrice({ symbol: 'BTCUSDT' });
const funding = await client.getFundingRate({ symbol: 'BTCUSDT' });
const openInterest = await client.getOpenInterest({ symbol: 'BTCUSDT' });
```

## Key Rules

- Public endpoints work without API keys
- `interval` for klines: `1m`, `3m`, `5m`, `15m`, `30m`, `1h`, `2h`, `4h`, `6h`, `8h`, `12h`, `1d`, `3d`, `1w`, `1M`
