# Trading & Positions

136 functions across 11 categories.

## Categories

- [Easy Insurances](#easy-insurances) (13 commands)
- [Easy Safeties](#easy-safeties) (18 commands)
- [Order Handling](#order-handling) (8 commands)
- [Order Information](#order-information) (8 commands)
- [Position Information](#position-information) (14 commands)
- [Position Prices](#position-prices) (9 commands)
- [Signal Helpers](#signal-helpers) (24 commands)
- [Trade Actions (Managed)](#trade-actions-managed) (5 commands)
- [Trade Actions (Unmanaged)](#trade-actions-unmanaged) (8 commands)
- [Trade Bot](#trade-bot) (4 commands)
- [Trade Market Information](#trade-market-information) (25 commands)

---

# Easy Insurances

## AbsolutePriceChange

```lua
AbsolutePriceChange([amount], [targetPrice], [positionId])
```

Compares the average entry price to current price, absolute value-based.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `amount` | `number` | No | Minimum absolute price change before the insurance agrees. |
| `targetPrice` | `number` | No | The target price of the trade. Default is the current buy or sell price. |
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |

**Returns:** `boolean`

Returns true if the minimum price change percentage is reached.

---

## NeverEnterWithALoss

```lua
NeverEnterWithALoss([acceptedLoss], [targetPrice])
```

Blocks enter trade when targetPrice is above the last sell order. Only works on spot and when there is no position open.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `acceptedLoss` | `number` | No | The accepted loss in percentage. Default is 0. This also affects user-defined targetPrice. |
| `targetPrice` | `number` | No | The target price of the trade. Can be used when placing orders beforehand in unmanaged trading. |

**Returns:** `boolean`

Returns true if the trade is allowed.

---

## NeverExitWithLoss

```lua
NeverExitWithLoss([acceptedLoss], [targetPrice], [positionId])
```

Compares the average enter price to the targetPrice or current price.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `acceptedLoss` | `number` | No | The accepted loss in percentage. Default is 0. This also affects user-defined targetPrice. |
| `targetPrice` | `number` | No | The target price of the trade. Can be used when placing orders beforehand in unmanaged trading. |
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |

**Returns:** `boolean`

Returns true if the trade is allowed. Returns true while no position is open and for an unknown positionId. A negative acceptedLoss requires that much profit before the exit is allowed. A given targetPrice replaces the current price in the comparison. The call does not block orders by itself: use the result, for example as an insurance in InsuranceContainer() or TradeBotContainer().

---

## OrderOncePerBar

```lua
OrderOncePerBar([interval], [positionId])
```

Only allows a order to occur once per bar. Signals produced during a ordered bar are blocked.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `interval` | `number` | No | Interval of the bar in minutes. |
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |

**Returns:** `boolean`

Returns true when new bar opens or no orders occurred during a bar. An order counts from the update it is placed, so the command returns false on that update.

---

## OvercomeDoubleFeeCosts

```lua
OvercomeDoubleFeeCosts([targetPrice], [positionId])
```

Calculates the target profit and makes sure the fee costs are double-covered.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `targetPrice` | `number` | No | The target price of the trade. Default is the current buy or sell price. |
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |

**Returns:** `boolean`

Returns true if the trade profit is enough to cover double the fees.

---

## OvercomeFeeCosts

```lua
OvercomeFeeCosts([targetPrice], [positionId])
```

Calculates the target profit and makes sure the fee costs are covered.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `targetPrice` | `number` | No | The target price of the trade. Default is the current buy or sell price. |
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |

**Returns:** `boolean`

Returns true if the trade profit is enough to cover the fees.

---

## PercentagePriceChange

```lua
PercentagePriceChange([percentage], [targetPrice], [positionId])
```

Compares the average enter price to current price, percentage-based.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `percentage` | `number` | No | Minimum percentage change before the insurance agrees. |
| `targetPrice` | `number` | No | The target price of the trade. Default is the current buy or sell price. |
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |

**Returns:** `boolean`

Returns true if the minimum price change percentage is reached.

---

## StopLossCooldown

```lua
StopLossCooldown(minutes, [positionId])
```

Blocks any signal until a certain number of minutes have passed since the last exit trade executed by a safety.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `minutes` | `number` | Yes | Number of minutes to wait after the last exit made by a safety. |
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |

**Returns:** `boolean`

Returns false for `minutes` minutes after an exit made by a safety (for example a TradeBotContainer safety exit), counted from the exit fill, not from when the safety triggers; true otherwise. For example on 1-minute bars with 10, it is false from the update the exit fills through the tenth minute after it, and true from the eleventh. Exits placed by the script itself, for example DoExitPosition() after StopLoss(), do not count.

---

## TradeOncePerBar

```lua
TradeOncePerBar([interval], [positionId])
```

Only allows a trade to occur once per bar. Signals produced during a traded bar are blocked.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `interval` | `number` | No | Interval of the bar in minutes. |
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |

**Returns:** `boolean`

Returns true when new bar opens or no trade occurred during a bar.

---

## TradeOnlySideways

```lua
TradeOnlySideways([threshold], [interval], [fullCandles], [market])
```

Checks the current market condition for sideways movement.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `threshold` | `number` | No | Sideways movement threshold. |
| `interval` | `number` | No | The ticks interval. |
| `fullCandles` | `boolean` | No | If true, the last closed candles will be returned. |
| `market` | `string` | No | The market of the tick. Default is the selected main market. |

**Returns:** `boolean`

Returns true if the trade is allowed.

---

## TradeOnlyTrending

```lua
TradeOnlyTrending([threshold], [interval], [fullCandles], [market])
```

Checks the current market condition for a trend.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `threshold` | `number` | No | Trend strength threshold. |
| `interval` | `number` | No | The ticks interval. |
| `fullCandles` | `boolean` | No | If true, the last closed candles will be returned. |
| `market` | `string` | No | The market of the tick. Default is the selected main market. |

**Returns:** `boolean`

Returns true if the trade is allowed.

---

## WaitAfterOrder

```lua
WaitAfterOrder(timeout, [positionId])
```

Blocks any signal until a certain number of minutes have passed since the last placed order. This includes orders executed in an update cycle

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `timeout` | `number` | Yes | Number of minutes to wait after the last order. |
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |

**Returns:** `boolean`

Returns true when the last placed order is a certain number of minutes ago. For example on 1-minute bars with timeout 2, it is false on the update the order is placed and the two following minutes, and true from the third.

---

## WaitAfterTrade

```lua
WaitAfterTrade(timeout, [positionId])
```

Blocks any signal until a certain number of minutes have passed since the last trade.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `timeout` | `number` | Yes | Number of minutes to wait after the last trade. |
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |

**Returns:** `boolean`

Returns true when the last trade is a certain number of minutes ago.

---


# Easy Safeties

## DeactivateAfterEnterOrder

```lua
DeactivateAfterEnterOrder([positionCount])
```

Deactivates the bot after an enter order has been completed.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `positionCount` | `number` | No | Number of position allowed before the check is preformed. |

**Returns:** `boolean`

Returns true when the bot has been deactivated.

---

## DeactivateAfterExitOrder

```lua
DeactivateAfterExitOrder([positionCount])
```

Deactivates the bot after an exit order has been completed.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `positionCount` | `number` | No | Number of position allowed before the check is preformed. |

**Returns:** `boolean`

Returns true when the bot has been deactivated.

---

## DeactivateAfterXActiveMinutes

```lua
DeactivateAfterXActiveMinutes(minutes)
```

Deactivates the bot when the is active for a certain time.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `minutes` | `number` | Yes | Number of positions allowed before the bot is deactivated. |

**Returns:** `boolean`

Returns true when the bot has been deactivated.

---

## DeactivateAfterXIdleMinutes

```lua
DeactivateAfterXIdleMinutes(minutes)
```

Deactivates the bot if the last completed order is a number of minutes in the past. Timeout starts after the first completed order.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `minutes` | `number` | Yes | Number of positions allowed before the bot is deactivated. |

**Returns:** `boolean`

Returns true when the bot has been deactivated.

---

## DeactivateAfterXOrders

```lua
DeactivateAfterXOrders([count])
```

Deactivates the bot after a number of orders has been completed.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `count` | `number` | No | Number of orders allowed before the bot is deactivated. |

**Returns:** `boolean`

Returns true when the bot has been deactivated.

---

## DeactivateAfterXPositions

```lua
DeactivateAfterXPositions([count])
```

Deactivates the bot after a number of positions has been completed.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `count` | `number` | No | Number of positions allowed before the bot is deactivated. |

**Returns:** `boolean`

Returns true when the bot has been deactivated.

---

## DeactivateOnLoss

```lua
DeactivateOnLoss(maxLoss)
```

Deactivates the bot when there is no open position and the ROI based losses have reached a certain level.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `maxLoss` | `number` | Yes | Maximum allowed loss based on the ROI. |

**Returns:** `boolean`

Returns true when the bot has been deactivated.

---

## DeactivateOnProfit

```lua
DeactivateOnProfit(minProfit)
```

Deactivates the bot when there is no open position and the ROI based profits have reached a certain level.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `minProfit` | `number` | Yes | Minimum profit based on the ROI. |

**Returns:** `boolean`

Returns true when the bot has been deactivated.

---

## DynamicStopLoss

```lua
DynamicStopLoss(percentage, [depth], [positionId], [direction])
```

Calculates the dynamic stop loss price and compares it with the current exit price.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `percentage` | `number` | Yes | Price pump percentage. |
| `depth` | `number` | No | Read depth. Default is 30 ticks. |
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |
| `direction` | `enum` | No | The direction of the position. PositionLong or PositionShort. By default both. |

**Returns:** `boolean`

Returns true is the stop loss price has been breached.

---

## DynamicTakeProfit

```lua
DynamicTakeProfit(percentage, [depth], [positionId], [direction])
```

Calculates the dynamic take profit price and compares it with the current exit price.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `percentage` | `number` | Yes | Price pump percentage. |
| `depth` | `number` | No | Read depth. Default is 30 ticks. |
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |
| `direction` | `enum` | No | The direction of the position. PositionLong or PositionShort. By default both. |

**Returns:** `boolean`

Returns true is the take profit price has been breached.

---

## GrowingTrailingStopLoss

```lua
GrowingTrailingStopLoss(percentage, growPercentage, [positionId], [direction])
```

Keeps track of the highest/lowest recorded price and adjust the stop loss accordingly. The higher the price change the more the trailing distances grows.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `percentage` | `number` | Yes | Trailing stop loss percentage. |
| `growPercentage` | `number` | Yes | The grow percentage. 0.035 for example. |
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |
| `direction` | `enum` | No | The direction of the position. PositionLong or PositionShort. By default both. |

**Returns:** `boolean`

Returns true if the stop loss has been hit.

---

## ShrinkingTrailingStopLoss

```lua
ShrinkingTrailingStopLoss(percentage, shrinkPercentage, [positionId], [direction])
```

Keeps track of the highest/lowest recorded price and adjust the stop loss accordingly. The higher the price change the more the trailing distances shrinks.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `percentage` | `number` | Yes | Trailing stop loss percentage. |
| `shrinkPercentage` | `number` | Yes | This percentage will be added to the stop every time the stop adjust. The percentage will raise exponentially on every adjustment. |
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |
| `direction` | `enum` | No | The direction of the position. PositionLong or PositionShort. By default both. |

**Returns:** `boolean`

Returns true if the stop loss has been hit.

---

## StopLoss

```lua
StopLoss(percentage, [positionId], [direction])
```

Calculates the current percentage change from the average enter price and compares it with the stop loss value.

The percentage is the price change and is not multiplied by the leverage. For a return-on-margin threshold, divide by the leverage or use StopLossROI() / TakeProfitROI().

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `percentage` | `number` | Yes | Stop loss percentage. |
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |
| `direction` | `enum` | No | The direction of the position. PositionLong or PositionShort. By default both. Only a position on this side is evaluated; returns false when the open position is on the other side. |

**Returns:** `boolean`

Returns true if the stop loss has been hit.

Returns a boolean only; no order is placed, also in managed scripts. Act on it, for example `if StopLoss(x) then DoExitPosition() end`, or pass it to SafetyContainer(). It is re-evaluated on every call and does not latch. With no position it returns false. With a positionId it evaluates that position in its own direction (for a short, the stop loss is hit when the price rises).

---

## StopLossROI

```lua
StopLossROI(percentage, [positionId], [direction])
```

Compares the position's `GetPositionROI` (profit on margin, about the price move × leverage; fees excluded while the position is open) with the percentage and gives a signal if the set loss is reached. While a position is open, `StopLossROI(x)` equals `StopLoss(x ÷ leverage)`.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `percentage` | `number` | Yes | Stop loss ROI percentage. |
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |
| `direction` | `enum` | No | The direction of the position. PositionLong or PositionShort. By default both. Only a position on this side is evaluated; returns false when the open position is on the other side. |

**Returns:** `boolean`

Returns true if the stop loss has been hit.

> **Warning:** A closed position keeps its final ROI, so `StopLossROI` can keep returning true after the position is closed. Check `GetPositionAmount(positionId) > 0` before acting on it.

---

## TakeProfit

```lua
TakeProfit(percentage, [positionId], [direction])
```

Calculates the current percentage change from the average enter price and compares it with the take profit value.

The percentage is the price change and is not multiplied by the leverage. For a return-on-margin threshold, divide by the leverage or use StopLossROI() / TakeProfitROI().

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `percentage` | `number` | Yes | Take profit percentage. |
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |
| `direction` | `enum` | No | The direction of the position. PositionLong or PositionShort. By default both. Only a position on this side is evaluated; returns false when the open position is on the other side. |

**Returns:** `boolean`

Returns true if the take profit has been reached.

Returns a boolean only; no order is placed, also in managed scripts. Act on it, for example `if TakeProfit(x) then DoExitPosition() end`, or pass it to SafetyContainer(). It is re-evaluated on every call and does not latch. With no position it returns false. With a positionId it evaluates that position in its own direction (for a short, the take profit is reached when the price falls).

---

## TakeProfitROI

```lua
TakeProfitROI(percentage, [positionId], [direction])
```

Compares the position's `GetPositionROI` (profit on margin, about the price move × leverage; fees excluded while the position is open) with the percentage and gives a signal if the set profit is reached. While a position is open, `TakeProfitROI(x)` equals `TakeProfit(x ÷ leverage)`.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `percentage` | `number` | Yes | Take profit ROI percentage. |
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |
| `direction` | `enum` | No | The direction of the position. PositionLong or PositionShort. By default both. Only a position on this side is evaluated; returns false when the open position is on the other side. |

**Returns:** `boolean`

Returns true if the take profit has been reached.

---

## TrailingArmStopLoss

```lua
TrailingArmStopLoss(percentage, arm, [positionId], [direction])
```

Keeps track of the highest/lowest recorded price and adjust the stop loss accordingly when the arm% has been reached.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `percentage` | `number` | Yes | Trailing stop loss percentage. |
| `arm` | `number` | Yes | The Arm percentage. |
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |
| `direction` | `enum` | No | The direction of the position. PositionLong or PositionShort. By default both. |

**Returns:** `boolean`

Returns true if the stop loss has been hit.

---

## TrailingStopLoss

```lua
TrailingStopLoss(percentage, [positionId], [direction])
```

Keeps track of the highest/lowest recorded price and adjust the stop loss accordingly.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `percentage` | `number` | Yes | Trailing stop loss percentage. |
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |
| `direction` | `enum` | No | The direction of the position. PositionLong or PositionShort. By default both. |

**Returns:** `boolean`

Returns true if the stop loss has been hit.

Returns a boolean only; no order is placed, also in managed scripts. Act on it, for example `if TrailingStopLoss(x) then DoExitPosition() end`, or pass it to SafetyContainer(). It is re-evaluated on every call and does not latch. With no position it returns false. With a positionId it evaluates that position in its own direction (for a short, the stop loss is hit when the price rises).

---


# Order Handling

## CancelAllOrders

```lua
CancelAllOrders([positionId])
```

Cancels all open orders.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |

**Returns:** `void`

---

## CancelOrder

```lua
CancelOrder(orderId)
```

Sends out a cancel order for the given order identifier. The cancellation takes effect on the next update: the order is still open for the rest of the current tick, the log shows "Cancelling order <id>" on this tick and "Order has been cancelled by script." on the next. An order cancelled without any fill does not appear in GetAllFilledOrders().

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `orderId` | `string` | Yes | Unique order identifier provided by the PlaceOrder commands. |

**Returns:** `void`

---

## IsAnyOrderFinished

```lua
IsAnyOrderFinished([positionId])
```

Checks if there are any open orders completed/cancelled.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |

**Returns:** `boolean`

Returns true if there are any open orders completed/cancelled.

---

## IsAnyOrderOpen

```lua
IsAnyOrderOpen([positionId])
```

Checks if there are any open orders.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |

**Returns:** `boolean`

Returns true if there are any open orders.

---

## IsBuyOrder

```lua
IsBuyOrder(orderId)
```

Checks if the given order is a buy order. Only reliable while the order is open: it returns false once the order is filled or cancelled. Use `OrderContainer(orderId).isBuyOrder` for a filled order.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `orderId` | `string` | Yes | Unique order identifier provided by the PlaceOrder commands. |

**Returns:** `boolean`

Returns true if the given order is on buy side.

---

## IsOrderFilled

```lua
IsOrderFilled(orderId)
```

Checks if the given order is filled.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `orderId` | `string` | Yes | Unique order identifier provided by the PlaceOrder commands. |

**Returns:** `boolean`

Returns true if the given order is filled.

---

## IsOrderOpen

```lua
IsOrderOpen(orderId)
```

Checks if the given order is still open.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `orderId` | `string` | Yes | Unique order identifier provided by the PlaceOrder commands. |

**Returns:** `boolean`

Returns true if the given order is still open.

---

## IsSellOrder

```lua
IsSellOrder(orderId)
```

Checks if the given order is a sell order. Only reliable while the order is open: it returns false once the order is filled. Use `OrderContainer(orderId).isSellOrder` for a filled order.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `orderId` | `string` | Yes | Unique order identifier provided by the PlaceOrder commands. |

**Returns:** `boolean`

Returns true if the given order is on sell side.

---


# Order Information

## GetAllFilledOrders

```lua
GetAllFilledOrders([positionId])
```

A collection of (partially) filled orders executed by the bot. Maximum size is 50 orders.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `positionId` | `string` | No | Optional unique identifier. When set only orders related to that position are returned. |

**Returns:** `dynamic`

Returns a Lua table (1-based list) of OrderContainer() objects, sorted descending on close timestamp. An empty table when there are none.

---

## GetAllOpenOrders

```lua
GetAllOpenOrders([positionId])
```

A collection of open orders executed by the bot.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `positionId` | `string` | No | Optional unique identifier. When set only orders related to that position are returned. |

**Returns:** `dynamic`

Returns a Lua table (1-based list) of OrderContainer() objects. An empty table when there are none.

Orders placed in the current update may not be listed yet: on a live bot they appear once the exchange confirms them, in a backtest on the next update. Keep the returned order identifiers to count them.

---

## GetFailedOrderMessage

```lua
GetFailedOrderMessage(orderId)
```

The reason for the order failure.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `orderId` | `string` | Yes | Unique order identifier provided by the PlaceOrder commands. |

**Returns:** `string`

Returns a string with the reason for the order failure.

---

## GetOrderCancelledAmount

```lua
GetOrderCancelledAmount(orderId)
```

Calculates the cancelled amount if the order is completed/cancelled.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `orderId` | `string` | Yes | Unique order identifier provided by the PlaceOrder commands. |

**Returns:** `number`

Returns the cancelled amount if the order is completed/cancelled. On an order that is still open it logs "WARNING: GetOrderCancelledAmount(): Order is still active." and returns 0.

---

## GetOrderFilledAmount

```lua
GetOrderFilledAmount(orderId, [afterFees])
```

Calculates the amount filled for a specific order.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `orderId` | `string` | Yes | Unique order identifier provided by the PlaceOrder commands. |
| `afterFees` | `boolean` | No | If true, the fee cost are subtracted from the total amount if they are paid in the base currency. |

**Returns:** `number`

Returns the amount filled for a specific order.

---

## GetOrderOpenTime

```lua
GetOrderOpenTime(orderId, [inSeconds])
```

Calculates how long an order has been open, in minutes, while it is open. Returns 0 once the order is filled or cancelled.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `orderId` | `string` | Yes | Unique order identifier provided by the PlaceOrder commands. |
| `inSeconds` | `boolean` | No | If set on true, the timespan will be in seconds. |

**Returns:** `number`

Returns the time the order has been open for in minutes (in seconds with inSeconds), or 0 once it is filled or cancelled.

---

## GetOrderProfit

```lua
GetOrderProfit(orderId)
```

Calculates the realized profits of the order.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `orderId` | `string` | Yes | Unique order identifier provided by the PlaceOrder commands. |

**Returns:** `number`

Returns gross profits of the order.

---

## OrderContainer

```lua
OrderContainer(orderId)
```

Gets all the information from an specific order.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `orderId` | `string` | Yes | Unique order identifier provided by the PlaceOrder commands. |

**Returns:** `dynamic`

Returns an array with order information.

**Container fields** (accessible by name or 1-based index):

| Index | Name | Type | Description |
|-------|------|------|-------------|
| 1 | `price` | `number` | The price on which the order is filled or executed. |
| 2 | `executedAmount` | `number` | The order amount (as placed, whether or not it filled). See filledAmount for the filled part. |
| 3 | `filledAmount` | `number` | The filled amount. |
| 4 | `isOpen` | `boolean` | If true, the order is open. |
| 5 | `isFilled` | `boolean` | If true, the order is filled 100%. |
| 6 | `isCancelled` | `boolean` | If true, the order is cancelled but might be filled partially. |
| 7 | `feeCosts` | `number` | The total fee costs. |
| 8 | `feeCurrency` | `string` | The currency of the fee costs. Empty until the order fills. On a spot sell it names the base coin, although the fee is charged in the quote coin. |
| 9 | `openTime` | `number` | While the order is open: how long it has been open, in seconds. After a fill: 0. After a cancel: a Unix timestamp. |
| 10 | `positionId` | `string` | The position identifier related to the order. |
| 11 | `orderId` | `string` | The unique order identifier. |
| 12 | `isEnterOrder` | `boolean` | If true, the order is an enter order. On spot, a sell placed with no open position counts as an enter order. Reads false after the order is cancelled. |
| 13 | `isExitOrder` | `boolean` | If true, the order is an exit order. |
| 14 | `triggerPrice` | `number` | The trigger-price of a conditional order. |
| 15 | `isBuyOrder` | `boolean` | If true, the order is a buy order. Reads false after the order is cancelled. |
| 16 | `isSellOrder` | `boolean` | If true, the order is a sell order. |

---


# Position Information

## AdjustVPosition

```lua
AdjustVPosition(price, amount, [positionId])
```

Adjusts a virtual bot position by processing a simulated order using the defined specifications. Works similarly to PlaceExitLongOrder/PlaceExitShortOrder, but will NOT create a real order; only simulated!

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `price` | `number` | Yes | The close price of the position. |
| `amount` | `number` | Yes | The amount to adjust: negative value reduces, positive value adds. |
| `positionId` | `string` | No | Unique position identifier. |

**Returns:** `void`

---

## CloseVPosition

```lua
CloseVPosition(price, [positionId])
```

Closes a virtual bot position entirely by processing a simulated order using the defined specifications. Works similarly to DoExitPosition and PlaceExitPositionOrder, but will NOT create a real order; only simulated! Use this to "clean up" bot positions if required.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `price` | `number` | Yes | The enter price of the position. |
| `positionId` | `string` | No | Unique position identifier. |

**Returns:** `void`

---

## CreatePosition

```lua
CreatePosition(direction, price, amount, [market], [leverage], [positionId])
```

Creates a bot position by processing a simulated order using the defined specifications. By doing this, profit/loss and position details can be used by the script. Use the returned position ID with PositionContainer() to extract position data.

The simulated orders of `CreatePosition`, `AdjustVPosition` and `CloseVPosition` are logged as "(Simulated Order)" fills and are included in the backtest report statistics (closed and losing positions, executed and completed orders, average margin). They do not count as trading commands for the "No trading command detected" compile check.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `direction` | `enum` | Yes | The direction of the position. |
| `price` | `number` | Yes | The enter price of the position. |
| `amount` | `number` | Yes | The size of the position. |
| `market` | `string` | No | The market of the position. |
| `leverage` | `number` | No | The leverage of the position. -1 for default. |
| `positionId` | `string` | No | Unique position identifier. |

**Returns:** `string`

Returns the position ID (the given `positionId` when one is passed).

---

## GetAllOpenPositions

```lua
GetAllOpenPositions()
```

Gets all open positions and returns them as PositionContainers.

**Returns:** `dynamic`

Returns a Lua table (1-based list) of PositionContainers. An empty table when there are none.

---

## GetPositionAmount

```lua
GetPositionAmount([positionId])
```

Gets the total amount open in a specific position.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once: if omitted while several positions are open, the command logs "Requires positionId when multiple positions are open.", still returns a value, and the backtest fails at the end of the current tick. |

**Returns:** `number`

Returns the total amount open in a specific position.

---

## GetPositionDirection

```lua
GetPositionDirection([positionId])
```

Gets the direction of a specific position.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |

**Returns:** `enum`

Returns PositionLong, PositionShort or NoPosition.

---

## GetPositionEnterPrice

```lua
GetPositionEnterPrice([positionId], [includeClosed])
```

Gets the average enter price of an open position.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |
| `includeClosed` | `boolean` | No | True for total position amount (default), false for only open amount. |

**Returns:** `number`

Returns the average enter price.

---

## GetPositionMarket

```lua
GetPositionMarket([positionId])
```

Gets the market of an open position.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |

**Returns:** `string`

Returns the account, market & leverage as a single string.

---

## GetPositionProfit

```lua
GetPositionProfit([positionId], [targetPrice])
```

Gets the total realized plus unrealized profit of a specific position.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |
| `targetPrice` | `number` | No | Optional target price. Default is the exit-side best quote: the bid for a long, the ask for a short. |

**Returns:** `number`

Returns the total realized plus unrealized profit of a specific position. On spot, margin and linear (quote-margined) contracts the result is in the quote currency; on inverse contracts it is in the settlement (base) coin.

For an open long on spot: (current price − average enter price) × amount, minus the entry fee; the coming exit fee is not deducted. With `targetPrice`, an estimated exit fee at that price is also deducted (targetPrice × amount × fee rate), so passing a target changes the formula, not just the price. After the position closes, `GetPositionProfit(positionId)` returns the realized profit (both fees deducted); the form without a positionId returns 0.

---

## GetPositionROI

```lua
GetPositionROI([positionId], [targetPrice])
```

Calculates the ROI based on used margin and unrealized + realized profits. While the position is open the ROI uses the gross price move (fees not included), so it is about the price move × leverage and can be positive while GetPositionProfit() is negative; after the close it uses realized profit net of fees. Pass the positionId after the close (the no-argument form then returns 0).

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |
| `targetPrice` | `number` | No | Optional target price. Default is the exit-side best quote: the bid for a long, the ask for a short. |

**Returns:** `number`

Returns the ROI as a percentage. On leverage markets this is the return on margin: the price change in percent multiplied by the set leverage, fees excluded.

After the position closes, `GetPositionROI(positionId)` returns the position's final realized ROI (net of fees) until a new position opens in that slot.

The ROI is derived from the position profit rounded to 8 decimals, so on small positions (notably inverse contracts, where profit is in BTC) it changes in steps of 0.00000001 ÷ margin, and `StopLossROI` / `TakeProfitROI` thresholds are quantised accordingly.

---

## IsPositionClosed

```lua
IsPositionClosed([positionId])
```

Checks whether or not a position is closed.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |

**Returns:** `boolean`

Returns true if position is closed, otherwise false. Returns false when no position has existed yet; it becomes true only after a position has closed.

---

## LongAmount

```lua
LongAmount([market])
```

Gets the total amount open in a long position.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `number`

Returns the total amount open in a long position.

---

## PositionContainer

```lua
PositionContainer([positionId], [includeClosed])
```

Gets all the information from the open bot position.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |
| `includeClosed` | `boolean` | No | True for total position amount (default), false for only open amount. Affects enterPrice value in returned object. |

**Returns:** `dynamic`

Returns an array with position information.

A closed position can still be read with its positionId. Without an argument and with no position, an empty default is returned ("", zeros, NoPosition).

**Container fields** (accessible by name or 1-based index):

| Index | Name | Type | Description |
|-------|------|------|-------------|
| 1 | `positionId` | `string` | The position identifier. |
| 2 | `market` | `string` | The position key: account id, market and a suffix joined by `_` (for example `<accountId>_BINANCE_BTC_USDT__0`), not a plain market name. |
| 3 | `isLong` | `boolean` | Set on true when the direction is long. |
| 4 | `isShort` | `boolean` | Set on true when the direction is short. |
| 5 | `enterPrice` | `number` | The average enter price. |
| 6 | `amount` | `number` | The amount open in the position. |
| 7 | `profit` | `number` | The realized + unrealized profits. |
| 8 | `roi` | `number` | The ROI based on the used margin and realized + unrealized profits. |
| 9 | `openTime` | `number` | Timestamp (in unix) when the position was opened. |
| 10 | `updatedTime` | `number` | Timestamp (in unix) when the position was last updated. |
| 11 | `closeTime` | `number` | Timestamp (in unix) when the position was closed. If position is still open, value will be 0 |
| 12 | `direction` | `enum` | The position direction: PositionLong, PositionShort or NoPosition. Keeps the last direction after the position is closed. |

---

## ShortAmount

```lua
ShortAmount([market])
```

Gets the total amount open in a short position.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `number`

Returns the total amount open in a short position.

---


# Position Prices

## AverageEnterPrice

```lua
AverageEnterPrice([positionId], [includeClosed])
```

Gets the average enter price.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |
| `includeClosed` | `boolean` | No | True for total position amount (default), false for only open amount. |

**Returns:** `number`

Returns the average enter price.

---

## AverageExitPrice

```lua
AverageExitPrice([positionId])
```

Gets the average exit price.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |

**Returns:** `number`

Returns the average exit price.

---

## LastExitLongPrice

```lua
LastExitLongPrice()
```

Gets the last used exit long price.

**Returns:** `number`

Returns the last used exit long price.

---

## LastExitPositionPrice

```lua
LastExitPositionPrice()
```

Gets the last used exit short or long price.

**Returns:** `number`

Returns the last used exit short or long price.

---

## LastExitShortPrice

```lua
LastExitShortPrice()
```

Gets the last used exit short price.

**Returns:** `number`

Returns the last used exit short price.

---

## LastLongPrice

```lua
LastLongPrice()
```

Gets the last enter long price.

**Returns:** `number`

Returns the last enter long price.

---

## LastLongProfit

```lua
LastLongProfit([positionId])
```

Gets the profit of the last long exit or sell trade.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |

**Returns:** `number`

Returns the gross profit of the last exit (fees not deducted), equal to GetOrderProfit() of the exit order. For an unmanaged position, pass the positionId: without it the command returns 0 once the position is closed.

---

## LastShortPrice

```lua
LastShortPrice()
```

Gets the last enter short price.

**Returns:** `number`

Returns the last enter short price.

---

## LastShortProfit

```lua
LastShortProfit([positionId])
```

Gets the profit of the last short exit or buy trade.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |

**Returns:** `number`

Returns the profit.

---


# Signal Helpers

## BoolToSignal

```lua
BoolToSignal([isLong], [isShort], [isExit], [isNone])
```

Converts 4 boolean values to a signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `isLong` | `boolean` | No | When true, SignalLong will be returned. |
| `isShort` | `boolean` | No | When true, SignalShort will be returned. |
| `isExit` | `boolean` | No | When true, SignalExitPosition will be returned. |
| `isNone` | `boolean` | No | When true, SignalNone will be returned. |

**Returns:** `enum`

The signal as a result of the boolean values. When isLong is true, SignalLong is returned regardless of the other arguments. Without arguments, SignalNone is returned.

---

## ConvertSignal

```lua
ConvertSignal(signal, signalToCovert, convertedSignal)
```

**Aliases:** `MapSignal`

Converts the signal if its matches the defined one.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `signal` | `enum` | Yes | The original signal. |
| `signalToCovert` | `enum` | Yes | The signal we want to convert. |
| `convertedSignal` | `enum` | Yes | The signal we want to convert too. |

**Returns:** `enum`

Returns the original or converted signal depending on the input.

---

## DelaySignal

```lua
DelaySignal(signal, minutes)
```

Saves and delays a signal by a number of minutes.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `signal` | `enum` | Yes | The original signal. |
| `minutes` | `number` | Yes | The number of minutes of the delay. |

**Returns:** `enum`

Returns the signal that was passed in exactly `minutes` updates earlier, for one update only, or SignalNone otherwise. Call it on every update; each call site keeps its own history, so two calls with the same input do not share it.

---

## GetAboveBelowSignal

```lua
GetAboveBelowSignal(fast, slow)
```

Returns a continuous signal when values are above/below of each other.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `fast` | `number` | Yes | The 'faster' value. |
| `slow` | `number` | Yes | The 'slower' value. |

**Returns:** `enum`

Returns SignalLong when fast is above slow, SignalShort when fast is below slow and SignalNone will occur if values are exactly the same.

---

## GetBuySellLevelSignal

```lua
GetBuySellLevelSignal(value, buyLevel, sellLevel)
```

Determines the signal from a result with a buy and sell level.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `value` | `number` | Yes | The value to compare. |
| `buyLevel` | `number` | Yes | Below this value, a DoLong() is executed. |
| `sellLevel` | `number` | Yes | Above this value, a DoShort() is executed. |

**Returns:** `enum`

Returns SignalLong when the result is below the buy level, SignalShort when the result is above the sell level and SignalNone otherwise, including when the result equals either level.

---

## GetConsensusSignal

```lua
GetConsensusSignal(signals[])
```

Counts the number of signals. When more than half of all inputs agree, it returns that signal; SignalNone inputs count in the total, so (SignalLong, SignalNone) returns SignalNone while (SignalLong, SignalLong, SignalNone) returns SignalLong. A tie returns SignalNone.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `signals[]` | `EnumParams` | Yes | Indicator signals, passed as separate arguments (a variable number of them). A collection (Lua table, HNC or series) is not accepted and aborts the script. |

**Returns:** `enum`

Returns SignalLong, SignalShort, SignalExitPosition or SignalNone when there is a consensus or SignalNone when there is not.

---

## GetCrossOverUnderSignal

```lua
GetCrossOverUnderSignal(fast, slow)
```

Determines the signal from a result with a crossover or -under level.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `fast` | `number|table` | Yes | The 'faster' value. |
| `slow` | `number|table` | Yes | The 'slower' value. |

**Returns:** `enum`

Returns SignalLong when the a crossover occurs, SignalShort when a crossunder occurs and SignalNone when non one those happened.

---

## GetRemoteSignal

```lua
GetRemoteSignal(id)
```

Gets the remote signal which is published by SaveRemoteSignal() or the STORE_SIGNAL webhook. An incoming webhook signal triggers an immediate script execution, bypassing the normal tick interval. In the triggered update `Time()` still returns the start of the current minute, not the arrival time.

A script cannot contain both GetRemoteSignal() and SaveRemoteSignal(), even if neither is called; it is refused before compiling with "Using both GetRemoteSignal() and SaveRemoteSignal() in the same script isn't allowed."

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `id` | `string` | Yes | The unique identifier of the signal. |

**Returns:** `enum`

Returns a trade signal enumeration. For example SignalLong(), SignalShort(), SignalExitPosition(), SignalNone() or SignalError(). Returns SignalNone when no signal is stored for the id; no error is raised.

Each stored value is returned once; the next read returns SignalNone. The same value sent again within about a minute is not delivered again, although it still triggers an update.

In a backtest, returns the stored signal history: each bar returns the last signal received during its minute (earlier signals in that minute are not seen, and repeats suppressed live still appear); minutes without a signal return SignalNone.

The `STORE_SIGNAL` webhook stores a signal for this command:

`https://cloud.haasapi.com/SignalAPI.php?channel=STORE_SIGNAL&id=<id>&secret=<secret>&signal=<code>`

| Code | Signal |
|------|--------|
| 1 | `SignalLong` |
| 2 | `SignalShort` |
| 3 | `SignalExitPosition` |
| 4 | `SignalExitLong` |
| 5 | `SignalExitShort` |
| 6 | `SignalReservedA` (custom signal 1) |
| 7 | `SignalReservedB` (custom signal 2) |

---

## GetSuperSignal

```lua
GetSuperSignal(indicatorValue, signalValue, signalType, [centerPosition], [buyLevel], [sellLevel])
```

Returns a continuous trade signal with defined signal type and values.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `indicatorValue` | `HaasNumberCollection` | Yes | The 'faster' or indicator value. |
| `signalValue` | `HaasNumberCollection` | Yes | The 'slower' or signal value. |
| `signalType` | `enum` | Yes | The signal type which defines how the inputs are interpreted. |
| `centerPosition` | `number` | No | The center position for 'Zero Cross' signal types. 0 for indicators that oscillate below and above zero, 50 for indicators like RSI, MFI and STOCH. |
| `buyLevel` | `number` | No | The buy level for 'Threshold' signal types. Used for indicators like RSI, MFI and STOCH. |
| `sellLevel` | `number` | No | The sell level for 'Threshold' signal types. Used for indicators like RSI, MFI and STOCH. |

**Returns:** `enum`

Returns a signal based on defined 'signalType' (see InputSignalTypes()).

---

## GetThresholdSignal

```lua
GetThresholdSignal(value, threshold, [swing])
```

Determines the signal based on a result and a threshold.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `value` | `number` | Yes | The value to compare. |
| `threshold` | `number` | Yes | Below this value, a SignalLong is returned. If its above a SignalShort. |
| `swing` | `number` | No | Minimum swing below or above the threshold before a signal is returned. |

**Returns:** `enum`

Returns SignalLong when the result is below the threshold and SignalShort when the result is above the threshold.

---

## GetUnanimousSignal

```lua
GetUnanimousSignal(signals[])
```

Validates if all signals are the same. Returns a direction only if every input is that direction; any SignalNone input yields SignalNone.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `signals[]` | `EnumParams` | Yes | Indicator signals, passed as separate arguments (a variable number of them). A Lua table of signals is not accepted and aborts the script. |

**Returns:** `enum`

Returns SignalLong, SignalShort or SignalNone when there is unanimous decision and SignalNone when there is not.

---

## GetWebHookSignal

```lua
GetWebHookSignal(id)
```

Gets the latest incoming webhook signal by ID. Triggers immediate script execution when a webhook arrives, bypassing the normal minute tick interval — use this for high-performance signal-based trading. In the triggered update `Time()` still returns the start of the current minute, not the arrival time. Use the payload's `webhook_received_at` for the arrival time.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `id` | `string` | Yes | The webhook signal ID to listen for. Must match the key used in the incoming PUSH_SIGNAL webhook request. |

**Returns:** `dynamic`

Returns the latest signal payload stored by the incoming webhook, or nil if no signal has been received. The returned object always contains `webhook_received_at` (a Unix timestamp set by the server when the signal was received). All other fields are whatever was included in the incoming webhook payload, as strings; convert numbers with tonumber() before arithmetic or comparison.

The payload is a plain Lua table holding only the sender's fields plus `webhook_received_at`; the `ttl` parameter is not included. The first read removes the payload: a second call, even in the same update, returns nil, so keep the result in a variable.

Returns nil in backtests; webhook payloads are not replayed.

**Example payload:**

```lua
{
  webhook_received_at = 1711800000,  -- always present (Unix timestamp)
  action = "buy",                    -- user-defined
  price = "42000",                   -- user-defined; values arrive as strings
  symbol = "BTCUSDT"                 -- user-defined
}
```

> **Warning:** Once a signal has been read, it is removed from the system. An unread payload expires after the number of seconds given by the webhook's `ttl` parameter; after that the call returns nil. It is recommended to use a different signal ID for each bot to avoid conflicts.

---

## GetWeightedConsensusSignal

```lua
GetWeightedConsensusSignal(longWeight, shortWeight, exitWeight, signals[])
```

Calculates and determines the weighted consensus signal. Each signal adds its weight (1 by default, or the weight set with `SignalProperties`) to its direction; `SignalNone` inputs are ignored. A direction is returned only if it alone reaches its required weight. If more than one direction reaches its weight, or none does, the result is SignalNone. For example `GetWeightedConsensusSignal(2, 2, 2, SignalLong, SignalLong, SignalShort, SignalShort)` returns SignalNone, while `GetWeightedConsensusSignal(3, 2, 2, ...)` with the same signals returns SignalShort.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `longWeight` | `number` | Yes | The required sum of the long signals weight before the signal is valid. |
| `shortWeight` | `number` | Yes | The required sum of the short signals weight before the signal is valid. |
| `exitWeight` | `number` | Yes | The required sum of the exit signals weight before the signal is valid. |
| `signals[]` | `EnumParams` | Yes | Indicator signals, passed as separate arguments (a variable number of them). A Lua table of signals is not accepted and aborts the script. |

**Returns:** `enum`

Returns SignalLong, SignalShort, SignalExitPosition or SignalNone when there is a consensus or SignalNone when there is not.

---

## HindSightSignal

```lua
HindSightSignal(period)
```

Uses hindsight to tell if its the best moment to long or short

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `period` | `number` | Yes | Hindsight view period length. |

**Returns:** `enum`

Returns a trading signal

---

## IgnoreSignalIf

```lua
IgnoreSignalIf(signal, signalToIgnore)
```

Marks the signal to be ignored when it matches a certain signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `signal` | `enum` | Yes | The original signal. |
| `signalToIgnore` | `enum` | Yes | The signal to ignore. SignalLong, SignalShort or SignalExitPosition. |

**Returns:** `enum`

Returns the original signal.

---

## PositionToBool

```lua
PositionToBool(position)
```

Converts a position value to 3 boolean values.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `position` | `enum` | Yes | Bot position enum. PositionLong, PositionShort or NoPosition. |

**Returns:** `dynamic`

Three boolean values based on the position input.

**Container fields** (accessible by name or 1-based index):

| Index | Name | Type | Description |
|-------|------|------|-------------|
| 1 | `isLong` | `boolean` | True when the position is Long. |
| 2 | `isShort` | `boolean` | True when the position is Short. |
| 3 | `isNone` | `boolean` | True when the position is NoPosition. |

---

## ReverseSignal

```lua
ReverseSignal(signal)
```

Reverses a signal from long to short or short to long. SignalNone will be ignored and stay the same.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `signal` | `enum` | Yes | The original signal. |

**Returns:** `enum`

Returns the reversed signal.

---

## SaveRemoteSignal

```lua
SaveRemoteSignal(id, secret, signal)
```

This command can save and distribute a trading signal over the HaasCloud. This can be used in combination with trade signal enumeration. For example SignalLong(), SignalShort(), SignalExitPosition() or SignalNone().

A script cannot contain both GetRemoteSignal() and SaveRemoteSignal(), even if neither is called; it is refused before compiling with "Using both GetRemoteSignal() and SaveRemoteSignal() in the same script isn't allowed."

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `id` | `string` | Yes | The unique identifier of the signal. |
| `secret` | `string` | Yes | The secret key on which to save the signal. |
| `signal` | `enum` | Yes | The signal enum to save. |

**Returns:** `void`

> **Warning:** This command also runs in backtests, and a signal saved there is published to the live signal store, where it reaches live bots reading that signal id. Backtest a signal producer against a separate signal id.

---

## SignalMapper

```lua
SignalMapper(signal, [reverse], [mapLongTo], [mapShortTo], [mapExitTo], [mapNoneTo])
```

Maps a signal value to their defined specifications.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `signal` | `enum` | Yes | The original signal. |
| `reverse` | `boolean` | No | If true, the signals are revered. |
| `mapLongTo` | `enum` | No | The mapped long signal. |
| `mapShortTo` | `enum` | No | The mapped short signal. |
| `mapExitTo` | `enum` | No | The mapped exit signal. |
| `mapNoneTo` | `enum` | No | The mapped none signal. |

**Returns:** `enum`

The mapped signal.

---

## SignalProperties

```lua
SignalProperties(signal, [useLong], [useShort], [useExit], [weight], [delay])
```

Sets properties such as weight or ignore in the signal input.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `signal` | `enum` | Yes | The original signal. |
| `useLong` | `boolean` | No | When true, this signal is used for long decisions. |
| `useShort` | `boolean` | No | When true, this signal is used for short decisions. |
| `useExit` | `boolean` | No | When true, this signal is used for exit decisions. |
| `weight` | `number` | No | The weight of the signal. Minimum weight of 1 required. Used by GetWeightedConsensusSignal; GetConsensusSignal and GetUnanimousSignal ignore it. |
| `delay` | `number` | No | Number of minutes the signal will be delayed. The returned signal is the one from that many minutes earlier; until enough history exists it returns SignalNone. The delay counts minutes, not chart bars (delay 10 on a 5-minute chart is 10 minutes), and each call site keeps its own history. |

**Returns:** `enum`

Returns the signal with the new specifications.

A signal with `useLong`, `useShort` or `useExit` set to false is left out of that direction's check in `GetUnanimousSignal`. The returned value prints as the signal's name and compares equal to the plain constant (`SignalProperties(SignalShort) == SignalShort` is true).

---

## SignalToBool

```lua
SignalToBool(signal)
```

Converts a signal value to 4 boolean values.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `signal` | `enum` | Yes | The original signal. |

**Returns:** `dynamic`

Four boolean values based on the signal input.

**Container fields** (accessible by name or 1-based index):

| Index | Name | Type | Description |
|-------|------|------|-------------|
| 1 | `isLong` | `boolean` | True when the signal is SignalLong. |
| 2 | `isShort` | `boolean` | True when the signal is SignalShort. |
| 3 | `isExit` | `boolean` | True when the signal is SignalExitPosition. |
| 4 | `isNone` | `boolean` | True when the signal is SignalNone. |

---

## SignalToLog

```lua
SignalToLog(signals)
```

Logs the given signals.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `signals` | `EnumParams` | Yes | The signals to log. |

**Returns:** `void`

---

## SignalWeight

```lua
SignalWeight(signal, weight)
```

Add a weight to the signal. Can be used in combination with GetWeightedConsensusSignal().

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `signal` | `enum` | Yes | The original signal. |
| `weight` | `number` | Yes | The weight of the signal. Minimum weight of 1 required. |

**Returns:** `enum`

Returns the original signal with a weight mark.

---

## UseSignalIf

```lua
UseSignalIf(signal, signalToUse)
```

Marks the signal to be used when its matches a certain signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `signal` | `enum` | Yes | The original signal. |
| `signalToUse` | `enum` | Yes | The signal to ignore. SignalLong, SignalShort or SignalExitPosition. |

**Returns:** `enum`

Returns the original signal.

---


# Trade Actions (Managed)

## DoExitPosition

```lua
DoExitPosition([note], [count])
```

Sends out a sell signal on spot or an exit positions signal on margin & leverage.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `note` | `string` | No | A note for the order. Visible in the open orders and history. |
| `count` | `number` | No |  |

**Returns:** `void`

---

## DoFlipPosition

```lua
DoFlipPosition([note])
```

Executes a DoLong() when the current position is short and an DoShort() the position is long.

Places the exit and the opposite entry as two orders on the same update. On leverage markets the engine log labels them ExitLong / ExitShort and GoLong / GoShort (Sell / Buy on spot).

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `note` | `string` | No | A note for the order. Visible in the open orders and history. |

**Returns:** `void`

---

## DoLong

```lua
DoLong([note], [count])
```

**Aliases:** `DoBuy` (behaves identically; its log messages name `DoBuy()`, for example `DoBuy(): Blocking buy. Bot is already in a bought position.`)

Generates a buy signal on spot or a long signal on margin & leverage.

On margin and leverage while short, it closes the short and opens a long: two orders (ExitShort and GoLong) on the same update. The position reads the new side once both have filled. This also applies in hedge position mode; managed commands keep a single position (use unmanaged orders with separate position ids for a long and a short side by side).

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `note` | `string` | No | A note for the order. Visible in the open orders and history. |
| `count` | `number` | No | On spot it does not multiply the trade amount and does not allow extra entries while a position is open. |

**Returns:** `void`

---

## DoShort

```lua
DoShort([note], [count])
```

**Aliases:** `DoSell` (behaves identically; its log messages name `DoSell()`, for example `DoSell(): Blocking sell. Bot has not bought yet.`)

Generates a sell signal on spot or a short signal on margin & leverage.

On margin and leverage while long, it closes the long and opens a short: two orders (ExitLong and GoShort) on the same update. The position reads the new side once both have filled. This also applies in hedge position mode; managed commands keep a single position (use unmanaged orders with separate position ids for a long and a short side by side).

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `note` | `string` | No | A note for the order. Visible in the open orders and history. |
| `count` | `number` | No |  |

**Returns:** `void`

---

## DoSignal

```lua
DoSignal(signal, [note])
```

Processes a signal enum. Triggers a DoLong() when the signal equals SignalLong, a DoShort() when the signal equals SignalShort, a DoExitPosition() when the signal equals SignalExitPosition, SignalExitLong or SignalExitShort, and does nothing when the signal is SignalNone. SignalExitLong and SignalExitShort are not side-specific: either one closes the open position whatever its side. SignalError, SignalReservedA and SignalReservedB are ignored like SignalNone: no order, no log line, and an open position is kept.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `signal` | `enum` | Yes | The signal to process. |
| `note` | `string` | No | A note for the order. Visible in the open orders and history. |

**Returns:** `void`

---


# Trade Actions (Unmanaged)

## PlaceBuyOrder

```lua
PlaceBuyOrder(price, amount, [market], [type], [note], [positionId], [timeout], [triggerPrice], [reduceOnly], [hiddenOrder])
```

Places a buy order for the given amount and price. Returns the order identifier (text). Orders below the exchange minimum are blocked with a warning and nothing is placed. The minimum is a notional evaluated at the order's own price, so a limit far below the market needs a larger amount (see MinimumTradeAmount(market, price)). In backtests the quote wallet is debited at the order's price even when the order fills lower.

In backtests, orders larger than the available balance are not rejected; live bots reject them (the call logs "Insufficient funds" as an error and returns an empty string). Size orders from the wallet (for example `WalletAmount`) and check the returned identifier.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `price` | `number` | Yes | The price on which to execute the order. If the order is a market order, this field will be ignored. |
| `amount` | `number` | Yes | The amount to execute. |
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |
| `type` | `enum` | No | The type of order. Default is the bot's Order type setting (see GetOrderType()); pass it explicitly when the order must fill now. |
| `note` | `string` | No | A note for the order. Visible in the open orders and history. |
| `positionId` | `string` | No | Optional unique internal identifier that can be used to maintain multiple separate positions. |
| `timeout` | `number` | No | The order timeout in seconds. By default the timeout is 600 seconds / 10 minutes; 0 also uses the default. Using a negative value will disable the timeout. The timeout is enforced on the first update after it elapses. |
| `triggerPrice` | `number` | No | Trigger price, used for conditional orders. This parameter must only be set when using native order types that are supported by the exchange. |
| `reduceOnly` | `boolean` | No | Ensures that the executed order does not flip the opened position. |
| `hiddenOrder` | `boolean` | No | The hidden order option ensures an order does not appear in the order book. |

**Returns:** `string`

Unique order identifier, or an empty string when the order is blocked before placement (for example over the tracked-order cap, or with insufficient funds on a live bot). A returned identifier means the order was submitted, not that it was accepted: a rejection (for example "Order has failed to execute. Reason: Order would immediately execute" for a maker-or-cancel order priced through the market) arrives as a warning later in the update. Confirm with IsOrderOpen() / IsOrderFilled().

On a live bot with a market order type, the "Executing ... @ <price>" log line shows the price argument, not the fill price. Read the fill price from the order or the "order finished" log line.

---

## PlaceCancelledOrder

```lua
PlaceCancelledOrder(orderId, [price])
```

Executes the remaining amount of a canceled order.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `orderId` | `string` | Yes | Unique order identifier. Returned by the PlaceOrder commands. |
| `price` | `number` | No | The price on which to execute. Be defaults the best ask or bid price is used. |

**Returns:** `string`

Returns an new order identifier. Empty if no order is executed.

---

## PlaceExitLongOrder

```lua
PlaceExitLongOrder(price, amount, [market], [type], [note], [positionId], [timeout], [triggerPrice], [reduceOnly], [hiddenOrder])
```

Places an exit long order for the given amount and price. Returns the order identifier (text).

Margin and leverage markets only. On a spot market the order is refused with "Can not progress an margin/leverage order on a spot market.", the command returns an empty string, and the backtest fails at the end of that update; `pcall` does not catch it. Use `PlaceBuyOrder` / `PlaceSellOrder` on spot.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `price` | `number` | Yes | The price on which to execute the order. If the order is a market order, this field will be ignored. |
| `amount` | `number` | Yes | The amount to execute. |
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |
| `type` | `enum` | No | The type of order. Default is the bot's Order type setting (see GetOrderType()); pass it explicitly when the order must fill now. |
| `note` | `string` | No | A note for the order. Visible in the open orders and history. |
| `positionId` | `string` | No | Optional unique internal identifier that can be used to maintain multiple separate positions. |
| `timeout` | `number` | No | The order timeout in seconds. By default the timeout is 600 seconds / 10 minutes. Using a negative value will disable the timeout. |
| `triggerPrice` | `number` | No | Trigger price, used for conditional orders. This parameter must only be set when using native order types that are supported by the exchange. |
| `reduceOnly` | `boolean` | No | Ensures that the executed order does not flip the opened position. |
| `hiddenOrder` | `boolean` | No | The hidden order option ensures an order does not appear in the order book. |

**Returns:** `string`

Unique order identifier, or an empty string when the order is blocked before placement (for example over the tracked-order cap, or with insufficient funds on a live bot). A returned identifier means the order was submitted, not that it was accepted: a rejection (for example "Order has failed to execute. Reason: Order would immediately execute" for a maker-or-cancel order priced through the market) arrives as a warning later in the update. Confirm with IsOrderOpen() / IsOrderFilled().

---

## PlaceExitPositionOrder

```lua
PlaceExitPositionOrder([positionId], [price], [type], [note], [timeout])
```

Places exit orders for a specific bot position.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |
| `price` | `number` | No | The price on which to execute. Be defaults the best ask or bid price is used. |
| `type` | `enum` | No | Order type used for the order. |
| `note` | `string` | No | A note for the order. Visible in the open orders and history. |
| `timeout` | `number` | No | The order timeout in seconds. By default the timeout is 600 seconds / 10 minutes except for NoTimeOutOrderType. |

**Returns:** `string`

Returns an order identifier. When the position has nothing to exit, returns an empty string without a warning or error.

---

## PlaceExitShortOrder

```lua
PlaceExitShortOrder(price, amount, [market], [type], [note], [positionId], [timeout], [triggerPrice], [reduceOnly], [hiddenOrder])
```

Places an exit short order for the given amount and price. Returns the order identifier (text).

Margin and leverage markets only. On a spot market the order is refused with "Can not progress an margin/leverage order on a spot market.", the command returns an empty string, and the backtest fails at the end of that update; `pcall` does not catch it. Use `PlaceBuyOrder` / `PlaceSellOrder` on spot.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `price` | `number` | Yes | The price on which to execute the order. If the order is a market order, this field will be ignored. |
| `amount` | `number` | Yes | The amount to execute. |
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |
| `type` | `enum` | No | The type of order. Default is the bot's Order type setting (see GetOrderType()); pass it explicitly when the order must fill now. |
| `note` | `string` | No | A note for the order. Visible in the open orders and history. |
| `positionId` | `string` | No | Optional unique internal identifier that can be used to maintain multiple separate positions. |
| `timeout` | `number` | No | The order timeout in seconds. By default the timeout is 600 seconds / 10 minutes. Using a negative value will disable the timeout. |
| `triggerPrice` | `number` | No | Trigger price, used for conditional orders. This parameter must only be set when using native order types that are supported by the exchange. |
| `reduceOnly` | `boolean` | No | Ensures that the executed order does not flip the opened position. |
| `hiddenOrder` | `boolean` | No | The hidden order option ensures an order does not appear in the order book. |

**Returns:** `string`

Unique order identifier, or an empty string when the order is blocked before placement (for example over the tracked-order cap, or with insufficient funds on a live bot). A returned identifier means the order was submitted, not that it was accepted: a rejection (for example "Order has failed to execute. Reason: Order would immediately execute" for a maker-or-cancel order priced through the market) arrives as a warning later in the update. Confirm with IsOrderOpen() / IsOrderFilled().

---

## PlaceGoLongOrder

```lua
PlaceGoLongOrder(price, amount, [market], [type], [note], [positionId], [timeout], [triggerPrice], [reduceOnly], [hiddenOrder])
```

Places a go long order for the given amount and price. Returns the order identifier (text).

Margin and leverage markets only. On a spot market the order is refused with "Can not progress an margin/leverage order on a spot market.", the command returns an empty string, and the backtest fails at the end of that update; `pcall` does not catch it. Use `PlaceBuyOrder` / `PlaceSellOrder` on spot.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `price` | `number` | Yes | The price on which to execute the order. If the order is a market order, this field will be ignored. |
| `amount` | `number` | Yes | The amount to execute. |
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |
| `type` | `enum` | No | The type of order. Default is the bot's Order type setting (see GetOrderType()); pass it explicitly when the order must fill now. |
| `note` | `string` | No | A note for the order. Visible in the open orders and history. |
| `positionId` | `string` | No | Optional unique internal identifier that can be used to maintain multiple separate positions. |
| `timeout` | `number` | No | The order timeout in seconds. By default the timeout is 600 seconds / 10 minutes. Using a negative value will disable the timeout. |
| `triggerPrice` | `number` | No | Trigger price, used for conditional orders. This parameter must only be set when using native order types that are supported by the exchange. |
| `reduceOnly` | `boolean` | No | Ensures that the executed order does not flip the opened position. |
| `hiddenOrder` | `boolean` | No | The hidden order option ensures an order does not appear in the order book. |

**Returns:** `string`

Unique order identifier, or an empty string when the order is blocked before placement (for example over the tracked-order cap, or with insufficient funds on a live bot). A returned identifier means the order was submitted, not that it was accepted: a rejection (for example "Order has failed to execute. Reason: Order would immediately execute" for a maker-or-cancel order priced through the market) arrives as a warning later in the update. Confirm with IsOrderOpen() / IsOrderFilled().

---

## PlaceGoShortOrder

```lua
PlaceGoShortOrder(price, amount, [market], [type], [note], [positionId], [timeout], [triggerPrice], [reduceOnly], [hiddenOrder])
```

Places a go short order for the given amount and price. Returns the order identifier (text).

Margin and leverage markets only. On a spot market the order is refused with "Can not progress an margin/leverage order on a spot market.", the command returns an empty string, and the backtest fails at the end of that update; `pcall` does not catch it. Use `PlaceBuyOrder` / `PlaceSellOrder` on spot.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `price` | `number` | Yes | The price on which to execute the order. If the order is a market order, this field will be ignored. |
| `amount` | `number` | Yes | The amount to execute. |
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |
| `type` | `enum` | No | The type of order. Default is the bot's Order type setting (see GetOrderType()); pass it explicitly when the order must fill now. |
| `note` | `string` | No | A note for the order. Visible in the open orders and history. |
| `positionId` | `string` | No | Optional unique internal identifier that can be used to maintain multiple separate positions. |
| `timeout` | `number` | No | The order timeout in seconds. By default the timeout is 600 seconds / 10 minutes. Using a negative value will disable the timeout. |
| `triggerPrice` | `number` | No | Trigger price, used for conditional orders. This parameter must only be set when using native order types that are supported by the exchange. |
| `reduceOnly` | `boolean` | No | Ensures that the executed order does not flip the opened position. |
| `hiddenOrder` | `boolean` | No | The hidden order option ensures an order does not appear in the order book. |

**Returns:** `string`

Unique order identifier, or an empty string when the order is blocked before placement (for example over the tracked-order cap, or with insufficient funds on a live bot). A returned identifier means the order was submitted, not that it was accepted: a rejection (for example "Order has failed to execute. Reason: Order would immediately execute" for a maker-or-cancel order priced through the market) arrives as a warning later in the update. Confirm with IsOrderOpen() / IsOrderFilled().

---

## PlaceSellOrder

```lua
PlaceSellOrder(price, amount, [market], [type], [note], [positionId], [timeout], [triggerPrice], [reduceOnly], [hiddenOrder])
```

Places a sell order for the given amount and price. Returns the order identifier (text).

In backtests on spot, a sell larger than the position is not clipped: the order is split into a closing part (the position amount) and an extra part, and the extra part is tracked as an open short position. A sell with no position at all is likewise tracked as an open short. To close a spot position, sell `GetPositionAmount()`; the bought amount is already reduced by a fee charged in the base currency.

In backtests, orders larger than the available balance are not rejected; live bots reject them (the call logs "Insufficient funds" as an error and returns an empty string). Size orders from the wallet (for example `WalletAmount`) and check the returned identifier.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `price` | `number` | Yes | The price on which to execute the order. If the order is a market order, this field will be ignored. |
| `amount` | `number` | Yes | The amount to execute. |
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |
| `type` | `enum` | No | The type of order. Default is the bot's Order type setting (see GetOrderType()); pass it explicitly when the order must fill now. |
| `note` | `string` | No | A note for the order. Visible in the open orders and history. |
| `positionId` | `string` | No | Optional unique internal identifier that can be used to maintain multiple separate positions. |
| `timeout` | `number` | No | The order timeout in seconds. By default the timeout is 600 seconds / 10 minutes. Using a negative value will disable the timeout. |
| `triggerPrice` | `number` | No | Trigger price, used for conditional orders. This parameter must only be set when using native order types that are supported by the exchange. |
| `reduceOnly` | `boolean` | No | Ensures that the executed order does not flip the opened position. |
| `hiddenOrder` | `boolean` | No | The hidden order option ensures an order does not appear in the order book. |

**Returns:** `string`

Unique order identifier, or an empty string when the order is blocked before placement (for example over the tracked-order cap, or with insufficient funds on a live bot). A returned identifier means the order was submitted, not that it was accepted: a rejection (for example "Order has failed to execute. Reason: Order would immediately execute" for a maker-or-cancel order priced through the market) arrives as a warning later in the update. Confirm with IsOrderOpen() / IsOrderFilled().

On a live bot with a market order type, the "Executing ... @ <price>" log line shows the price argument, not the fill price. Read the fill price from the order or the "order finished" log line.

---


# Trade Bot

## IndicatorContainer

```lua
IndicatorContainer([signals[]])
```

Merges multiple signals into a single list and determines the unanimous and consensus signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `signals[]` | `EnumParams` | No | Indicator signals. Takes a variable number of arguments. |

**Returns:** `dynamic`

Returns a list with on index 1 the unanimous and on index 2 the consensus signal.

Only `SignalLong`, `SignalShort` and `SignalExitPosition` are merged. `SignalExitLong`, `SignalExitShort`, `SignalReservedA`, `SignalReservedB` and `SignalError` count as `SignalNone` in both the unanimous and the consensus result, so passing them through the container never triggers a trade.

Each call logs one line listing its inputs (for example `IndicatorContainer(): SignalLong: SignalLong | SignalShort: SignalShort`) unless `DisableIndicatorContainerLogs()` has been called.

**Container fields** (accessible by name or 1-based index):

| Index | Name | Type | Description |
|-------|------|------|-------------|
| 1 | `unanimousSignal` | `enum` | The unanimous signal. |
| 2 | `consensusSignal` | `enum` | The consensus signal. |

---

## InsuranceContainer

```lua
InsuranceContainer([signals[]])
```

Validates if all the signals agree.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `signals[]` | `...boolean` | No | Safety signals collection. Takes a variable number of arguments. |

**Returns:** `boolean`

Returns true if all the signals in the collection are true. Pass the signals as separate booleans: a Lua table is not expanded (`InsuranceContainer({true, false})` returns true). With no arguments it returns true. This is not a trading command; a script also needs TradeBotContainer() or a Do* command.

---

## SafetyContainer

```lua
SafetyContainer([signals[]])
```

Checks a collection for active safety signals.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `signals[]` | `...boolean` | No | Safety signals collection. Takes a variable number of arguments. |

**Returns:** `boolean`

Returns true if any of the signals is active. Pass the signals as separate booleans: a Lua table is not expanded (`SafetyContainer({false, true})` returns false). With no arguments it returns false. This is not a trading command; a script also needs TradeBotContainer() or a Do* command.

---

## TradeBotContainer

```lua
TradeBotContainer([safetySignal], [indicatorSignal], [insuranceSignal])
```

Checks the safety, indicator & insurance signal. If a safety is active, DoExitPosition() will be executed. When there are no safeties active, the indicator signal will be executed if the insurances agree. The insurance also blocks indicator exit signals (SignalExitPosition): while it is false, only a safety can close the position. A safety exit logs "TradeBotContainer(): A safety has triggered." and its order log lines end with " (S)". A safety while flat calls DoExitPosition() anyway, which logs the harmless warning "Blocking sell. Bot has not bought yet." With no arguments the command does nothing.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `safetySignal` | `boolean` | No | Safety container result. |
| `indicatorSignal` | `enum` | No | Indicator container result. |
| `insuranceSignal` | `boolean` | No | Insurance container result. |

**Returns:** `void`

---


# Trade Market Information

## AmountDecimals

```lua
AmountDecimals([market], [amount])
```

Returns a number of how many decimals is allowed for order amounts.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |
| `amount` | `number` | No |  |

**Returns:** `number`

Returns the allowed decimal count.

---

## AmountLabel

```lua
AmountLabel([market])
```

**Aliases:** `AmountCurrency`

Gets the amount label. For reflection, in case the market is Bitstamp BTC/USDit will return 'BTC'. When we are on BitMEX XBT/USD (XBTUSD), it will return 'contract(s)'. On a Bybit inverse perpetual it returns 'Contract(s)', while `ProfitLabel()` returns the settlement coin ('BTC').

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `string`

Returns the amount label as string.

---

## AmountStep

```lua
AmountStep([market])
```

Returns the minimum allowed trade amount step for market.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `number`

Returns the trade amount step size.

---

## BaseCurrency

```lua
BaseCurrency([market])
```

Returns the base currency of the market. For reflection, in case we are on Bitstamp BTC/USD it will return 'BTC'.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `string`

Returns the base currency as string.

---

## ContractName

```lua
ContractName([market])
```

Gets the contract name of the market.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `string`

Returns the contract name of the market.

---

## ContractType

```lua
ContractType([market])
```

Gets the contract type of the market (Vanilla, Inverse, or Quanto) as a number code; see Returns.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `number`

Returns a number, not one of the contract-type constants: 2807 for a linear (USDT-margined) contract and 2808 for an inverse contract. Use MarketType() to tell spot from futures and ContractName() for the contract name.

---

## ContractValue

```lua
ContractValue([market])
```

Gets the value of a single contract, defined in their denomination.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `number`

Returns the value of a single contract, defined in their denomination.

On an inverse USD contract (for example `BYBIT_BTC_USD_PERPETUAL`) returns 1: one contract is worth 1 USD, and trade and position amounts are counted in contracts.

---

## GetAccountMarkets

```lua
GetAccountMarkets([accountId])
```

An array of markets affiliated with a particular account.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `accountId` | `string` | No | The guid returned by AccountGuid(), InputAccount or InputAccountMarket for example. |

**Returns:** `dynamic`

Returns a Lua table (1-based list) of market strings in the form `<EXCHANGE>_<BASE>_<QUOTE>_`, for example `BINANCE_BTC_USDT_`. Its size depends on the account.

---

## GetExchangeMarkets

```lua
GetExchangeMarkets(exchangeCode)
```

An array of markets affiliated with a particular exchange.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `exchangeCode` | `string` | Yes | The exchange code affiliated with the exchange. |

**Returns:** `dynamic`

Returns a Lua table (1-based list) of market strings in the form `<EXCHANGE>_<BASE>_<QUOTE>_`, for example `BINANCE_BTC_USDT_`.

---

## IsTradeAmountEnough

```lua
IsTradeAmountEnough(market, price, amount, [logWarning], [reduceOnlyExit])
```

Gets whether or not the trade amount is high enough for exchange limits.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | Yes | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |
| `price` | `number` | Yes | The price on which to check the trade amount. |
| `amount` | `number` | Yes | The amount to check. |
| `logWarning` | `boolean` | No | If true, a warning is logged. |
| `reduceOnlyExit` | `boolean` | No | If true, the amount is checked as a reduce-only exit. On spot markets, and on markets whose minimum is an amount rather than a notional value, it does not change the result. |

**Returns:** `boolean`

Returns whether or not the trade amount is high enough for exchange limits.

---

## IsValidMarket

```lua
IsValidMarket(market)
```

Checks if the input is a valid market.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | Yes | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `boolean`

Returns true if the market is valid.

---

## MakersFee

```lua
MakersFee([market])
```

Gets makers fee of the market.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `number`

Returns makers fee of the market as a percentage (0.1 means 0.1 %).

---

## MarketType

```lua
MarketType([market])
```

Gets the market type enumeration of the market.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `enum`

Returns SpotTrading, MarginTrading or LeverageTrading.

---

## MinimumTradeAmount

```lua
MinimumTradeAmount([market], [price])
```

Calculates the minimum trade amount on a certain price while keeping exchange specifications in consideration.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |
| `price` | `number` | No | The price on which to calculate the trade amount. By default the close price is used. |

**Returns:** `number`

Returns the minimum trade amount taking the decimal count, minimum amount and volume into account.

---

## ParseTradeAmount

```lua
ParseTradeAmount(market, price, amount)
```

Parses the trade amount to exchange specifications.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | Yes | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |
| `price` | `number` | Yes | The price on which to parse the trade amount. |
| `amount` | `number` | Yes | The amount to parse. |

**Returns:** `number`

Returns the trade amount adjusted for exchange specifications.

---

## ParseTradePrice

```lua
ParseTradePrice(market, price)
```

Parses the trade price to exchange specifications.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | Yes | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |
| `price` | `number` | Yes | The price on which to check the trade amount. |

**Returns:** `number`

Returns the trade price adjusted for exchange specifications.

---

## PriceDecimals

```lua
PriceDecimals([market], [price])
```

**Aliases:** `QuoteDecimals`

Returns a number of how many decimals is allowed for order prices.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |
| `price` | `number` | No | Specific price value. This can have an effect on the number of decimals. |

**Returns:** `number`

Returns the allowed decimal count.

---

## PriceStep

```lua
PriceStep([market])
```

**Aliases:** `QuoteStep`

Returns the minimum allowed price step for market.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `number`

Returns the price step size.

---

## ProfitLabel

```lua
ProfitLabel([market])
```

**Aliases:** `ProfitCurrency`

Returns the profit label. For reflection, in case the market is Bitstamp BTC/USD it will return 'USD'. When we are on BitMEX XBT/USD (XBTUSD), it will return 'XBT'.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `string`

Returns the profit label as string.

---

## QuoteCurrency

```lua
QuoteCurrency([market])
```

Returns the quote currency of the market. For reflection, in case we are on Bitstamp BTC/USD it will return 'USD'.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `string`

Returns the quote currency as string.

---

## SetFee

```lua
SetFee(percentage, [market])
```

Saves a custom fee percentage which is used in back tests and returned by Fee(). Not a trading command: a script still needs a trading command to trade.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `percentage` | `number` | Yes | Fee percentage to store. |
| `market` | `string` | No | The guid returned by AccountGuid(), InputAccount or InputAccountMarket for example. |

**Returns:** `void`

---

## TakersFee

```lua
TakersFee([market])
```

Gets takers fee of the market.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `number`

Returns takers fee of the market as a percentage (0.1 means 0.1 %).

---

## TradeMarketContainer

```lua
TradeMarketContainer([market])
```

Retrieves all the information on the trade market.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `dynamic`

Returns the market information on an array.

**Container fields** (accessible by name or 1-based index):

| Index | Name | Type | Description |
|-------|------|------|-------------|
| 1 | `baseCurrency` | `string` | The base currency of the market. For reflection, in case we are on Bitstamp BTC/USD it will return 'BTC'. |
| 2 | `quoteCurrency` | `string` | The quote currency of the market. For reflection, in case we are on Bitstamp BTC/USD it will return 'USD'. |
| 3 | `contractName` | `string` | The contract name of the market. Empty on spot/margin markets. |
| 4 | `contractValue` | `number` | The value of a single contract, defined in their denomination. |
| 5 | `makersFee` | `number` | Makers fee of the market. |
| 6 | `takersFee` | `number` | Takers fee of the market. |
| 7 | `underlyingAsset` | `string` | The underlying assets of the market. For reflection, in case we are on Bitstamp BTC/USD it will return 'BTC'. When we are on BitMEX XBT/USD (XBTUSD), it will return 'XBT'. |
| 8 | `minimumTradeAmount` | `number` | The minimum trade amount on a the last price while keeping exchange specifications in consideration. |
| 9 | `minimumTradeVolume` | `number` | The minimum trade volume. Denominated in the quote currency. |
| 10 | `calculatedMinTradeAmount` | `number` | The minimum trade amount on a the last price while keeping exchange specifications in consideration. |
| 11 | `profitLabel` | `string` | The profit label of the market. For reflection, in case the market is Bitstamp BTC/USD it will return 'BTC'. When we are on BitMEX XBT/USD (XBTUSD), it will return 'XBT'. |
| 12 | `amountLabel` | `string` | The amount label of the market. For reflection, in case the market is Bitstamp BTC/USD it will return 'BTC'. When we are on BitMEX XBT/USD (XBTUSD), it will return 'contract(s)'. |
| 13 | `marketType` | `enum` | The market type enumeration of the market. |

---

## UnderlyingAsset

```lua
UnderlyingAsset([market])
```

Returns the underlying assets of the market. For reflection, in case we are on Bitstamp BTC/USD it will return 'BTC'. When we are on BitMEX XBT/USD (XBTUSD), it will return 'XBT'.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `string`

Returns the underlying assets as string.

---

## UsedMargin

```lua
UsedMargin(market, price, amount, leverage)
```

Calculates the used margin for a specific position.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | Yes | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |
| `price` | `number` | Yes | Position price. |
| `amount` | `number` | Yes | Position amount. |
| `leverage` | `number` | Yes | Leverage ratio. Use 0 for cross-margin. |

**Returns:** `number`

Returns price × amount / leverage, in the underlying currency value. With leverage 0 (cross margin) the market's maximum leverage (`GetMaxLeverage()`) is used.

On spot markets the leverage is ignored and the full notional (price × amount) is returned.

On inverse contracts the amount is in contracts and the result is in the settlement coin: amount ÷ price ÷ leverage (for example `UsedMargin(market, 83563, 100, 10)` = 0.00011967 BTC). Margin, profit, fees, wallet and report figures are then all in the settlement coin.

---


*Auto-generated from HaasScript source code.*
