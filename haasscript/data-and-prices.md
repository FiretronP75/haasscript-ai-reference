# Data & Prices

63 functions across 5 categories.

## Categories

- [Price Data](#price-data) (16 commands)
- [Price Market Information](#price-market-information) (12 commands)
- [Profit Information](#profit-information) (6 commands)
- [Time Information](#time-information) (19 commands)
- [Wallet](#wallet) (10 commands)

---

# Price Data

> **Note:** In a backtest, every price command returns the latest 500 candles of its interval on every update, for every interval (for example `ClosePrices()`, `ClosePrices(60)`, `ClosePrices(1440)` and `GetVolume()`). The history is loaded before the backtest start, so the full 500 candles are available on the first update. `#series` and `Count(series)` return the same length.

## BuyPrices

```lua
BuyPrices([interval], [fullCandles], [market], [hlcStyle])
```

**Aliases:** `AskPrices`

Gets the buy/ask prices.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `interval` | `number` | No | The ticks interval. |
| `fullCandles` | `boolean` | No | If true, the last closed candles will be returned. |
| `market` | `string` | No | The market of the tick. Default is the selected main market. |
| `hlcStyle` | `boolean` | No | When enabled, the data returned will be adjusted for HLC instead of OHLC. Meaning that the OHL data can change. |

**Returns:** `HaasNumberCollection`

Returns the buy/ask prices.

---

## ClosePrices

```lua
ClosePrices([interval], [fullCandles], [market], [hlcStyle])
```

**Aliases:** `Prices`

A collection with close prices. Sorted from new to old.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `interval` | `number` | No | The ticks interval. |
| `fullCandles` | `boolean` | No | If true, the last closed candles will be returned. In backtests the default series already contains only closed candles (`[1]` is the last closed candle, on the main interval and on higher intervals), so this flag returns the same series. |
| `market` | `string` | No | The market of the tick. Default is the selected main market. |
| `hlcStyle` | `boolean` | No | When enabled, each candle's open is replaced by the previous candle's close. The close, high and low are unchanged, so the open can lie above the high or below the low. |

**Returns:** `HaasNumberCollection`

Returns a collection of close prices.

Higher-interval candles are aligned to the clock. In a backtest on a 1-minute chart, `ClosePrices(60)[1]` is the last completed clock hour (minutes 0 to 59 of the previous hour); the hour still forming is not included.

---

## CurrentPrice

```lua
CurrentPrice([market])
```

Gets all the price data from the current candle.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market of the tick. Default is the selected main market. |

**Returns:** `dynamic`

Returns a container with named fields (also accessible by 1-based index).

**Container fields** (accessible by name or 1-based index):

| Index | Name | Type | Description |
|-------|------|------|-------------|
| 1 | `open` | `number` | The open price of the newest candle (index [1]). |
| 2 | `high` | `number` | The high price of the newest candle (index [1]). |
| 3 | `low` | `number` | The low price of the newest candle (index [1]). |
| 4 | `close` | `number` | The close price of the newest candle (index [1]). |
| 5 | `volume` | `number` | The volume of the newest candle. |
| 6 | `ask` | `number` | The current best ask price. Also a real quote in backtests; it can differ from `close`. |
| 7 | `bid` | `number` | The current best bid price. Also a real quote in backtests; it can differ from `close`. |

```lua
local cp = CurrentPrice()
Log(cp.ask)    -- ask price (named access)
Log(cp[6])     -- same value (index access)
Log(cp.close)  -- close price of the newest candle
```

`open`, `high`, `low` and `close` are the same values as index [1] of `OpenPrices()`, `HighPrices()`, `LowPrices()` and `ClosePrices()`.

---

## GetBodyHighPrices

```lua
GetBodyHighPrices([interval], [fullCandles], [market], [hlcStyle])
```

A collection with the highest price of the candle its body prices. Sorted from new to old.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `interval` | `number` | No | The ticks interval. |
| `fullCandles` | `boolean` | No | If true, the last closed candles will be returned. |
| `market` | `string` | No | The market of the tick. Default is the selected main market. |
| `hlcStyle` | `boolean` | No | When enabled, the data returned will be adjusted for HLC instead of OHLC. Meaning that the OHL data can change. |

**Returns:** `HaasNumberCollection`

Returns a collection of the highest price of the candle its body prices.

---

## GetBodyLowPrices

```lua
GetBodyLowPrices([interval], [fullCandles], [market], [hlcStyle])
```

A collection with the lowest price of the candle its body prices. Sorted from new to old.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `interval` | `number` | No | The ticks interval. |
| `fullCandles` | `boolean` | No | If true, the last closed candles will be returned. |
| `market` | `string` | No | The market of the tick. Default is the selected main market. |
| `hlcStyle` | `boolean` | No | When enabled, the data returned will be adjusted for HLC instead of OHLC. Meaning that the OHL data can change. |

**Returns:** `HaasNumberCollection`

Returns a collection of the lowest price of the candle its body prices.

---

## GetVolume

```lua
GetVolume([interval], [fullCandles], [market], [hlcStyle])
```

A collection with volume data. Sorted from new to old.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `interval` | `number` | No | The ticks interval. |
| `fullCandles` | `boolean` | No | If true, the last closed candles will be returned. |
| `market` | `string` | No | The market of the tick. Default is the selected main market. |
| `hlcStyle` | `boolean` | No | When enabled, the data returned will be adjusted for HLC instead of OHLC. Meaning that the OHL data can change. |

**Returns:** `HaasNumberCollection`

Returns a collection with volume data.

---

## HeikinOpenPrices

```lua
HeikinOpenPrices([interval], [fullCandles], [market])
```

Gets the Heikin-Ashi open prices.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `interval` | `number` | No | The ticks interval. |
| `fullCandles` | `boolean` | No | If true, the last closed candles will be returned. |
| `market` | `string` | No | The market of the tick. Default is the selected main market. |

**Returns:** `HaasNumberCollection`

Returns the Heikin-Ashi open prices.

---

## HighPrices

```lua
HighPrices([interval], [fullCandles], [market], [hlcStyle])
```

A collection with high prices. Sorted from new to old.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `interval` | `number` | No | The ticks interval. |
| `fullCandles` | `boolean` | No | If true, the last closed candles will be returned. In backtests the default series already contains only closed candles (`[1]` is the last closed candle, on the main interval and on higher intervals), so this flag returns the same series. |
| `market` | `string` | No | The market of the tick. Default is the selected main market. |
| `hlcStyle` | `boolean` | No | When enabled, each candle's open is replaced by the previous candle's close. The close, high and low are unchanged, so the open can lie above the high or below the low. |

**Returns:** `HaasNumberCollection`

Returns a collection of high prices.

---

## HLCPrices

```lua
HLCPrices([interval], [fullCandles], [market], [hlcStyle])
```

A collection with the average of high, low and close prices. Sorted from new to old.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `interval` | `number` | No | The ticks interval. |
| `fullCandles` | `boolean` | No | If true, the last closed candles will be returned. |
| `market` | `string` | No | The market of the tick. Default is the selected main market. |
| `hlcStyle` | `boolean` | No | When enabled, the data returned will be adjusted for HLC instead of OHLC. Meaning that the OHL data can change. |

**Returns:** `HaasNumberCollection`

Returns a collection of the average of high, low and close prices.

---

## HLPrices

```lua
HLPrices([interval], [fullCandles], [market], [hlcStyle])
```

A collection with an average of high and low prices. Sorted from new to old.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `interval` | `number` | No | The ticks interval. |
| `fullCandles` | `boolean` | No | If true, the last closed candles will be returned. |
| `market` | `string` | No | The market of the tick. Default is the selected main market. |
| `hlcStyle` | `boolean` | No | When enabled, the data returned will be adjusted for HLC instead of OHLC. Meaning that the OHL data can change. |

**Returns:** `HaasNumberCollection`

Returns a collection of an average of high and low prices.

---

## LowPrices

```lua
LowPrices([interval], [fullCandles], [market], [hlcStyle])
```

A collection with low prices. Sorted from new to old.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `interval` | `number` | No | The ticks interval. |
| `fullCandles` | `boolean` | No | If true, the last closed candles will be returned. In backtests the default series already contains only closed candles (`[1]` is the last closed candle, on the main interval and on higher intervals), so this flag returns the same series. |
| `market` | `string` | No | The market of the tick. Default is the selected main market. |
| `hlcStyle` | `boolean` | No | When enabled, each candle's open is replaced by the previous candle's close. The close, high and low are unchanged, so the open can lie above the high or below the low. |

**Returns:** `HaasNumberCollection`

Returns a collection of low prices.

---

## OCPrices

```lua
OCPrices([interval], [fullCandles], [market], [hlcStyle])
```

A collection with an average of open and close prices. Sorted from new to old.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `interval` | `number` | No | The ticks interval. |
| `fullCandles` | `boolean` | No | If true, the last closed candles will be returned. |
| `market` | `string` | No | The market of the tick. Default is the selected main market. |
| `hlcStyle` | `boolean` | No | When enabled, the data returned will be adjusted for HLC instead of OHLC. Meaning that the OHL data can change. |

**Returns:** `HaasNumberCollection`

Returns a collection of an average of open and close prices.

---

## OHLCPrices

```lua
OHLCPrices([interval], [fullCandles], [market], [hlcStyle])
```

**Aliases:** `HeikenClosePrices`

A collection with the average of open, high, low and close prices. Sorted from new to old.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `interval` | `number` | No | The ticks interval. |
| `fullCandles` | `boolean` | No | If true, the last closed candles will be returned. |
| `market` | `string` | No | The market of the tick. Default is the selected main market. |
| `hlcStyle` | `boolean` | No | When enabled, the data returned will be adjusted for HLC instead of OHLC. Meaning that the OHL data can change. |

**Returns:** `HaasNumberCollection`

Returns a collection of the average of open, high, low and close prices.

---

## OpenPrices

```lua
OpenPrices([interval], [fullCandles], [market], [hlcStyle])
```

A collection with open prices. Sorted from new to old.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `interval` | `number` | No | The ticks interval. |
| `fullCandles` | `boolean` | No | If true, the last closed candles will be returned. In backtests the default series already contains only closed candles (`[1]` is the last closed candle, on the main interval and on higher intervals), so this flag returns the same series. |
| `market` | `string` | No | The market of the tick. Default is the selected main market. |
| `hlcStyle` | `boolean` | No | When enabled, each candle's open is replaced by the previous candle's close. The close, high and low are unchanged, so the open can lie above the high or below the low. |

**Returns:** `HaasNumberCollection`

Returns a collection of open prices.

---

## SellPrices

```lua
SellPrices([interval], [fullCandles], [market], [hlcStyle])
```

**Aliases:** `BidPrices`

A collection with sell/ask prices. Sorted from new to old.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `interval` | `number` | No | The ticks interval. |
| `fullCandles` | `boolean` | No | If true, the last closed candles will be returned. |
| `market` | `string` | No | The market of the tick. Default is the selected main market. |
| `hlcStyle` | `boolean` | No | When enabled, the data returned will be adjusted for HLC instead of OHLC. Meaning that the OHL data can change. |

**Returns:** `HaasNumberCollection`

Returns a collection of sell/ask prices.

---

## SourcePrices

```lua
SourcePrices(sourceType, [interval], [fullCandles], [market], [hlcStyle])
```

Gets the price source based on the input enum given. Can be any of Enumerations Source Price Types.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `sourceType` | `enum` | Yes | Source price type enum. |
| `interval` | `number` | No | The ticks interval. |
| `fullCandles` | `boolean` | No | If true, the last closed candles will be returned. |
| `market` | `string` | No | The market of the tick. Default is the selected main market. |
| `hlcStyle` | `boolean` | No | When enabled, the data returned will be adjusted for HLC instead of OHLC. Meaning that the OHL data can change. |

**Returns:** `HaasNumberCollection`

Returns a collection of prices.

---


# Price Market Information

## AverageCandleSize

```lua
AverageCandleSize([market])
```

Calculates the average candle body size of the last 50 minutes.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `number`

Returns the average candle body size.

---

## AverageOrderbookSpread

```lua
AverageOrderbookSpread([market])
```

Calculates the average orderbook bid/ask spread of the last 50 minutes.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `number`

Returns the average orderbook bid/ask spread.

---

## CreateMarket

```lua
CreateMarket([priceSource], [baseCurrency], [quoteCurrency], [contractName])
```

Creates a market string that can be used for a variety of commands.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `priceSource` | `string` | No | The price source of the market. If empty the main bot price source is set. |
| `baseCurrency` | `string` | No | The base currency of the market. If empty the bots base currency price source is set. |
| `quoteCurrency` | `string` | No | The quote currency of the market. If empty the bots main quote currency is set. |
| `contractName` | `string` | No | The contract name of the market. If empty the bots main contract name is set. Used exactly as given (case is not normalized), so pass the exchange's contract name, for example `PERPETUAL`. |

**Returns:** `string`

Returns the market string.

---

## GetLastTrades

```lua
GetLastTrades([depth], [market])
```

Gets a list of the last trades.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `depth` | `number` | No | Maximum number of records returned (can return less). |
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `dynamic`

Returns a list of the last trades as trade objects with the fields `price`, `amount` and `isBuy` (not nested arrays).

In backtests, returns nil (no error or warning), so `#result` or `result[1]` raises a Lua error. Check for nil before use.

---

## GetOrderbook

```lua
GetOrderbook([market])
```

Gets the orderbook on a specific market.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `dynamic`

Returns a collection with 4 arrays.

**Container fields** (accessible by name or 1-based index):

| Index | Name | Type | Description |
|-------|------|------|-------------|
| 1 | `askPrices` | `HaasNumberCollection` | Ask/buy prices. Sorted from low to high. |
| 2 | `askAmounts` | `HaasNumberCollection` | Ask/buy amounts. Sorted on price from low to high. |
| 3 | `bidPrices` | `HaasNumberCollection` | Bid/sell prices. Best bid first (high to low) in backtests. |
| 4 | `bidAmounts` | `HaasNumberCollection` | Bid/sell amounts, in the same order as `bidPrices`. |

Backtests have no historical order book: there `GetOrderbook()` returns a synthetic book of 100 levels per side around the current price, each level 0.05% further from the previous one, with amounts 1, 2, 3, ... Order-book logic cannot be validated in a backtest.

---

## GetOrderbookAsk

```lua
GetOrderbookAsk([market])
```

Gets a list of the orderbook ask records.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `dynamic`

Returns a list of ask levels, best (lowest) ask first. Each element is a `[price, amount]` pair. Output format: [[price, amount], [price, amount] ...]. In backtests the list holds 10 levels.

---

## GetOrderbookBid

```lua
GetOrderbookBid([market])
```

Gets a list of the orderbook bid records.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `dynamic`

Returns a list of bid levels, best (highest) bid first. Each element is a `[price, amount]` pair. Output format: [[price, amount], [price, amount] ...]. In backtests the list holds 10 levels.

---

## LastBuyTradesCommand

```lua
LastBuyTradesCommand([Seconds back], [market])
```

Returns all last selling orders

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `Seconds back` | `number` | No |  |
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `number`

Returns the buying trading volume

---

## LastSellTradesCommand

```lua
LastSellTradesCommand([Seconds back], [market])
```

Returns all last selling orders

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `Seconds back` | `number` | No |  |
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `number`

Returns the selling trading volume

---

## LastTradesSentiment

```lua
LastTradesSentiment([market])
```

Calculates the last trades sentiment as a number.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `number`

Returns the last trades sentiment. 0.5 is the default, idle value.

---

## OrderbookSentiment

```lua
OrderbookSentiment([market])
```

Calculates the orderbook sentiment as a number.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `number`

Returns the orderbook sentiment. 0.5 is the default, idle value.

---

## PriceLevel

```lua
PriceLevel(price)
```

Determines the price level. For example. 123456 will return 10000. 123 will return 100.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `price` | `number` | Yes | Source data. |

**Returns:** `number`

Returns the price level.

---


# Profit Information

## GetBotProfit

```lua
GetBotProfit([market], [includeUnrealized])
```

Total realized profits on a specific market. Unrealized profits are included when 'includeUnrealized' is set on true.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The guid returned by AccountGuid(), InputAccount or InputAccountMarket for example. |
| `includeUnrealized` | `boolean` | No | Set on true to include the unrealized profits. By default false. |

**Returns:** `number`

Return total realized profit. On spot/margin the result will be in the quote currency. For leverage its in the underlying currency.

---

## GetBotROI

```lua
GetBotROI([market])
```

Gets the return on investment. The ROI is calculated by dividing the total realized profits by the average position size.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The guid returned by AccountGuid(), InputAccount or InputAccountMarket for example. |

**Returns:** `number`

Return the ROI percentage.

---

## GetCurrentProfit

```lua
GetCurrentProfit([direction], [market])
```

Gets unrealized plus realized profits of the current open position. Optional parameters fall back to main bot settings. The accountId parameter can take a positionId as well.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `direction` | `enum` | No | Bot position enum. PositionLong, PositionShort or NoPosition. |
| `market` | `string` | No | The guid returned by AccountGuid(), InputAccount or InputAccountMarket for example. |

**Returns:** `number`

Returns the total profit. On spot/margin the result will be in the quote currency. For leverage its in the underlying currency.

---

## GetCurrentROI

```lua
GetCurrentROI([direction], [market])
```

Gets the return on investment percentage based on the total cost/margin used of the open positions. Optional parameters fall back to main bot settings. The accountId parameter can take a positionId as well.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `direction` | `enum` | No | Bot position enum. PositionLong, PositionShort or NoPosition. |
| `market` | `string` | No | The guid returned by AccountGuid(), InputAccount or InputAccountMarket for example. |

**Returns:** `number`

Returns the ROI percentage of the open position, fees excluded. On spot it equals `GetPositionROI()`. Returns 0 with no open position.

---

## GetTradingReport

```lua
GetTradingReport([market])
```

Gets the trading report container with many useful values to help properly evaluate a strategy or a trading system.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The guid returned by AccountGuid(), InputAccount or InputAccountMarket for example. |

**Returns:** `dynamic`

The trading report container.

**Container fields** (accessible by name or 1-based index):

| Index | Name | Type | Description |
|-------|------|------|-------------|
| 1 | `maxDrawDownPrc` | `number` | Max. DrawDown in percentages. |
| 2 | `maxDrawDown` | `number` | Max. DrawDown in underlying asset. |
| 3 | `sharpeRatio` | `number` | Sharpe Ratio. |
| 4 | `sortinoRatio` | `number` | Sortino Ratio. |
| 5 | `winPercentage` | `number` | Win percentage. |
| 6 | `profitRatio` | `number` | Profit Ratio. |
| 7 | `profitFactor` | `number` | Profit Factor. |
| 8 | `cpcIndex` | `number` | CPC Index. |
| 9 | `tailRatio` | `number` | Tail Ratio. |
| 10 | `commonSenseRatio` | `number` | Common Sense Ratio. |
| 11 | `outlierWinRatio` | `number` | Outlier Win Ratio. |
| 12 | `outlierLossRatio` | `number` | Outlier Loss Ratio. |
| 13 | `profitMarginRatio` | `number` | Profit Margin Ratio (Gain / Total Used Margin). |
| 14 | `biggestWin` | `number` | Biggest Winner. |
| 15 | `biggestLoss` | `number` | Biggest Loser: a position's realized loss including its entry and exit fees. |
| 16 | `highestPointInProfit` | `number` | Highest Point in total PNL. |
| 17 | `lowestPointInProfit` | `number` | Lowest Point in total PNL. |
| 18 | `totalMarginUsed` | `number` | Total Margin Used. |
| 19 | `averageMargin` | `number` | Average Margin used per position: the mean of each position's margin (notional ÷ the leverage in force when it opened). |
| 20 | `averageProfit` | `number` | Average Profit per position. |
| 21 | `closedPositions` | `number` | Closed Positions count. |
| 22 | `profitablePositions` | `number` | Profitable Positions count. |
| 23 | `losingPositions` | `number` | Losing Positions count. |
| 24 | `executedOrders` | `number` | Executed Orders count. |
| 25 | `completedOrders` | `number` | Completed Orders count. |

In the backtest report, "Return on investment" is the realized profit ÷ `averageMargin`, and "Fee costs" is the fee rate × the notional of every fill. "Average open time" is the mean time from order placement to fill, not how long positions were held.

---

## SetBotRoiBaseValue

```lua
SetBotRoiBaseValue(baseValue, [market])
```

Sets a custom base value used in the ROI calculation (realized profits / baseValue * 100).

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `baseValue` | `number` | Yes | The base value for the ROI calculation |
| `market` | `string` | No | The guid returned by AccountGuid(), InputAccount or InputAccountMarket for example. |

**Returns:** `void`

---


# Time Information

## AdjustTimestamp

```lua
AdjustTimestamp([unix], [addSeconds], [addMinutes], [addHours], [addMonths], [addDays], [addYears])
```

Adjusts an Unix timestamp.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `unix` | `number` | No | The unix timestamp, by default the current time.. |
| `addSeconds` | `number` | No | Number of seconds to add. |
| `addMinutes` | `number` | No | Number of minutes to add. |
| `addHours` | `number` | No | Number of hours to add. |
| `addMonths` | `number` | No | Number of months to add. |
| `addDays` | `number` | No | Number of days to add. |
| `addYears` | `number` | No | Number of years to add. |

**Returns:** `number`

Returns the new Unix timestamp.

---

## CloseTime

```lua
CloseTime(unix, interval)
```

Converts a timestamps to the close timestamp of the candle.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `unix` | `number` | Yes | The timestamp to round. Set on 0 for current. |
| `interval` | `number` | Yes | The interval on which the candle is build. |

**Returns:** `number`

Returns the close timestamp of the candle.

---

## CreateTimestamp

```lua
CreateTimestamp([year], [month], [day], [hour], [minute], [second])
```

Creates an Unix timestamp (UTC) from the specified value. Omitted arguments take the current time, so `CreateTimestamp(2024, 1, 1)` is 1 January 2024 at the current time of day, not at midnight. Pass all six arguments for a fixed time: `CreateTimestamp(2024, 1, 1, 0, 0, 0)` = 1704067200.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `year` | `number` | No | The year of the timestamp, by default the current time. |
| `month` | `number` | No | The month of the timestamp, by default the current time. |
| `day` | `number` | No | The date of the timestamp, by default the current time. |
| `hour` | `number` | No | The hour of the timestamp, by default the current time. |
| `minute` | `number` | No | The minute of the timestamp, by default the current time. |
| `second` | `number` | No | The seconds of the timestamp, by default the current time. |

**Returns:** `number`

Returns the Unix timestamp.

---

## CurrentDate

```lua
CurrentDate([unix])
```

Gets the date of the month of the current time or requested time. Based on UTC.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `unix` | `number` | No | An unix timestamp, by default the current time. |

**Returns:** `number`

Returns the date of the month value. A number from 1 to 31.

---

## CurrentDay

```lua
CurrentDay([unix])
```

Gets the day of the week of the current time or requested time. Based on UTC.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `unix` | `number` | No | An unix timestamp, by default the current time. |

**Returns:** `number`

Returns the day of the week value. A number from 1 to 7, starting on Sunday.

---

## CurrentHour

```lua
CurrentHour([unix])
```

Gets the hour the current time or requested time. Based on UTC.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `unix` | `number` | No | An unix timestamp, by default the current time. 0 also means the current time. |

**Returns:** `number`

Returns the hour value. A number from 0 to 23.

---

## CurrentMinute

```lua
CurrentMinute([unix])
```

Gets the minute the current time or requested time. Based on UTC.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `unix` | `number` | No | An unix timestamp, by default the current time. |

**Returns:** `number`

Returns the minute value. A number from 0 to 59.

---

## CurrentMonth

```lua
CurrentMonth([unix])
```

Gets the month of the year of the current time or requested time. Based on UTC.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `unix` | `number` | No | An unix timestamp, by default the current time. |

**Returns:** `number`

Returns the month of the year value. A number from 1 to 12.

---

## CurrentSecond

```lua
CurrentSecond([unix])
```

Gets the second the current time or requested time. Based on UTC.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `unix` | `number` | No | An unix timestamp, by default the current time. |

**Returns:** `number`

Returns the second value. A number from 0 to 59.

---

## CurrentWeek

```lua
CurrentWeek([unix])
```

Gets the week of the year of the current time or requested time. Based on UTC.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `unix` | `number` | No | An unix timestamp, by default the current time. |

**Returns:** `number`

Returns the week of the year value. A number from 1 to 53.

---

## CurrentYear

```lua
CurrentYear([unix])
```

Gets the year the current time or requested time. Based on UTC.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `unix` | `number` | No | An unix timestamp, by default the current time. |

**Returns:** `number`

Returns the year.

---

## FormatDateTime

```lua
FormatDateTime([unix], [dateDelimiter], [dateTimeDelimiter], [timeDelimiter], [includeSeconds], [includeTime], [includeYear])
```

Formats a unix datetime number (to "YYYY-MM-DD at HH:MM" by default). Based on UTC: `FormatDateTime(1700000000)` = "2023-11-14 at 22:13".

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `unix` | `number` | No | The unix datetime number to format. |
| `dateDelimiter` | `string` | No | The characters between the year and month, and between the month and day. |
| `dateTimeDelimiter` | `string` | No | The characters between the date and time. |
| `timeDelimiter` | `string` | No | The characters between the hour and minute, and between the minute and second. |
| `includeSeconds` | `boolean` | No | If true, the seconds will be included in the formatted time. |
| `includeTime` | `boolean` | No | If true, the formatted time will be included. |
| `includeYear` | `boolean` | No | If true, the year will be included in the formatted date. |

**Returns:** `string`

The formatted unix datetime number.

---

## GetTimer

```lua
GetTimer([key])
```

Gets the elapsed time for a timer.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `key` | `string` | No | Optional key if using multiple timers. |

**Returns:** `number`

Returns the elapsed time in milliseconds.

---

## MinutesTillCandleClose

```lua
MinutesTillCandleClose(interval)
```

Calculates the number of minutes before a new candle opens.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `interval` | `number` | Yes | Interval in minutes. |

**Returns:** `number`

Returns the number of minutes before a new candle opens.

---

## OpenTime

```lua
OpenTime(unix, interval)
```

Converts a timestamps to the open timestamp of the candle

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `unix` | `number` | Yes | The timestamp to round. Set on 0 for current |
| `interval` | `number` | Yes | The interval on which the candle is build |

**Returns:** `number`

Returns the open timestamp of the candle.

---

## Sleep

```lua
Sleep(milliseconds)
```

Pauses script execution for a specified number of milliseconds. Useful for adding random delays to avoid predictable order timing patterns.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `milliseconds` | `number` | Yes | Number of milliseconds to sleep (e.g., 100 = 100ms, 5000 = 5 seconds). Maximum 10000ms (10 seconds) recommended. |

**Returns:** `number`

Returns the actual time slept in milliseconds.

**Note:** Sleep time does not count toward script execution timeout, allowing multiple Sleep() calls without triggering warnings.

`Sleep` also pauses in backtests, so it lengthens the backtest run.

**Example — Anti-Detection Order Timing:**

```lua
-- Add random delay to avoid predictable patterns
local rsi = RSI(ClosePrices(), 14)

if rsi < 30 then
    local delayMs = Random(0, 9000)  -- 0-9 seconds
    Log("Delaying order by " .. delayMs .. "ms")
    Sleep(delayMs)
    DoLong()
end
```

---

## StartTimer

```lua
StartTimer([key])
```

Starts a timer. A timer lasts for the current update only: on a later update `GetTimer` logs "WARNING: GetTimer(): Cannot get a timer that doesn't exist" and returns 0.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `key` | `string` | No | Optional key if using multiple timers. |

**Returns:** `void`

---

## StopTimer

```lua
StopTimer([key])
```

Stops a timer.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `key` | `string` | No | Optional key if using multiple timers. |

**Returns:** `number`

Returns the elapsed time in milliseconds.

---

## Time

```lua
Time()
```

Gets the current unix timestamp. Based on UTC.

**Returns:** `number`

Returns the current unix timestamp.

---


# Wallet

## Balance

```lua
Balance([accountId], [coin], [market])
```

Total balance specifications of a specific currency. Available, locked and total balance. Works in backtests; does not work yet for simulated accounts.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `accountId` | `string` | No | The guid returned by AccountGuid(), InputAccount() or InputAccountMarket() for example. Can be left empty for default properties. |
| `coin` | `string` | No | Coin of which to retrieve the wallet amount. Default value is the 'UnderlyingAsset' currency (the base coin on a spot market, for example BTC on BTC/USDT). |
| `market` | `string` | No | Optional market parameter for exchanges with separates wallet for each market. |

The first positional argument is always `accountId`. A coin passed as the only argument (`Balance("USDT")`) is ignored without an error, and the default coin's balance is returned. To select a coin, pass the account first — `Balance(AccountGuid(), "USDT")` — or use a named-argument table: `Balance({coin = "USDT"})`.

**Returns:** `dynamic`

Returns a result object with the available, locked and total balance.

**Container fields** (accessible by name or 1-based index):

| Index | Name | Type | Description |
|-------|------|------|-------------|
| 1 | `available` | `number` | The available balance. |
| 2 | `locked` | `number` | The locked balance for orders and/or position margin among others. |
| 3 | `total` | `number` | The total balance. Available and locked. |

Field names are lowercase and case-sensitive: `.available` returns the value, `.Available` returns nil.

---

## BalanceAmount

```lua
BalanceAmount([accountId], [coin], [market])
```

Total balance value of a specific currency. Including order and position margin. Works in backtests; does not work yet for simulated accounts.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `accountId` | `string` | No | The guid returned by AccountGuid(), InputAccount() or InputAccountMarket() for example. Can be left empty for default properties. |
| `coin` | `string` | No | Coin of which to retrieve the wallet amount. Default value is the 'UnderlyingAsset' currency (the base coin on a spot market, for example BTC on BTC/USDT). |
| `market` | `string` | No | Optional market parameter for exchanges with separates wallet for each market. |

The first positional argument is always `accountId`. A coin passed as the only argument (`BalanceAmount("USDT")`) is ignored without an error, and the default coin's balance is returned. Pass the account first — `BalanceAmount(AccountGuid(), "USDT")` — or use `BalanceAmount({coin = "USDT"})`.

**Returns:** `number`

Returns the total balance value.

---

## MarginToTradeAmount

```lua
MarginToTradeAmount(price, margin, leverage, [market])
```

Convert margin to a trade amount. In spot markets, the margin is anticipated to be in the quote currency (e.g., USDT in BTC/USDT). For leverage markets, the margin is expected to be in the margin currency of the contract (USDT on a USDT perpetual).

On spot markets the leverage is ignored: the result is margin / price (`MarginToTradeAmount(45000, 1000, 10)` = 0.02222).

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `price` | `number` | Yes | The price used in the conversion from margin to trade amount. |
| `margin` | `number` | Yes | The margin used to calculate the trade amount |
| `leverage` | `number` | Yes | Leverage ratio. Use 0 for cross-margin. |
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `number`

Returns the calculated trade amount based on the margin.

On a Bybit USDT perpetual the result is margin × leverage / (price × (1 + (2 × leverage − 1) × takerFee)), rounded down to the amount step, where takerFee is `TakersFee()` / 100: the margin also covers the taker fees to open and to close the position. The difference from margin × leverage / price grows with the leverage (about 1.4 % at 10x, about 10 % at 100x).

---

## MaxExitLongAmount

```lua
MaxExitLongAmount([market])
```

Calculates the maximum amount which can be used to place an exit long order based on wallet data. Assets reserved in open orders are excluded by default.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The guid returned by AccountGuid(), InputAccount or InputAccountMarket for example. |

**Returns:** `number`

Returns the maximum exit long amount.

---

## MaxExitShortAmount

```lua
MaxExitShortAmount([market])
```

Calculates the maximum amount which can be used to place an exit short order based on wallet data. Assets reserved in open orders are excluded by default.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The guid returned by AccountGuid(), InputAccount or InputAccountMarket for example. |

**Returns:** `number`

Returns the maximum exit short amount.

---

## MaxLongAmount

```lua
MaxLongAmount([market])
```

Calculates the maximum amount which can be used to place a long order based on wallet data.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The guid returned by AccountGuid(), InputAccount or InputAccountMarket for example. |

**Returns:** `number`

Returns the maximum long amount.

---

## MaxShortAmount

```lua
MaxShortAmount([market])
```

Calculates the maximum amount which can be used to place a short order based on wallet data.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The guid returned by AccountGuid(), InputAccount or InputAccountMarket for example. |

**Returns:** `number`

Returns the maximum long amount.

---

## UserPositionContainer

```lua
UserPositionContainer([accountId], [market], [leverage], [direction])
```

Gets all the information from the open user position: the account's position on the market. In backtests it does not reflect the script's own positions and reports no position. Read the script's own positions with PositionContainer() and the position commands, which behave the same in backtests and running bots.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `accountId` | `string` | No | The guid returned by AccountGuid(), InputAccount or InputAccountMarket for example. |
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |
| `leverage` | `number` | No | Deprecated, Can be ignored. |
| `direction` | `enum` | No | Direction of the position PositionLong or PositionShort. Empty for any. If there is both a long and short position, the long position will be returned. |

The first positional argument is always `accountId`. A market passed as the only argument (`UserPositionContainer("BINANCE_ETH_USDT_")`) is ignored without an error, and the main market's position is returned. Pass the account first: `UserPositionContainer(AccountGuid(), "BINANCE_ETH_USDT_")`.

**Returns:** `dynamic`

Returns an array with position information.

A full container is returned even when there is no position (numbers 0, flags false, direction NoPosition); it is never nil.

**Container fields** (accessible by name or 1-based index):

| Index | Name | Type | Description |
|-------|------|------|-------------|
| 1 | `market` | `string` | The position key: account id, market and a suffix joined by `_` (for example `<accountId>_BINANCE_BTC_USDT__-1`), not a plain market name. |
| 2 | `isLong` | `boolean` | Set on true when the direction is long. |
| 3 | `isShort` | `boolean` | Set on true when the direction is short. |
| 4 | `enterPrice` | `number` | The average enter price. |
| 5 | `amount` | `number` | The amount open in the position. |
| 6 | `profit` | `number` | The realized + unrealized profits. |
| 7 | `roi` | `number` | The ROI based on the used margin and realized + unrealized profits. |
| 8 | `direction` | `enum` | The position direction: PositionLong, PositionShort or NoPosition. |

---

## WalletAmount

```lua
WalletAmount([accountId], [coin], [market])
```

Get the available amount in the wallet.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `accountId` | `string` | No | The guid returned by AccountGuid(), InputAccount() or InputAccountMarket() for example. Can be left empty for default properties. |
| `coin` | `string` | No | Coin of which to retrieve the wallet amount. Default value is the 'UnderlyingAsset' currency (the base coin on a spot market, for example BTC on BTC/USDT). |
| `market` | `string` | No | Optional market parameter for exchanges with separates wallet for each market. |

The first positional argument is always `accountId`. A coin passed as the only argument (`WalletAmount("USDT")`) is ignored without an error, and the default coin's amount is returned. Pass the account first — `WalletAmount(AccountGuid(), "USDT")` — or use `WalletAmount({coin = "USDT"})`.

**Returns:** `number`

Returns the available amount. On leverage markets with cross margin, in backtests, this is the balance after fees and realized profit minus the margin in use; unrealized profit is not included.

---

## WalletCheck

```lua
WalletCheck(accountId, coin, amount)
```

Checks if the wallet has enough of the funds.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `accountId` | `string` | Yes | The guid returned by AccountGuid(), InputAccount() or InputAccountMarket() for example. Can be left empty for default properties. |
| `coin` | `string` | Yes | Coin of which to retrieve the wallet amount. |
| `amount` | `number` | Yes | The minimum amount needed. |

**Returns:** `boolean`

Returns true if the wallet has enough funds.

---


*Auto-generated from HaasScript source code.*
