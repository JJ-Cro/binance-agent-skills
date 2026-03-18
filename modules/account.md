# Binance Account Module

Balances, transfers, and account info with `MainClient` and `USDMClient`.

## Spot Balance (MainClient)

```typescript
import { MainClient } from 'binance';
const client = new MainClient({ api_key, api_secret });

const balances = await client.getBalances();
const usdt = balances.find(b => b.coin === 'USDT');
// usdt.free, usdt.locked
```

## Futures Balance (USDMClient)

```typescript
import { USDMClient } from 'binance';
const client = new USDMClient({ api_key, api_secret });

const balances = await client.getBalance();
// balances[].balance, availableBalance, totalUnrealizedProfit
```

## Internal Transfer (MainClient)

```typescript
// Spot ↔ USD-M futures
await client.universalTransfer({
  type: 'MAIN_UMFUTURE',  // Spot to USD-M
  asset: 'USDT',
  amount: 100,
});

// Types: MAIN_UMFUTURE, UMFUTURE_MAIN, MAIN_CMFUTURE, CMFUTURE_MAIN, etc.
```

## Deposit Address

```typescript
const addr = await client.getDepositAddress({ coin: 'BTC' });
```

## Key Rules

- Spot uses `getBalances()`; futures uses `getBalance()`
- Transfer types: `MAIN_UMFUTURE` (spot→usdm), `UMFUTURE_MAIN` (usdm→spot), etc.
- Check `client.getAccountInformation()` for account-level restrictions
