# Helpers & Utilities

118 functions across 7 categories.

## Categories

- [Array Helpers](#array-helpers) (21 commands)
- [Equations](#equations) (15 commands)
- [Flow Control](#flow-control) (4 commands)
- [Mathematical](#mathematical) (49 commands)
- [Memory Helpers](#memory-helpers) (11 commands)
- [Miscellaneous](#miscellaneous) (8 commands)
- [String Helpers](#string-helpers) (10 commands)

---

# Array Helpers

The array helpers accept Lua tables, HaasNumberCollections and engine series (price data) alike, with identical results; the result keeps the input's form. Exceptions: `ArrayRemove` accepts Lua tables only, and `ArrayConcat` needs an engine collection first when mixing forms.

## ArrayAdd

```lua
ArrayAdd(input, element)
```

Adds an element to an existing array.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `dynamic` | Yes | Source data. |
| `element` | `dynamic` | Yes | Element which to add to the array. |

**Returns:** `dynamic`

Returns a new array; the input is not modified. A Lua table in gives a Lua table out; a series in gives a series out.

---

## ArrayAny

```lua
ArrayAny(input, [value])
```

**Aliases:** `ArrayContains`

Returns true if the array contains any element or an element matches a specific value.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `dynamic` | Yes | Source data. |
| `value` | `dynamic` | No | Optional value to match. |

**Returns:** `boolean`

Returns true if the input has any elements or if 'value' is defined when any of the elements matches the value.

---

## ArrayConcat

```lua
ArrayConcat(array1, array2)
```

Concatenates two arrays; the first array's elements come first. If `array1` is a Lua table, `array2` must be a Lua table too: a Lua table followed by a HaasNumberCollection or engine series stops the script ("Object reference not set to an instance of an object.") without `pcall` catching it. To mix forms, put the engine collection first; the result is then an engine collection.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `array1` | `dynamic` | Yes | First array. |
| `array2` | `dynamic` | Yes | Second array. |

**Returns:** `dynamic`

Returns the concatenated array.

---

## ArrayDistinct

```lua
ArrayDistinct(input)
```

Returns a new list with only unique elements. In other words; removes all duplicates.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `dynamic` | Yes | Source data. |

**Returns:** `dynamic`

Returns the unique values of the input.

---

## ArrayFilter

```lua
ArrayFilter(input, value, [filterType])
```

Returns a new list with elements that only matches the value. Matching-rules specified by filterType.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `dynamic` | Yes | Source data. |
| `value` | `dynamic` | Yes | Value to match. |
| `filterType` | `enum` | No | The type of filter to use. Default is ArrayFilterInclusiveType. |

**Returns:** `dynamic`

Returns the list.

---

## ArrayFind

```lua
ArrayFind(input, value, [filterType])
```

Returns a new list of indices of the matching values. Matching-rules specified by filterType.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `dynamic` | Yes | Source data. |
| `value` | `dynamic` | Yes | Value to match. |
| `filterType` | `enum` | No | The type of filter to use. Default is ArrayFilterInclusiveType. |

**Returns:** `dynamic`

Returns the list of indices; an empty list when nothing matches. The index base depends on the input: 1-based for a Lua table, 0-based for a HaasNumberCollection or an engine series (price data). Add 1 before indexing a series with the result.

---

## ArrayIndex

```lua
ArrayIndex(input, index)
```

**Aliases:** `ArrayGet`

Gets the value on a specific index. A negative index will take from the end of the array.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `dynamic` | Yes | Source data. |
| `index` | `number` | Yes | The index to get. |

**Returns:** `dynamic`

Return the value on that index.

Behavior by element type:

- Numbers: returned as a number.
- Pairs (for example the elements of `GetOrderbookAsk()`): only the first component is returned. `ArrayGet(GetOrderbookAsk(), 1)` is the best ask price; use `GetOrderbookAsk()[1]` for the pair and `ArrayGet(pair, 2)` for the amount.
- Enums in an engine container: returned as the enum's numeric code (`ArrayGet(UserPositionContainer(), 8)` gives a number where `[8]` gives `NoPosition`). Enums in a Lua table are returned unchanged. Use brackets to get the enum.
- Booleans: returned unchanged from a Lua table. A boolean element of an engine collection (for example `ArrayGet(Compare(2, 1), 1)`) stops the script with "Unable to cast Boolean to a numeric value." Use brackets.
- Strings: stop the script with "Unable to cast <value> to a numeric value." Read string fields with brackets or by name.

The two errors above are not caught by `pcall`, and a backtest fails.

---

## ArrayLast

```lua
ArrayLast(input, [offset])
```

Gets the last value of the array with an optional offset.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `dynamic` | Yes | Source data. |
| `offset` | `number` | No | The offset from the last index. |

**Returns:** `dynamic`

Return the last value of the array taking the offset into account.

---

## ArrayPop

```lua
ArrayPop(input)
```

Remove an item from the end of an array.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `dynamic` | Yes | Source data. |

**Returns:** `dynamic`

Returns a new array; the input is not modified. A Lua table in gives a Lua table out; a series in gives a series out.

---

## ArrayRemove

```lua
ArrayRemove(input, index)
```

Removes an element from a specified index.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `dynamic` | Yes | Source data. Lua tables only: a HaasNumberCollection or an engine series stops the script ("Object reference not set to an instance of an object.") without `pcall` catching it. Convert to a Lua table first. |
| `index` | `number` | Yes | Index of the element to remove. |

**Returns:** `dynamic`

Returns a new array; the input is not modified. A Lua table in gives a Lua table out; a series in gives a series out.

---

## ArrayReplace

```lua
ArrayReplace(input, oldValue, newValue)
```

Replaces a specific value with another value.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `dynamic` | Yes | Source data. |
| `oldValue` | `dynamic` | Yes | The old value. |
| `newValue` | `dynamic` | Yes | The new value. |

**Returns:** `dynamic`

The input with replaced values.

---

## ArrayShift

```lua
ArrayShift(input)
```

Remove an item from the beginning of an array.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `dynamic` | Yes | Source data. |

**Returns:** `dynamic`

The input without the first value

---

## ArraySort

```lua
ArraySort(input, [descending])
```

Sorts the array values in ascending or descending order.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `dynamic` | Yes | Source data. |
| `descending` | `boolean` | No | The order of the sort. If set to true, order will be descending. Otherwise ascending. |

**Returns:** `dynamic`

Returns the sorted array.

---

## ArraySum

```lua
ArraySum(input, [key])
```

Computes the sum of the array. Can be used in combination with a list of numbers or container results like GetAllOpenOrders() or GetAllFilledOrders().

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `dynamic` | Yes | Source data. |
| `key` | `string` | No | The key of the data to be summed. |

**Returns:** `number`

Returns the sum of the array.

---

## ArrayUnshift

```lua
ArrayUnshift(input, element)
```

Adds an element to the beginning of an array.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `dynamic` | Yes | Source data. |
| `element` | `dynamic` | Yes | Element which to add to the array. |

**Returns:** `dynamic`

The input with the element added in front of the array

---

## Count

```lua
Count(input, [value])
```

Gets the length of an array or the number of occurrences within the array of a specific value.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `dynamic` | Yes | Source data. |
| `value` | `dynamic` | No | Optional value to match. |

**Returns:** `number`

Returns the length of an array or the number of occurrences within the array of a specific value.

---

## Grab

```lua
Grab(input, offset, [count])
```

**Aliases:** `Range`

Grabs a range from the array. If count is not set, return all after the offset.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `dynamic` | Yes | Source data. |
| `offset` | `number` | Yes | Offset from start. |
| `count` | `number` | No | Number of items to grab. |

**Returns:** `dynamic`

Specified range from the array.

---

## NewArray

```lua
NewArray([elements[]])
```

**Aliases:** `NewList`

Creates an array. Not available in scripts on TradeServer Cloud: `NewArray` and `NewList` are nil there (calling them fails with "attempt to call a nil value"). Use a Lua table literal such as `{1, 2, 3}`.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `elements[]` | `...dynamic` | No | Optional array element. |

**Returns:** `dynamic`

Returns an array with the input elements.

---

## NewSortedArray

```lua
NewSortedArray([elements1], [elements2], [elements3], [elements4], [elements5], [elements6], [elements7], [elements8], [elements9], [elements10])
```

**Aliases:** `NewSortedList`

Creates a sorted array. Not available in scripts on TradeServer Cloud: `NewSortedArray` is nil there. Build a Lua table and sort it with `table.sort` or `ArraySort`.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `elements1` | `dynamic` | No | Optional array element 1. |
| `elements2` | `dynamic` | No | Optional array element 2. |
| `elements3` | `dynamic` | No | Optional array element 3. |
| `elements4` | `dynamic` | No | Optional array element 4. |
| `elements5` | `dynamic` | No | Optional array element 5. |
| `elements6` | `dynamic` | No | Optional array element 6. |
| `elements7` | `dynamic` | No | Optional array element 7. |
| `elements8` | `dynamic` | No | Optional array element 8. |
| `elements9` | `dynamic` | No | Optional array element 9. |
| `elements10` | `dynamic` | No | Optional array element 10. |

**Returns:** `dynamic`

Returns an sorted array with the input elements.

---

## Offset

```lua
Offset(input, offset)
```

Gets an offset amount of the given array.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `dynamic` | Yes | Source data. |
| `offset` | `number` | Yes | Number of record to remove. |

**Returns:** `dynamic`

Returns the given array with the offset amount of data removed from it.

---

## SourceManager

```lua
SourceManager(newValue, [interval], [cap], [initialValues])
```

Stores numeric values in a collection. Only adds the new value when the candle on the specified interval closed. This results in interval based data.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `newValue` | `number` | Yes | This value will be added in front of the collection when the candle closed. |
| `interval` | `number` | No | The interval on which the data is based. Default is the selected main interval. |
| `cap` | `number` | No | Maximum size of the collection. |
| `initialValues` | `HaasNumberCollection` | No | The initial values when source in initialized |

**Returns:** `HaasNumberCollection`

Returns the numeric collection.

---


# Equations

## Compare

```lua
Compare(input1, input2)
```

Checks if input1 is below, equal to or above input2.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input1` | `number|table` | Yes | Source value(s) 1. |
| `input2` | `number|table` | Yes | Source value(s) 2. |

**Returns:** `dynamic`

Returns a list of 3 booleans for the newest pair of values: `[1]` isAbove (input1 is above input2), `[2]` isEqual, `[3]` isBelow.

**Container fields** (accessible by name or 1-based index):

| Index | Name | Type | Description |
|-------|------|------|-------------|
| 1 | `isAbove` | `boolean` | Is true when input1 is above input2. |
| 2 | `isEqual` | `boolean` | Is true when input1 is equals input2. |
| 3 | `isBelow` | `boolean` | Is true when input1 is below input2. |

---

## Equals

```lua
Equals(value1, value2)
```

Compares two values and returns whether or not the values are equal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `value1` | `dynamic` | Yes | First value to compare. |
| `value2` | `dynamic` | Yes | Second value to compare. |

**Returns:** `boolean`

Returns true when the values are equal.

---

## False

```lua
False()
```

Creates a false value.

**Returns:** `boolean`

Returns false.

---

## IfNull

```lua
IfNull(value, defaultValue)
```

Sets the defaultValue if the value is nil.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `value` | `dynamic` | Yes | The value to compare. |
| `defaultValue` | `dynamic` | Yes | The default value when the value is not set. |

**Returns:** `dynamic`

Returns the value of defaultValue when value is nil.

---

## IsBiggerAndSmallerThan

```lua
IsBiggerAndSmallerThan(value, lowValue, highValue)
```

Checks if the value is bigger than lowValue and smaller then highValue.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `value` | `number|table` | Yes | The value to compare. |
| `lowValue` | `number` | Yes | The lowest value. |
| `highValue` | `number` | Yes | The highest value. |

**Returns:** `boolean`

Returns true the value is bigger than lowValue and smaller then highValue.

---

## IsBiggerOrSmallerThan

```lua
IsBiggerOrSmallerThan(value, highValue, lowValue)
```

Checks if the value is bigger than highValue or smaller then lowValue.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `value` | `number|table` | Yes | The value to compare. |
| `highValue` | `number` | Yes | The highest value. |
| `lowValue` | `number` | Yes | The lowest value. |

**Returns:** `boolean`

Returns true the value is bigger than highValue or smaller then lowValue.

---

## IsBiggerThan

```lua
IsBiggerThan(input1, input2)
```

Checks if input1 is bigger than input2.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input1` | `number|table` | Yes | The value to compare. |
| `input2` | `number` | Yes | The value to compare with. |

**Returns:** `boolean`

Returns true input1 is bigger than input2.

---

## IsFalse

```lua
IsFalse(values[])
```

**Aliases:** `Not`

Validates all values on being false.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `values[]` | `...boolean` | Yes | The boolean to compare. |

**Returns:** `boolean`

Returns true all values are false.

---

## IsNotNull

```lua
IsNotNull(value)
```

Checks if the value is not null.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `value` | `dynamic` | Yes | The value to compare. |

**Returns:** `boolean`

Returns true if the value is not null.

---

## IsNull

```lua
IsNull(value)
```

Checks if the value is null.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `value` | `dynamic` | Yes | The value to compare. |

**Returns:** `boolean`

Returns true if the value is null.

---

## IsSmallerThan

```lua
IsSmallerThan(input1, input2)
```

Checks if input1 is smaller than input2.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input1` | `number|table` | Yes | The value to compare. |
| `input2` | `number` | Yes | The value to compare with. |

**Returns:** `boolean`

Returns true if input1 is smaller than input2.

---

## IsTrue

```lua
IsTrue(values[])
```

**Aliases:** `And`

Validates all values on being true.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `values[]` | `...boolean` | Yes | The booleans to compare, as separate arguments. A Lua table is not expanded: only its first element is checked (`IsTrue({true, false})` = true). A boolean engine collection, such as the result of `Compare()`, stops the script without `pcall` catching it. |

**Returns:** `boolean`

Returns true all values are true.

---

## NotEquals

```lua
NotEquals(value1, value2)
```

Compares two values and returns whether or not the values are different.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `value1` | `dynamic` | Yes | First value to compare. |
| `value2` | `dynamic` | Yes | Second value to compare. |

**Returns:** `boolean`

Returns true when the values are not equal.

---

## Or

```lua
Or(values[])
```

Validates if any of the values is true.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `values[]` | `...boolean` | Yes | The booleans to compare, as separate arguments. A Lua table is not expanded: only its first element is checked (`Or({false, true})` = false). A boolean engine collection, such as the result of `Compare()`, stops the script without `pcall` catching it. |

**Returns:** `boolean`

Returns true if any of the all values is true.

---

## True

```lua
True()
```

Creates a true value.

**Returns:** `boolean`

Returns true.

---


# Flow Control

## Branch

```lua
Branch(input)
```

Validates the input boolean and sets the output result accordingly. This block can be useful in decision-based flows. Not available in scripts on TradeServer Cloud: the name `Branch` is nil there.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `boolean` | Yes | Input boolean to check. |

**Returns:** `boolean`

Returns a list of boolean results.

**Container fields** (accessible by name or 1-based index):

| Index | Name | Type | Description |
|-------|------|------|-------------|
| 1 | `isTrue` | `boolean` | Set on true if the input is true. |
| 2 | `isFalse` | `boolean` | Set on true if the input is false. |

---

## IfElse

```lua
IfElse(statement, value1, value2)
```

**Aliases:** `Switch`

Will return 1 of 2 values based on the input boolean.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `statement` | `boolean` | Yes | Input boolean to check. |
| `value1` | `dynamic` | Yes | If true, this value is passed on. |
| `value2` | `dynamic` | Yes | If false, this value is passed on. |

**Returns:** `dynamic`

Return the first value if true or the second value when false.

---

## IfElseIf

```lua
IfElseIf(statement1, statement2, value1, value2, value3)
```

IfElseIf returns a value based on the boolean inputs.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `statement1` | `boolean` | Yes | Statement one. |
| `statement2` | `boolean` | Yes | Statement two. |
| `value1` | `dynamic` | Yes | Value1, returns if statement1 is valid. |
| `value2` | `dynamic` | Yes | Value2, returns if statement2 is valid. |
| `value3` | `dynamic` | Yes | Value3, returns is statement 1 and 2 are invalid. |

**Returns:** `dynamic`

Return the first value if statement1 is true, return the second value if statement2 is true, or returns value3 when all statements are invalid.

---

## Merge

```lua
Merge([values[]])
```

This commands executes at the very last. When one of the input values is set, the execution will continue from this command.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `values[]` | `...dynamic` | No | Possible values for this flow. |

**Returns:** `dynamic`

Returns the first set input value or doesn't execute.

---


# Mathematical

The mathematical helpers return a `HaasNumberCollection`: a number input gives a 1-element series (`Abs(-5)` returns a series whose element 1 is 5), and series inputs give a series as long as the shortest series input, computed element by element. Convert the result to a plain number (§7.3 of the development skill) where Lua requires one, for example as a `for` limit.

## Abs

```lua
Abs(input)
```

Calculates an absolute/positive number.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `number|table` | Yes | Source value. |

**Returns:** `HaasNumberCollection`

Returns an absolute/positive number.

---

## Acos

```lua
Acos(input)
```

Calculates the angle whose cosine is the specified number.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `number|table` | Yes | A number representing a cosine, that must be greater than or equal to -1, but less than or equal to 1. |

**Returns:** `HaasNumberCollection`

Returns the angle whose cosine is the specified number.

---

## Add

```lua
Add(input1, [input2])
```

**Aliases:** `Sum`

Adds numbers/tables and returns the result number/table.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input1` | `number|table` | Yes | Source value(s) 1. |
| `input2` | `number|table` | No | Source value(s) 2. |

**Returns:** `HaasNumberCollection`

Returns the total sum of a single value is provide, adds the 2 values if there are 2 provided.

---

## AddPercentage

```lua
AddPercentage(value, percentage)
```

**Aliases:** `AddPerc`

Add a percentage to the value.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `value` | `number|table` | Yes | The main value. |
| `percentage` | `number` | Yes | The percentage to add. |

**Returns:** `HaasNumberCollection`

Returns the value with the added percentage.

---

## Asin

```lua
Asin(input)
```

Calculates the angle whose sine is the specified number.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `number|table` | Yes | Source value(s). |

**Returns:** `HaasNumberCollection`

Returns the angle whose sine is the specified number.

---

## Atan

```lua
Atan(input)
```

Calculates the angle whose tangent is the specified number.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `number|table` | Yes | A number representing a tangent. |

**Returns:** `HaasNumberCollection`

Returns the angle whose tangent is the specified number.

---

## Atan2

```lua
Atan2(input1, input2)
```

Calculates the angle whose tangent is the quotient of two specified numbers.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input1` | `number|table` | Yes | The y coordinate of a point. |
| `input2` | `number|table` | Yes | The x coordinate of a point. |

**Returns:** `HaasNumberCollection`

Returns the angle whose tangent is the quotient of two specified numbers.

---

## Average

```lua
Average(input1, [input2])
```

Calculates the average value of the array, two numbers or averages between 2 arrays.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input1` | `number|table` | Yes | Source value(s) 1. |
| `input2` | `number|table` | No | Source value(s) 2. |

**Returns:** `HaasNumberCollection`

Returns the average value of the array, two numbers or averages between 2 arrays.

---

## Average2

```lua
Average2(input, [period])
```

Calculates the rolling average of the array over a period, or the average of the whole array when no period is given.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `HaasNumberCollection` | Yes | Source values. |
| `period` | `number` | No | Window length of the rolling average. Without it, the overall average is returned. |

**Returns:** `HaasNumberCollection`

Returns a series of rolling averages of length count - period + 1 (`Average2({1, 2, 3, 4}, 2)` = 1.5, 2.5, 3.5), or a 1-element series without a period (`Average2({1, 2, 3, 4})` = 2.5).

---

## Ceil

```lua
Ceil(input)
```

Calculates the smallest integral value greater than or equal to the specified number.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `number|table` | Yes | Source value(s). |

**Returns:** `HaasNumberCollection`

Returns the smallest integral value greater than or equal to the specified number.

---

## Change

```lua
Change(prices)
```

Calculates the signed change (newer minus older) between adjacent values; the result has one element fewer than the input.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |

**Returns:** `HaasNumberCollection`

Returns the new-old change of the values in an array.

---

## Clamp

```lua
Clamp(input, min, max)
```

Calculates the number or table of numbers clamped between the specified minimum and maximum.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `number|table` | Yes | Source value(s). |
| `min` | `number` | Yes | Minimum value. |
| `max` | `number` | Yes | Maximum value. |

**Returns:** `HaasNumberCollection`

Returns number or table of numbers clamped between the specified minimum and maximum.

---

## Compress

```lua
Compress(input)
```

Calculates the average values of every 2 values in an array. When the input is [10, 20, 30, 40] the result will be [15, 35].

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `HaasNumberCollection` | Yes | Source data. |

**Returns:** `HaasNumberCollection`

Returns the compressed array.

---

## Cos

```lua
Cos(input)
```

Calculates the cosine of the specified angle.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `number|table` | Yes | Source value(s). |

**Returns:** `HaasNumberCollection`

Returns the cosine of the specified angle.

---

## Cosh

```lua
Cosh(input)
```

Calculates the hyperbolic cosine of the specified angle.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `number|table` | Yes | Source value(s). |

**Returns:** `HaasNumberCollection`

Returns the hyperbolic cosine of the specified angle.

---

## Delta

```lua
Delta(input1, [input2])
```

**Aliases:** `PercentageChange`

Calculates the percentage between input1 and input2 or the percentage change between the values of input1.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input1` | `number|table` | Yes | Source value(s) 1. |
| `input2` | `number|table` | No | Source value(s) 2. |

**Returns:** `HaasNumberCollection`

Returns the percentage between input1 and input2. When for example input1 is 5 and input2 is 6, 20 is returned.

---

## Div

```lua
Div(input1, input2)
```

Divides input1 with input2. If one of the parameters is a single value, the other is divided by this.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input1` | `number|table` | Yes | Source value(s) 1. |
| `input2` | `number|table` | Yes | Source value(s) 2. |

**Returns:** `HaasNumberCollection`

Returns the divided numbers/tables.

---

## Exp

```lua
Exp(input)
```

Calculates 'e' raised to the specified power.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `number|table` | Yes | Source value(s). |

**Returns:** `HaasNumberCollection`

Returns 'e' raised to the specified power.

---

## FilterAbove

```lua
FilterAbove(input, threshold)
```

Filters out all values strictly above a specific value; values equal to it are kept (`FilterAbove({5, 6, 4}, 5)` = 5, 4).

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `number|table` | Yes | Source data. |
| `threshold` | `number` | Yes | The maximum threshold value. |

**Returns:** `HaasNumberCollection`

Returns the filtered input.

---

## FilterBelow

```lua
FilterBelow(input, threshold)
```

Filters out all values strictly below a specific value; values equal to it are kept (`FilterBelow({5, 6, 4}, 5)` = 5, 6).

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `number|table` | Yes | Source data. |
| `threshold` | `number` | Yes | The old value. |

**Returns:** `HaasNumberCollection`

Returns the filtered input.

---

## Floor

```lua
Floor(input)
```

Calculates the largest integral value less than or equal to the specified number.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `number|table` | Yes | Source value(s). |

**Returns:** `HaasNumberCollection`

Returns the largest integral value less than or equal to the specified number.

---

## GetHigh

```lua
GetHigh(prices, depth, [offset])
```

Gets the highest value.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `depth` | `number` | Yes | Number of records to include. |
| `offset` | `number` | No | Number of records to skip. |

**Returns:** `number`

Returns the highest value.

---

## GetHighs

```lua
GetHighs(array, depth)
```

Gets the highest values within lookback period.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `array` | `HaasNumberCollection` | Yes | Source data. |
| `depth` | `number` | Yes | Number of records to include. |

**Returns:** `HaasNumberCollection`

Returns the highest values within lookback period.

---

## GetLow

```lua
GetLow(prices, depth, [offset])
```

Gets the lowest value.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `depth` | `number` | Yes | Number of records to include. |
| `offset` | `number` | No | Number of records to skip. |

**Returns:** `number`

Returns the lowest value.

---

## GetLows

```lua
GetLows(array, depth)
```

Gets the lowest values within lookback period.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `array` | `HaasNumberCollection` | Yes | Source data. |
| `depth` | `number` | Yes | Number of records to include. |

**Returns:** `HaasNumberCollection`

Returns the lowest values within lookback period.

---

## Ln

```lua
Ln(input1, [input2])
```

Calculates the logarithm of a specified number. Without input2 it is the natural logarithm; input2 is the base (`Ln(8, 2)` = 3).

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input1` | `number|table` | Yes | Source value(s) 1. |
| `input2` | `number|table` | No | Logarithm base. Default is the natural logarithm. |

**Returns:** `HaasNumberCollection`

Returns the logarithm of a specified number.

---

## Log10

```lua
Log10(input)
```

Calculates the base 10 logarithm of a specified number.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `number|table` | Yes | Source value(s). |

**Returns:** `HaasNumberCollection`

Returns the base 10 logarithm of a specified number.

---

## Max

```lua
Max(values[])
```

Returns the maximum value. If the input is a single array, it returns the biggest number in that array (single value). If inputs are both numbers, it returns the biggest number (single value). If inputs are array and number, it returns the biggest number (array[1] vs number, single value). If inputs are both arrays, it returns an array of the biggest numbers per cell (array, length determined by the shorter array).

`Max(series, 20)` is not the highest value of the last 20 bars; it compares only the newest element with 20. Use `GetHigh(series, 20)` for the highest of the last 20 values (and `GetLow` instead of `Min`).

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `values[]` | `NumberOrTableParams` | Yes | Source value(s). |

**Returns:** `HaasNumberCollection`

Returns the biggest number or an array of biggest numbers per cell.

---

## Min

```lua
Min(values[])
```

Returns the minimum value. If the input is a single array, it returns the minimum in that array (single value). If inputs are both numbers, it returns the smallest number (single value). If inputs are array and number, it returns the smallest number (array[1] vs number, single value). If inputs are both arrays, it returns an array of the smallest numbers per cell (array, length determined by shortest array)

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `values[]` | `NumberOrTableParams` | Yes | Source value(s). |

**Returns:** `HaasNumberCollection`

Returns the smallest number or an array of smallest numbers per cell.

---

## Mul

```lua
Mul(input1, input2)
```

**Aliases:** `Mult`

Multiplies input1 with input2. If one of the parameters is a single value, the other is multiplied by this.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input1` | `number|table` | Yes | Source value(s) 1. |
| `input2` | `number|table` | Yes | Source value(s) 2. |

**Returns:** `HaasNumberCollection`

Returns the multiplied numbers/tables.

---

## NumberMax

```lua
NumberMax
```

Constant value: the maximum number.

**Type:** `number`

Returns a maximum value (2147483647).

---

## NumberMin

```lua
NumberMin
```

Constant value: the minimum number.

**Type:** `number`

Returns a minimum value (-2147483648).

---

## PI

```lua
PI
```

Constant value: the number of PI.

**Type:** `number`

Returns PI rounded to 11 decimals (3.14159265359). `math.pi` holds the full double-precision value.

---

## Pow

```lua
Pow(value, power)
```

Calculates a specified number/table raised to the specified power number/table.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `value` | `number|table` | Yes | The base value |
| `power` | `number|table` | Yes | The power value |

**Returns:** `HaasNumberCollection`

Returns a specified number/table raised to the specified power number/table.

---

## Random

```lua
Random([min], [max])
```

Creates a random number.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `min` | `number` | No | Minimum value. Default is 0. |
| `max` | `number` | No | Minimum value. Default is 1. |

**Returns:** `number`

Returns a random number.

---

## ReplaceAbove

```lua
ReplaceAbove(input, threshold, newValue)
```

Replaces any value strictly above a specific value with another value; values equal to it are kept (`ReplaceAbove({5, 6}, 5, 0)` = 5, 0).

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `number|table` | Yes | Source data. |
| `threshold` | `number` | Yes | Values above this threshold will be replaced. |
| `newValue` | `number` | Yes | The new value. |

**Returns:** `HaasNumberCollection`

Returns the input with replaced values.

---

## ReplaceBelow

```lua
ReplaceBelow(input, threshold, newValue)
```

Replaces any value below a specific value with another value.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `number|table` | Yes | Source data. |
| `threshold` | `number` | Yes | Values below this threshold will be replaced. |
| `newValue` | `number` | Yes | The new value. |

**Returns:** `HaasNumberCollection`

Returns the input with replaced values.

---

## Round

```lua
Round(input, digits)
```

Rounds a value (or values if table) to the nearest integer or to the specified number of fractional digits.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `number|table` | Yes | Source value(s). |
| `digits` | `number` | Yes | Number of digits after decimal point. |

**Returns:** `HaasNumberCollection`

Returns the rounded number.

---

## Satoshi

```lua
Satoshi
```

**Aliases:** `Epsilon`

Constant value: 1 Satoshi (=Epsilon).

**Type:** `number`

Returns 1 Satoshi (=Epsilon): 0.00000001 (1E-08).

---

## Sd

```lua
Sd(input)
```

Calculates the Standard Deviation from the data series.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `HaasNumberCollection` | Yes | Source value(s). |

**Returns:** `number`

Returns the Standard Deviation.

---

## Sign

```lua
Sign(input)
```

Calculates an integer that indicates the sign of a number.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `number|table` | Yes | Source value(s). |

**Returns:** `HaasNumberCollection`

Returns an integer that indicates the sign of a number.

---

## Sin

```lua
Sin(input)
```

Calculates the sine of the specified angle.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `number|table` | Yes | Source value(s). |

**Returns:** `HaasNumberCollection`

Returns the sine of the specified angle.

---

## Sinh

```lua
Sinh(input)
```

Calculates the hyperbolic sine of the specified angle.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `number|table` | Yes | Source value(s). |

**Returns:** `HaasNumberCollection`

Returns the hyperbolic sine of the specified angle.

---

## Sqrt

```lua
Sqrt(input)
```

Calculates the square root of a specified number.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `number|table` | Yes | Source value(s). |

**Returns:** `HaasNumberCollection`

Returns the square root of a specified number.

---

## Sub

```lua
Sub(input1, input2)
```

Subtracts numbers/tables and returns the result number/table.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input1` | `number|table` | Yes | Source value(s) 1. |
| `input2` | `number|table` | Yes | Source value(s) 2. |

**Returns:** `HaasNumberCollection`

Calculates the subtracted numbers/tables.

---

## SubPercentage

```lua
SubPercentage(value, percentage)
```

**Aliases:** `SubPerc`

Subtracts a percentage to the value.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `value` | `number|table` | Yes | The main value. |
| `percentage` | `number` | Yes | The percentage to subtract. |

**Returns:** `HaasNumberCollection`

Returns the value with the subtracted percentage.

---

## Tan

```lua
Tan(input)
```

Calculates the tangent of the specified angle.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `number|table` | Yes | Source value(s). |

**Returns:** `HaasNumberCollection`

Returns the tangent of the specified angle.

---

## Tanh

```lua
Tanh(input)
```

Calculates the hyperbolic tangent of the specified angle.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `number|table` | Yes | Source value(s). |

**Returns:** `HaasNumberCollection`

Returns the hyperbolic tangent of the specified angle.

---

## Truncate

```lua
Truncate(input, digits)
```

Truncates a value (or values if table) to the nearest integer or to the specified number of fractional digits.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `number|table` | Yes | Source value(s). |
| `digits` | `number` | Yes | Number of digits after decimal point. |

**Returns:** `HaasNumberCollection`

Returns the truncated number.

---


# Memory Helpers

## Delete

```lua
Delete(key)
```

Deletes a value on a specific key.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `key` | `string` | Yes | Unique identifier. |

**Returns:** `void`

---

## InitExportData

```lua
InitExportData(name, headers)
```

Initialize a file that can be exported later. Rows are written with WriteExportData, which only records during a backtest. Cloud servers allow up to 25 export files per script.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `name` | `string` | Yes | Unique name of the file |
| `headers` | `...string` | Yes | Header definition |

**Returns:** `void`

---

## Load

```lua
Load(key, [defaultValue])
```

Loads a value saved on the specified key.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `key` | `string` | Yes | Unique identifier. |
| `defaultValue` | `dynamic` | No | Default value returned when no data is saved on the key; it is also saved on the key, so later calls return it (even with another default) until `Delete`. Default is nil. |

**Returns:** `dynamic`

Returns the saved value; if nothing is saved, the defaultValue (which is then saved), or nil when no defaultValue is given.

---

## Log

```lua
Log(message, [color], [prefix], [suffix])
```

Create a message in the log.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `message` | `dynamic` | Yes | Message to log. |
| `color` | `string` | No | The color of the log message: a color constant (`Red`, `Green(50)` for 50 % opacity), `Color(r, g, b, [a])` (alpha 0–255), or any CSS color string (named, `#00FF00`, `rgb(0, 128, 255)`, `rgba(255, 0, 0, 0.5)`). The whole line is colored. |
| `prefix` | `string` | No | Optional prefix for the log message |
| `suffix` | `string` | No | Optional suffix for the log message |

**Returns:** `void`

Given a series, `Log` prints the whole series as a bracketed list; color, prefix and suffix still apply: `Log(HNC(3, 7), White, "PREFIX ", " SUFFIX")` logs `PREFIX [7.0, 7.0, 7.0] SUFFIX`.

---

## LogError

```lua
LogError(message)
```

Create an error message in the log.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `message` | `dynamic` | Yes | Error to log. |

**Returns:** `void`

---

## LogWalletError

```lua
LogWalletError(message)
```

Create a warning message in the log ("WARNING: <message>") and activates the trade amount error. It does not block trading: managed and unmanaged orders on later updates are still executed, in backtests and live.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `message` | `dynamic` | Yes | Warning to log. |

**Returns:** `void`

---

## LogWarning

```lua
LogWarning(message)
```

Create a warning message in the log.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `message` | `dynamic` | Yes | Warning to log. |

**Returns:** `void`

---

## Save

```lua
Save(key, value)
```

Saves a value on a specific key. This value can be retrieved with Load(). A maximum of 10.000 keys can be stored. Storing a key beyond the limit stops the script with "Bot memory limitation reached. Maximum number of keys reached. Cannot store more than 10000 values"; `pcall` does not catch it, and a backtest fails. Nothing is dropped silently. Avoid keys derived from an unbounded counter.

Numbers, strings, booleans (including false) and Lua tables, nested ones included, round-trip unchanged. A number saved as a plain number is loaded as a plain Lua number.

Saving a collection slice such as `close[2]` stores the whole remaining series (for example 499 values), without an error. The loaded value supports arithmetic but fails as a `for` limit ("'for' limit must be a number"). Use `ArrayGet(close, 2)` to save a single number.

Inside a custom command, values are stored separately from the calling script and separately for each call site of the command. They persist between calls within a backtest and start fresh in each backtest. A command therefore cannot share a total across its call sites through `Save`/`Load`.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `key` | `string` | Yes | Unique identifier. |
| `value` | `dynamic` | Yes | The value to store. Must not be nil: `Save(key, nil)` aborts the backtest ("Save(): Attempting to call with 'null' value for value.") and `pcall` cannot catch it. Use `Delete(key)` to clear a key. |

**Returns:** `void`

---

## SessionGet

```lua
SessionGet(key)
```

Gets a value stored in the session memory. Visual-editor block: in scripts on TradeServer Cloud the name is nil. In scripts, use locals within an update and Save/Load between updates.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `key` | `string` | Yes | The key of the saved data. |

**Returns:** `dynamic`

Returns a value stored in the session memory.

---

## SessionSet

```lua
SessionSet(key, value)
```

Stores a value for this session only. Visual-editor block: in scripts on TradeServer Cloud the name is nil. In scripts, use locals within an update and Save/Load between updates.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `key` | `string` | Yes | The key of the saved data. |
| `value` | `dynamic` | Yes | The data to save. |

**Returns:** `void`

---

## WriteExportData

```lua
WriteExportData(name, values)
```

Write data to a file that can be exported later. Only records during a backtest. Self-hosted file size is unlimited, use with caution; cloud servers cap a file at 250,000 rows.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `name` | `string` | Yes | Name of the file to write the data to. |
| `values` | `...string` | Yes | The values to write to the file. Needs to match the number of headers defined in InitExportData. Any value type is accepted: a price or indicator series writes only its newest element ([1]), a signal enum its name (e.g. `SignalLong`), a boolean `True`/`False`, `Time()` Unix seconds, numbers and strings as is. |

**Returns:** `void`

> **Warning:** A row whose number of values differs from the headers is a fatal error. The engine logs "WriteExportData(): Error appending row to file '<name>'. Expected <n> values, got <m> values.", the rest of the update runs, and the backtest then fails with no report. `pcall` does not catch it.

Rows written before a backtest fails are kept and can still be retrieved.

---


# Miscellaneous

## Comment

```lua
Comment(value)
```

Creates a text area block for comments.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `value` | `string` | Yes | The comment value. |

**Returns:** `void`

---

## CustomReport

```lua
CustomReport(key, value, [group], [displayValue])
```

Creates an item in the bot's custom reports. Use group="Override" to override standard bot report fields (e.g. RealizedProfit, UnrealizedProfit) for both live bots and backtest results.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `key` | `string` | Yes | The name of the item. |
| `value` | `dynamic` | Yes | The value of the item. |
| `group` | `string` | No | The report group name. Defaults to 'Custom Report'. Use 'Override' to override a standard bot report field (e.g. CustomReport("RealizedProfit", myValue, "Override")). |
| `displayValue` | `boolean` | No | When set on true, this value will be visible on the bot list in the UI.'. |

**Returns:** `void`

In a backtest the custom report is rebuilt on every update: only the rows and overrides written on the last update remain in the result. Call `CustomReport` on every update or inside `Finalize`. Values are stored as strings. Overriding a profit field (e.g. `RealizedProfit`) does not recompute `ROI`.

**Overriding built-in report values:**

When `group` is `"Override"` and `key` matches a supported built-in field, the bot's report displays the custom `value` instead of the calculated value. Overrides also appear under an **Override** group in the Custom Report section for visibility. Never pass `nil` as the value: it fails the script. To remove an override, stop calling CustomReport for that key; passing 0 sets the field to 0 and does not clear the override.

Supported keys: `GrossProfit`, `FeeCosts`, `RealizedProfit`, `UnrealizedProfit`, `ROI`, `BuyAndHodl`, `ClosedPositions`, `WinningPositions`, `AvgProfit`, `AvgPositionSize`, `AvgPositionMargin`, `FilledOrders`, `PartiallyFilledOrders`, `CancelledOrders`, `FailedOrders`, `BiggestWin`, `BiggestLoss`, `SharpeRatio`, `SortinoRatio`, `WinPercentage`, `ProfitFactor`

```lua
-- Override profit section values
CustomReport("RealizedProfit", myNetPnL, "Override")
CustomReport("ROI", myROI, "Override")
CustomReport("FeeCosts", myTotalFees, "Override")

-- To clear an override, stop writing it (never pass nil)
```

---

## GetCommand

```lua
GetCommand(name)
```

Gets a command by name.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `name` | `string` | Yes | The name of the command. |

**Returns:** `dynamic`

Returns the command.

---

## GetType

```lua
GetType(value)
```

Gets the data type of the input.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `value` | `dynamic` | Yes | The input value. |

**Returns:** `enum`

Returns the data type.

---

## HNC

```lua
HNC([size], [value])
```

HNC is an array on which match can be applied directly. This is not position with the lua code `{}`.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `size` | `number` | No | The size of the array. Default 0. |
| `value` | `number` | No | The default value when size is set, rounded to an integer (ties to even: `HNC(1, 2.5)` holds 2, `HNC(1, 86382.99)` holds 86383). Default is 0. Values assigned afterwards with `h[i] = v` keep their decimals. |

**Returns:** `HaasNumberCollection`

Returns an array with or without values.

---

## Number

```lua
Number(value)
```

Creates a block with a numeric input field.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `value` | `number` | Yes | The input value. |

**Returns:** `number`

Returns the input value.

---

## Return

```lua
Return()
```

Stops the current update cycle. If used in a command script, it will only stop the execution of the current command, not the main script.

**Returns:** `void`

---

## Text

```lua
Text(value)
```

Creates a block with an input field.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `value` | `string` | Yes | The input value. |

**Returns:** `string`

Returns the input value.

---


# String Helpers

## NewGuid

```lua
NewGuid()
```

Creates an unique identifier.

**Returns:** `string`

Returns the unique identifier as string.

---

## Parse

```lua
Parse(value, type)
```

Converts the value to a specific type.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `value` | `dynamic` | Yes | The something. |
| `type` | `enum` | Yes | The parameter type enum. |

**Returns:** `dynamic`

Returns the converted value or nil if parsing fails.

---

## ParseCsv

```lua
ParseCsv(csv, [hasHeaders], [columnDelimiter], [rowDelimiter])
```

Parses CSV data string and returns it as an object. URLs are also supported, except on TradeServer Cloud: there a URL or path logs "The command `ParseCsv` with a url or path is not supported in the cloud version.", the command returns nil and the backtest is marked failed.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `csv` | `string` | Yes | The CSV data string or URL to download CSV data from (URLs not on TradeServer Cloud). |
| `hasHeaders` | `boolean` | No | Whether or not data contains headers (default: false). When true, the header row is skipped and each row is keyed by the header names (`rows[1]["h1"]`). |
| `columnDelimiter` | `string` | No | Delimiter for columns (default: ';') |
| `rowDelimiter` | `string` | No | Delimiter for rows (default: '\n') |

**Returns:** `dynamic`

A 1-based list of rows; each row is a 1-based list of cells, or keyed by header name when hasHeaders is true. All values are strings.

---

## ParseJson

```lua
ParseJson(json)
```

Parses JSON data string and returns it as an object. URLs are also supported, except on TradeServer Cloud: there a URL or path logs "The command `ParseJson` with a url or path is not supported in the cloud version.", the command returns nil and the backtest is marked failed. Any input that is not JSON-shaped, an empty string included, is treated as a URL or path. JSON-shaped but malformed input (for example `'{"a":1,'`) stops the script with a parse error (for example "Unexpected end of content while loading JObject.") that `pcall` does not catch, and a backtest fails; there is no way to validate the input inside the script. Parse only trusted input.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `json` | `string` | Yes | The JSON data string or URL to download JSON data from (URLs not on TradeServer Cloud). |

**Returns:** `dynamic`

A table mirroring the JSON: objects become keyed tables and arrays 1-based lists. Numbers, strings and booleans keep their JSON types. `#` gives the length of an array and 0 for an object.

String fields stay strings. They coerce in arithmetic (`"83000.5" + 1` is 83001.5) but not in comparisons. A string never equals a signal constant, not even its own name (`SignalBuy == "SignalBuy"` is false, and `"" .. SignalBuy` is "SignalLong"), so map signal names from JSON to constants explicitly, for example `if parsed.prediction == "SignalBuy" then signal = SignalLong end`.

---

## StringContains

```lua
StringContains(value, searchValue, [ignoreCase])
```

Returns a value indicating whether a specified substring occurs within this string.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `value` | `string` | Yes | The main string. |
| `searchValue` | `string` | Yes | The substring to search. |
| `ignoreCase` | `boolean` | No | Ignoring the case of the scripts being compared. |

**Returns:** `boolean`

Returns true if the specified substring occurs within this string.

---

## StringExplode

```lua
StringExplode(value, delimiter)
```

**Aliases:** `StringSplit`

Splits a string into substrings that are based on the characters in an array.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `value` | `string` | Yes | The main string. |
| `delimiter` | `string` | Yes | The delimiter value. |

**Returns:** `dynamic`

Returns a list (Lua table) of strings: `StringExplode("a,b,c", ",")` gives `a`, `b`, `c`.

---

## StringFromQuery

```lua
StringFromQuery(value)
```

Split a string on '&' and '='.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `value` | `string` | Yes | The main string. |

**Returns:** `dynamic`

Returns a key-value table (a Lua table read by key, for example `result.name`); all values are strings (`StringFromQuery("a=1&b=2").a` is `"1"`, so `== 1` is false). `#result` is 0.

---

## StringIndexOf

```lua
StringIndexOf(value, searchValue, [ignoreCase])
```

Gets the zero-based index of the first occurrence of the specified string in the current.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `value` | `string` | Yes | The main string. |
| `searchValue` | `string` | Yes | The search value. |
| `ignoreCase` | `boolean` | No | Ignoring the case of the scripts being compared. |

**Returns:** `number`

Returns a zero-based index of the first occurrence of the specified string.

---

## StringJoin

```lua
StringJoin(value1, value2, [separator])
```

Concatenates 2 string with an optional separator.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `value1` | `dynamic` | Yes | The first part of the string. |
| `value2` | `dynamic` | Yes | The second part of the string. |
| `separator` | `string` | No | The string to use as a separator. |

**Returns:** `string`

Returns the concatenated string.

---

## SubString

```lua
SubString(value, start, length)
```

Retrieves a substring from this instance. The substring starts at a specified character position and has a specified length.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `value` | `string` | Yes | The main string. |
| `start` | `number` | Yes | Start index, zero based. |
| `length` | `number` | Yes | The length of the substring. |

**Returns:** `string`

Substring of the main value.

---


*Auto-generated from HaasScript source code.*
