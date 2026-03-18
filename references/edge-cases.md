# Edge Cases

Load when handling unusual scenarios or errors.

## Symbol Precision

Always round quantity/price to symbol rules. Fetch from `getExchangeInfo()`:
- `lotSize` / `stepSize` — quantity step
- `tickSize` — price step
- `minQty` — minimum order size

## Common Errors

| Error | Cause | Fix |
|-------|-------|-----|
| -1013 Invalid quantity | Quantity not multiple of lot size | Round to stepSize |
| -1111 Precision | Price/quantity precision wrong | Use tickSize/lotSize from exchange info |
| -2010 Insufficient balance | Not enough margin/balance | Check getBalance/getBalances first |
| -4164 Position side | Hedge mode requires posSide | Add positionSide: 'LONG' or 'SHORT' |

## Futures Contract Size

USD-M: `quantity` = number of contracts. Each contract = base amount (e.g. 0.001 BTC for BTCUSDT).
COIN-M: Same; settlement in base coin.

Convert "trade $X USDT" → `quantity = X / (markPrice * contractSize)` then round.
