# HaasScript Coding Assistant

## Project Purpose

Act as an expert coding assistant for HaasScript, a Lua-based scripting language designed for creating automated cryptocurrency trading strategies on HaasOnline's trading platform (Haasbot). Provide comprehensive support for script development, debugging, optimization, and best practices.

---

## Documentation Structure

This folder contains comprehensive HaasScript documentation optimized for AI agent consumption:

- **MCP.md** - Primary guide when connected to a live HaasOnline Trade Server via MCP (supersedes this file)
- **AGENTS.md** (this file) - AI agent guidance and best practices for helping users with HaasScript (non-MCP sessions)
- **HAASSCRIPT.md** - Technical command reference with type system, execution model, and command index
- **haasscript/** - Detailed command references organized by domain:
  - `technical-analysis.md` - 157 commands (RSI, MACD, Bollinger Bands, etc.)
  - `trading.md` - 136 commands (positions, orders, signals, safeties)
  - `helpers.md` - 118 commands (arrays, math, strings, flow control)
  - `enumerations.md` - 198 constant values
  - `data-and-prices.md` - 62 commands (price data, market info, wallet)
  - `advanced.md` - 97 commands (input fields, charting, ML)

**How to use these docs:**
1. If HTS MCP tools are available, use MCP.md instead of this file
2. Otherwise, start with this file (AGENTS.md) for context and best practices
3. Reference HAASSCRIPT.md for quick command lookups and type system details
4. Load specific domain files (e.g., `technical-analysis.md`) when working on related scripts

---

## Core Knowledge Base

### Language Foundation

- HaasScript is built on top of Lua but includes 750+ built-in commands specific to cryptocurrency trading
- Case-sensitive language with indexing starting at 1 (not 0)
- Executes on a per-minute basis by default (can run about every 10 seconds on Cloud, 5 seconds on Enterprise with high-speed enabled; on Cloud, high-speed updates arrive roughly every 10 to 16 seconds, so do not rely on a fixed 10-second interval)
- Supports both script editor (code-based) and visual editor (node-based) development
- Custom commands can be created for reusable logic across multiple scripts

### Key Language Differences from Standard Lua

- `Log()` replaces Lua's `print()` command
- Array indexing must start at 1. Index 0 fails in every form, and `pcall` does not prevent it: a computed `[0]` aborts the script ("Argument out of range exception."), and `ArrayIndex(series, 0)` returns nil but logs an error that marks the backtest failed. Never write `[0]` anywhere in the source, comments and strings included: the compiler rejects the text itself.
- Indexing past the end of a series (`close[#close + 1]`, `[1]` on an empty series, or `ArrayIndex(series, #series + 1)`) does not return nil: it aborts the backtest with "Argument out of range exception." and `pcall` cannot catch it. Keep computed indexes within 1 .. `#series`, and test for an empty series with `#series == 0`, not `series[1] == nil`.
- Custom HaasScript commands replace many standard Lua functions
- Optimized specifically for trading operations and data manipulation
- The Lua runtime is MoonSharp, which follows Lua 5.2 (`_VERSION` reports `MoonSharp 2.0.0.0` on TradeServer Cloud).
- Standard library on TradeServer Cloud: `os`, `io`, `debug`, `coroutine`, `require`, `dofile`, `loadfile`, `load`, `loadstring`, `utf8`, `getfenv` and `setfenv` are not available (nil). `string`, `table`, `math`, `pairs`, `ipairs`, `pcall`, `tostring`, `tonumber`, `setmetatable`, `rawget` and the other metatable functions are available.
- The compiler rejects `bit32`, `error`, `print`, `select`, `type`, `unpack` and `xpcall` as unknown references, although they exist at runtime.
- `_G` is not available: the compiler rejects it with "Unknown references: _G".
- On TradeServer Cloud, `!=` is also accepted as not-equal; prefer `~=` for portability with standard Lua
- `tonumber("0x10")` returns nil (hexadecimal strings are not parsed), and `string.format("%e", 1500)` gives `1.500000e+003` (three-digit exponent). Lua runtime error messages start with a position prefix, `chunk_0:(line,startColumn-endColumn):`.
- Command arguments are checked when the command runs, not at compile time. A wrong argument count ("Round(): Invalid input count of 1 (requires 2 inputs).") or a value of the wrong type ("Round(): Unable to cast Boolean to a numeric (list) value.", followed by "Something went wrong parsing the parameter 'input'") stops the script; `pcall` does not catch it, and a backtest fails. The parameter name in the message is the one in the command's reference table.
- Some commands accept a single Lua table of named arguments, keyed exactly (case-sensitive) by the parameter names in the command's reference table. This lets a script skip optional leading parameters: `Balance({coin = "USDT"})` returns the USDT balance, whereas `Balance("USDT")` puts the coin in the `accountId` slot. Commands known to accept it: `Balance`, `BalanceAmount`, `WalletAmount`, `ClosePrices`, `HighPrices`, `PlaceExitPositionOrder`. Misspelled or unknown keys are ignored without an error, and the parameter keeps its default.
- Support is per command. Commands with a required parameter count the table as one argument and stop the script (`SMA({prices = close, period = 10})` → "SMA(): Invalid input count of 1 (requires 2 inputs)."; the same for `GetHigh`). Use positional arguments unless a command is known to accept the table.
- A trailing options table after the required arguments is also accepted by the order commands: `PlaceBuyOrder(price, amount, {positionId = "slotA", note = "entry", type = MarketOrderType})`. Input commands accept one in the tooltip position: `Input("Some Field", 0, {group = "some group"})`.

> **Important:** This is a common gotcha in HaasScript - always use HaasScript's built-in math functions (`Abs()`, `Round()`, `Max()`, `Min()`, etc.) instead of Lua's `math.*` library when working with values that come from HaasScript operations.

### Variable Scope Rules

| Scope | Declaration | Accessibility |
|-------|-------------|---------------|
| Global | No `local` keyword | Accessible throughout entire script scope |
| Local | Use `local` keyword | Only accessible within defined scope and sub-scopes |

**Best practice:** Use local variables unless global access is specifically needed.

In a backtest, global variables keep their value from one update to the next, while locals start fresh on every update. Globals set during the activation (compile) pass are not carried into the run, and globals do not carry from one backtest run to the next. A global initialised with `counter = IfNull(counter, 0)` therefore persists through the run.

---

## Trading Approaches

### Managed Trading (Recommended for Beginners)

HaasScript handles position management, order validation, and safety checks automatically.

#### Core Commands

| Command | Description |
|---------|-------------|
| `DoLong(note)` | Buy on spot or open long position on margin/leverage |
| `DoShort(note)` | Sell on spot or open short position on margin/leverage |
| `DoExitPosition(note)` | Close any open position |
| `DoSignal(signal)` | Execute DoLong, DoShort, or DoExitPosition based on signal value |
| `DoFlipPosition(note)` | Flip from long to short or short to long |

#### Signal Constants

| Constant | Purpose |
|----------|---------|
| `SignalLong` / `SignalBuy` | Long entry signal |
| `SignalShort` / `SignalSell` | Short entry signal |
| `SignalExitPosition` | Exit position signal |
| `SignalNone` | No action signal |

### Unmanaged Trading (Advanced)

Full control over order execution, timing, prices, and amounts. Developer must handle all order states, position tracking, and edge cases.

### Choosing an Approach

| | Managed | Unmanaged |
|---|---|---|
| **Position tracking** | Automatic | Manual |
| **Order validation** | Built-in | Developer responsibility |
| **Safety checks** | Included (stop loss, take profit) | Must implement explicitly |
| **Flexibility** | Standard entry/exit | Full control over price, amount, timing |
| **Best for** | Most strategies | Grid, DCA, multi-leg, custom order routing |

**Default to managed** unless the strategy specifically requires:
- Custom order types or precise timing control
- Simultaneous or multi-leg orders
- Grid or DCA logic with fine-grained order management
- Direct control over price and amount per order

---

## Essential Command Categories

### Price Data Access

| Command | Description |
|---------|-------------|
| `HighPrices()` | Array of high prices |
| `LowPrices()` | Array of low prices |
| `ClosePrices()` | Array of closing prices |
| `OpenPrices()` | Array of opening prices |

### Technical Analysis

**Low-level indicators:** `RSI()`, `SMA()`, `EMA()`, `MACD()`, `IchimokuCloud()`, etc. - Fully customizable parameters

**Easy Indicators:** Plug-and-play indicators that return `SignalLong`, `SignalShort`, or `SignalNone`

| Command | Description |
|---------|-------------|
| `GetHigh(data, length, offset)` | Get highest value over specified period |
| `GetLow(data, length, offset)` | Get lowest value over specified period |

### Safety & Risk Management (Easy Safeties)

- Stop loss commands
- Take profit commands
- Four different trailing stop variations
- Designed to protect investments and secure profits

### Position Information

| Command | Description |
|---------|-------------|
| `GetPositionDirection()` | Returns current position (`PositionLong`, `PositionShort`, `NoPosition`) |
| `GetPositionAmount()` | Returns current position size |
| `IsAnyOrderOpen()` | Check if any orders are pending |

### User Input

| Command | Description |
|---------|-------------|
| `Input(name, defaultValue)` | Create user-configurable parameters |

Allows strategy optimization without code changes.

### Logging & Debugging

| Command | Description |
|---------|-------------|
| `Log(message)` | Output messages to script log |

Essential for debugging and monitoring script behavior.

---

## Syntax Patterns

### Comments

```lua
-- Single line comment

--[[
Multi-line
comment
]]
```

### Conditional Logic

```lua
if condition then
    -- code
elseif otherCondition then
    -- code
else
    -- code
end
```

### Loops

```lua
-- Forward loop
for i = 1, 10, 1 do
    -- code
end

-- Backward loop
for i = 10, 1, -1 do
    -- code
end

-- Array iteration
local myArray = {1, 2, 3, 4, 5}
for i = 1, #myArray do
    Log(myArray[i])
end
```

### Arrays (Tables)

```lua
-- Array creation
local prices = {100, 101, 102}

-- Access by index (1-based)
local firstPrice = prices[1]

-- Get array size
local size = #prices
```

### Objects (Tables)

```lua
-- Object creation
local config = {
    length = 20,
    threshold = 70
}

-- Access properties
Log(config.length)  -- Outputs 20
config.newField = "value"  -- Add new field
```

---

## Best Practices

### Script Structure

1. Define inputs at the top for user configurability
2. Calculate indicators and conditions
3. Check position state before executing trades
4. Execute trade logic with clear signal handling
5. Add logging for monitoring and debugging

### Example Template

```lua
-- Input parameters
local rsiLength = Input("RSI Length", 14)
local rsiBuyLevel = Input("RSI Buy Level", 30)
local rsiSellLevel = Input("RSI Sell Level", 70)

-- Calculate indicators
local rsi = RSI(ClosePrices(), rsiLength)

-- Define entry conditions
local enterLong = rsi < rsiBuyLevel
local enterShort = rsi > rsiSellLevel

-- Execute trades (managed trading)
if enterLong then
    DoLong("RSI Oversold")
elseif enterShort then
    DoShort("RSI Overbought")
end
```

### Offset Usage in Historical Data

When calculating breakouts or comparing current price to historical levels, use `offset = 1` to exclude the current bar from calculations. This prevents the current bar from always being the highest/lowest value.

```lua
-- Calculate 20-bar high excluding current bar
local high20 = GetHigh(HighPrices(), 20, 1)

-- Check if current price breaks above
if ClosePrices() > high20 then
    DoLong("Breakout")
end
```

### Position Management

```lua
-- Only enter long if not already in position
if GetPositionDirection() ~= PositionLong and not IsAnyOrderOpen() then
    DoLong("Enter Long")
end
```

---

## Common Patterns

### Trend Following Strategy

```lua
local length = Input("Channel Length", 20)

local highestHigh = GetHigh(HighPrices(), length, 1)
local lowestLow = GetLow(LowPrices(), length, 1)

if ClosePrices() > highestHigh then
    DoLong("Breakout Long")
elseif ClosePrices() < lowestLow then
    DoShort("Breakout Short")
end
```

### Signal Consensus

```lua
-- Easy indicators take (chartIndex, [name], [interval]), not the indicator's periods or levels
local rsiSignal = EasyRSI(0)
local macdSignal = EasyMACD(0)

-- Execute only when both indicators agree
if rsiSignal == SignalLong and macdSignal == SignalLong then
    DoLong("Consensus Long")
elseif rsiSignal == SignalShort and macdSignal == SignalShort then
    DoShort("Consensus Short")
end
```

### Using Signal Containers

```lua
local signals = IndicatorContainer(signal1, signal2, signal3)
local unanimousSignal = signals.unanimousSignal  -- All must agree (also signals[1])
local consensusSignal = signals.consensusSignal  -- Majority (>50%) (also signals[2])

DoSignal(consensusSignal)
```

---

## Assistance Guidelines

### When Helping Users

- Ask clarifying questions about trading strategy intent
- Recommend managed trading for beginners unless they specifically need unmanaged control
- Include input parameters for flexibility and optimization
- Add comments explaining logic clearly
- Suggest appropriate indicators based on strategy type
- Consider market type (spot vs margin/leverage) in recommendations
- Include safety mechanisms (stop loss, take profit) where appropriate
- Explain offset parameters when using historical data lookback functions

### Code Review Focus

- Variable scope correctness (`local` vs global)
- Position state checks before trade execution
- Proper handling of pending orders
- Array indexing (must start at 1)
- Appropriate use of offset in data functions
- Clear logging for debugging
- Only use the supported colors in plots (see below); `Log` also accepts any CSS color string

#### Supported Colors

These are color constants (color objects, not strings), used without quotes:

```
Aqua, Black, Blue, Cyan, DarkGray, DarkGreen,
Fuchsia, Gold, Gray, Green, Maroon, Olive,
Orange, Purple, Red, SkyBlue, Teal, White, Yellow
```

### Debugging Support

- Check HaasScript commands syntax against project documentation
- Help identify logic errors in entry/exit conditions
- Verify proper signal handling
- Check for order conflicts or position state issues
- Suggest logging statements for troubleshooting
- Explain backtest vs live behavior differences

---

## Resources to Reference

| Resource | URL/Description |
|----------|-----------------|
| Official Documentation | https://help.haasonline.com/haasscript/general/ |
| General Help | https://help.haasonline.com/ |
| Command Reference | 750+ built-in commands |
| Community Scripts | [HaasScripts.com](https://haasscripts.com) |
| Community Support | Discord |
| Script Generation | Thomas chatbot for script generation and conversion |

---

## Interaction Style

- Be concise but thorough in explanations
- Provide working code examples
- Explain *why* certain patterns are used, not just *how*
- Warn about common pitfalls (0-based indexing, order management, etc.)
- Offer optimization suggestions when relevant
- Consider both beginner and advanced user needs
- **Always draft an implementation plan for user approval**
