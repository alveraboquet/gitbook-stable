# Stepgain

This page describes how margin trading works with the Stepgain strategy. The triggers for trades are slightly different than in the same strategy for regular trading.

## How to work with this strategy

{% hint style="info" %}
**Expected behavior for margin trading**

Gunbot will open one position, either long or short, and close this position when the target is reached. When the stop is hit before profitably closing a trade, Gunbot will place a stop order at loss. After closing a position, Gunbot will again look to open a new long or short position. Gunbot will not add to existing open positions.

Please don't manually add to or reduce positions opened by Gunbot, unless you stop running Gunbot on this trading pair until you've closed this position.
{% endhint %}

{% hint style="warning" %}
Using `stepgain` \(margin\) is only meaningful with `MEAN_REVERSION` enabled.

The info below assumes you have set this.
{% endhint %}

### Long / Buy

A long position is opened when price is between your stepgain buy levels and changes direction while being below `LONG_LEVEL`.

### Short / Sell

A short position is opened when price is between your stepgain sell levels and changes direction while being above `SHORT_LEVEL`.

### Close

A position is closed when the desired `ROE` is reached.

### Stop

A position is closed at loss when its stop is reached.

## Strategy parameters

Following settings options are available for `stepgain` and can be set in the strategy configurator of the GUI or the strategies section of the config.js file.

These settings are global and apply to all pairs running this strategy. When you want a specific parameter to be different for one or more pairs, use an [override](https://github.com/GuntharDeNiro/BTCT/wiki/Gunbot-settings#overrides) at the pair level.

Using the `BUY_METHOD` and `SELL_METHOD` parameters you can combine different methods for buying and selling. This strategy page assumes both `BUY_METHOD` and `SELL_METHOD` are set to `stepgain`. Accepted values are all strategy names as listed [here](https://github.com/GuntharDeNiro/BTCT/wiki/About-Gunbot-strategies).

## Margin settings

Margin settings control settings like leverage and the target for ROE. These parameters are relevant when using `stepgain` as buy and/or sell method.

### Long Level

{% tabs %}
{% tab title="Description" %}
This sets the target for opening a long position at a percentage above the highest EMA.

When you set this to 1, buy orders will only be placed when the current price is at least 1% above the currently highest EMA.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical ??? represent a percentage.

**Default value:** 1
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy buy | RT buy |
|  | RT buyback |
|  | RT sell |
|  | Close |
|  | Stop limit |
|  | Strategy sell |
|  | DCA buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `LONG_LEVEL`
{% endtab %}
{% endtabs %}

### Short Level

{% tabs %}
{% tab title="Description" %}
This sets the target for opening a short position at a percentage below the lowest EMA.

When you set this to 1, sell orders will only be placed when the current price is at least 1% below the currently lowest EMA.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical ??? represent a percentage.

**Default value:** 1
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy sell | RT buy |
|  | RT buyback |
|  | RT sell |
|  | Close |
|  | Stop limit |
|  | Strategy buy |
|  | DCA buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `SHORT_LEVEL`
{% endtab %}
{% endtabs %}

### ROE

{% tabs %}
{% tab title="Description" %}
This sets the target for closing a position.

ROE is the Return On Equity for a position, the percentage profit and loss on your invested margin. This value is calculated in a similar way to how Bitmex calculates it, it does include leverage and does not include fees.

**Examples:**

Long position, 1x leverage.  
When price moves 1% above the average entry price, 1% ROE is reached.

Long position, 100x leverage \(or cross leverage\).  
When price moves 1% above the average entry price, 100% ROE is reached.

Short position, 20x leverage  
When price moves 1% below the average entry price, 20% ROE is reached.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical ??? represent a percentage.

**Default value:** 1
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Close | RT buy |
|  | RT buyback |
|  | RT sell |
|  | Close |
|  | Stop limit |
|  | Strategy buy |
|  | Strategy sell |
|  | DCA buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `ROE`
{% endtab %}
{% endtabs %}

### PND

{% tabs %}
{% tab title="Description" %}
Use "PND" logic to close trades. This mode tries to not close a position before a pump or dump has fully played out - usually beats ROE trailing performance.

Respects the minimum ROE set.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Close | RT buy |
|  | RT buyback |
|  | RT sell |
|  | Close |
|  | Stop limit |
|  | Strategy buy |
|  | Strategy sell |
|  | DCA buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `PND`
{% endtab %}
{% endtabs %}

### PND protection

{% tabs %}
{% tab title="Description" %}
Threshold to close a position when it drops below ROE again.

A value of 1.5 means that if ROE reached 1.5x the minimum target, the position will get closed immediately if the trend turns.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical

**Default value:** 1.5
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Close | RT buy |
|  | RT buyback |
|  | RT sell |
|  | Close |
|  | Stop limit |
|  | Strategy buy |
|  | Strategy sell |
|  | DCA buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `PND_PROTECTION`
{% endtab %}
{% endtabs %}

### Leverage

{% tabs %}
{% tab title="Description" %}
Sets the leverage for opening any position. Setting 0 places the order with cross margin.

{% hint style="warning" %}
On Binance Futures you must set leverage per pair on the exchange itself.
{% endhint %}
{% endtab %}

{% tab title="Values" %}
**Values:** numerical

**Default value:** 0
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy buy | RT buy |
| Strategy sell | RT buyback |
|  | RT sell |
|  | Close |
|  | Stop limit |
|  | Close |
|  | DCA buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `LEVERAGE`
{% endtab %}
{% endtabs %}

### Stop Buy

{% tabs %}
{% tab title="Description" %}
Places a market stop order for a long position, at the same time as the position is opened.

When set to 1 and a long order is opened at a price of 100, a stop market order will be placed at 99.

{% hint style="info" %}
This setting is exclusive to Bitmex
{% endhint %}
{% endtab %}

{% tab title="Values" %}
**Values:** numerical - represents a percentage.

**Default value:** 0
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy buy | RT buy |
|  | RT buyback |
|  | RT sell |
|  | Close |
|  | Stop limit |
|  | Close |
|  | Strategy sell |
|  | DCA buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `STOP_BUY`
{% endtab %}
{% endtabs %}

### Stop Sell

{% tabs %}
{% tab title="Description" %}
Places a market stop order for a short position, at the same time as the position is opened.

When set to 1 and a short order is opened at a price of 100, a stop market order will be placed at 101.

{% hint style="info" %}
This setting is exclusive to Bitmex
{% endhint %}
{% endtab %}

{% tab title="Values" %}
**Values:** numerical - represents a percentage.

**Default value:** 0
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy sell | RT buy |
|  | RT buyback |
|  | RT sell |
|  | Close |
|  | Stop limit |
|  | Close |
|  | Strategy buy |
|  | DCA buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `STOP_SELL`
{% endtab %}
{% endtabs %}

### ROE Trailing

{% tabs %}
{% tab title="Description" %}
Use this to enable tssl-style trailing for ROE.

Trailing limit is set with `ROE_LIMIT`.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Close | RT buy |
|  | RT buyback |
|  | RT sell |
|  | Strategy sell |
|  | Stop limit |
|  | Close |
|  | Strategy buy |
|  | DCA buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `ROE_TRAILING`
{% endtab %}
{% endtabs %}

### ROE Scalper

{% tabs %}
{% tab title="Description" %}
Use this to enable an alternate trailing mechanism for closing positions.

Trailing limit is set with `ROE_LIMIT`. Additionally `ROE_TRAILING` must be enabled.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Close | RT buy |
|  | RT buyback |
|  | RT sell |
|  | Strategy sell |
|  | Stop limit |
|  | Close |
|  | Strategy buy |
|  | DCA buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `ROE_SCALPER`
{% endtab %}
{% endtabs %}

### ROE Limit

{% tabs %}
{% tab title="Description" %}
This sets the range for ROE trailing.

**ROE trailing:**

Range is a percentage of current ROE. Setting a `ROE_LIMIT` of 5 at a `ROE` target of 1 would set an initial range between 0.95 and 1.05.

**ROE scalper:**

Range is an absolute ROE value. Setting a ROE\_LIMIT of 5 at a `ROE` target of 10 means that the trailing stop is initially set at ROE 5 \(`ROE` minus `ROE_LIMIT`\).

**Both**:

As long as ROE keeps increasing, the range moves along with ROE. As soon as ROE start decreasing, the lower range gets frozen. A close order is placed when ROE crosses the lower limit.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical ??? represent a trailing range.

**Default value:** 1
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Close | RT buy |
|  | RT buyback |
|  | RT sell |
|  | Strategy sell |
|  | Stop limit |
|  | Close |
|  | Strategy buy |
|  | DCA buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `ROE_LIMIT`
{% endtab %}
{% endtabs %}

### Pre Order

{% tabs %}
{% tab title="Description" %}
When set to true, limit orders will placed at a configurable rate beyond the best bid/ask price to get ahead of the order book.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Close | RT buy |
| Strategy sell | RT buyback |
| Strategy buy | RT sell |
|  | Stop limit |
|  | DCA buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `PRE_ORDER`
{% endtab %}
{% endtabs %}

### Pre Order Gap

{% tabs %}
{% tab title="Description" %}
Sets the gap between the best bid/ask price in the orderbook and the rate at which a limit order gets placed. Long orders are placed at ask + gap. Short orders are placed at bid - gap.

It is possible to use negative values, this will increase the chance of receiving maker fees.

Example when set to 1 and a buy signal occurs at an ask price of 100: a limit order gets placed at a rate of 101. When set to -1 and a buy signal occurs at an ask price of 100: a limit order gets placed at a rate of 99.

Don't use a negative gap together with `STOP_BUY` and/or `STOP_SELL`, as these stops do not combine well with position that do not always fill. Instead use `STOP_LIMIT`.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical - represents a percentage.

**Default value:** 0
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy sell | RT buy |
| Strategy buy | RT buyback |
|  | RT sell |
|  | Stop limit |
|  | DCA buy |
|  | Close |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `PRE_ORDER_GAP`
{% endtab %}
{% endtabs %}

### Mean Reversion

{% tabs %}
{% tab title="Description" %}
When set to true, the strategy follows a mean reversion way of trading, instead of trend following.

This setting must be enabled to use stepgain as buy or sell method. When set to false, this strategy reverts to the same behavior as the gain strategy.

Long and short levels are reversed in this mode, long level is placed below EMA, short level is placed above EMA.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy sell | RT buy |
| Strategy buy | RT buyback |
|  | RT sell |
|  | Stop limit |
|  | DCA buy |
|  | Close |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `MEAN_REVERSION`
{% endtab %}
{% endtabs %}

## Buy settings

Buy settings are the primary trigger for opening long positions. These parameters control the execution of buy orders when using `stepgain` as buy method.

### Buy enabled

{% tabs %}
{% tab title="Description" %}
Set this to false to prevent Gunbot from placing buy orders.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** true
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy buy | Strategy sell |
| DCA buy | Stop limit |
| RT buy | Close |
| RT buyback | RT sell |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `BUY_ENABLED`
{% endtab %}
{% endtabs %}

### Stepgain Buy Lvl

{% tabs %}
{% tab title="Description" %}
This sets which step should be considered for buying:

1: Buy when price drops below `BUYLVL1` and the trend reverses or price hits `BUYLVL2`.

2: Buy when price drops below `BUYLVL2` and the trend reverses or price hits `BUYLVL3`.

3: Buy when price drops below `BUYLVL3` and the trend reverses.
{% endtab %}

{% tab title="Values" %}
**Values:** 1 / 2 / 3 ??? represents steps.

**Default value:** 1
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy buy | Strategy sell |
|  | Stop limit |
|  | Close |
|  | RT sell |
|  | DCA buy |
|  | RT buy |
|  | RT buyback |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `BUYLVL`
{% endtab %}
{% endtabs %}

### Stepgain Buy Lvl 1

{% tabs %}
{% tab title="Description" %}
Defines the first level below the lowest EMA to be considered for buying. Only used when `BUYLVL` is set to 1.

When set to 1, this means that the price needs to be at least 1 percent below the lowest EMA.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical ??? represents a percentage.

**Default value:** 0.6
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy buy | Strategy sell |
|  | Stop limit |
|  | Close |
|  | RT sell |
|  | DCA buy |
|  | RT buy |
|  | RT buyback |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `BUYLVL1`
{% endtab %}
{% endtabs %}

### Stepgain Buy Lvl 2

{% tabs %}
{% tab title="Description" %}
Defines the second level below the lowest EMA to be considered for buying. Used when `BUYLVL` is set to 1 or 2.

When set to 2, this means that the price needs to be at least 2 percent below the lowest EMA.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical ??? represents a percentage.

**Default value:** 2
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy buy | Strategy sell |
|  | Stop limit |
|  | Close |
|  | RT sell |
|  | DCA buy |
|  | RT buy |
|  | RT buyback |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `BUYLVL2`
{% endtab %}
{% endtabs %}

### Stepgain Buy Lvl 3

{% tabs %}
{% tab title="Description" %}
Defines the third level below the lowest EMA to be considered for buying. Used when `BUYLVL` is set to 2 or 3.

When set to 10, this means that the price needs to be at least 10 percent below the lowest EMA.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical ??? represents a percentage.

**Default value:** 70
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy buy | Strategy sell |
|  | Stop limit |
|  | Close |
|  | RT sell |
|  | DCA buy |
|  | RT buy |
|  | RT buyback |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `BUYLVL3`
{% endtab %}
{% endtabs %}

### NBA

{% tabs %}
{% tab title="Description" %}
"Never Buy Above". Use this to only allow buy orders below the last sell rate.

This sets the minimum percentage difference between the last sell order and the next buy. The default setting of 0 disables this option.

When set to 1, Gunbot will only place a buy order when the strategy buy criteria meet and price is at least 1% below the last sell price.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical, represents a percentage.

**Default value:** 0
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy buy | Strategy sell |
|  | Stop limit |
|  | Close |
|  | RT sell |
|  | DCA buy |
|  | RT buy |
|  | RT buyback |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `NBA`
{% endtab %}
{% endtabs %}

## Sell settings

Sell settings are the primary trigger for opening short positions. These parameters control the execution of sell orders when using `stepgain` as sell method.

### Sell enabled

{% tabs %}
{% tab title="Description" %}
Set this to false to prevent Gunbot from placing sell orders.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** true
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy sell | Strategy buy |
| Stop limit | RT buy |
| RT sell | RT buyback |
|  | Close |
|  | DCA buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `SELL_ENABLED`
{% endtab %}
{% endtabs %}

### Stepgain Sell Lvl

{% tabs %}
{% tab title="Description" %}
This sets which step should be considered for selling:

1: Sell when price rises above `SELLLVL1` and the trend reverses or price hits `SELLLVL2`.

2: Sell when price rises above `SELLLVL2` and the trend reverses or price hits `SELLLVL3`.

3: Sell when price rises above `SELLLVL3` and the trend reverses.
{% endtab %}

{% tab title="Values" %}
**Values:** 1 / 2 / 3 ??? represents steps.

**Default value:** 1
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy sell | Strategy buy |
|  | RT buy |
|  | RT buyback |
|  | RT sell |
|  | Close |
|  | DCA buy |
|  | Stop limit |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `SELLLVL`
{% endtab %}
{% endtabs %}

### Stepgain Sell Lvl 1

{% tabs %}
{% tab title="Description" %}
Defines the first level above the break-even point to be considered for selling. Only used when `SELLLVL` is set to 1.

When set to 1, this means that the price needs to be at least 1 percent above the average bought price.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical ??? represents a percentage.

**Default value:** 0.6
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy sell | Strategy buy |
|  | RT buy |
|  | RT buyback |
|  | RT sell |
|  | Close |
|  | DCA buy |
|  | Stop limit |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `SELLLVL1`
{% endtab %}
{% endtabs %}

### Stepgain Sell Lvl 2

{% tabs %}
{% tab title="Description" %}
Defines the second level above the break-even point to be considered for selling. Used when `SELLLVL` is set to 1 or 2.

When set to 2, this means that the price needs to be at least 2 percent above the average bought price.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical ??? represents a percentage.

**Default value:** 2
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy sell | Strategy buy |
|  | RT buy |
|  | RT buyback |
|  | RT sell |
|  | Close |
|  | DCA buy |
|  | Stop limit |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `SELLLVL2`
{% endtab %}
{% endtabs %}

### Stepgain Sell Lvl 3

{% tabs %}
{% tab title="Description" %}
Defines the third level above the break-even point to be considered for selling. Used when `SELLLVL` is set to 2 or 3.

When set to 10, this means that the price needs to be at least 10 percent above the average bought price.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical ??? represents a percentage.

**Default value:** 70
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy sell | Strategy buy |
|  | RT buy |
|  | RT buyback |
|  | RT sell |
|  | Close |
|  | DCA buy |
|  | Stop limit |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `SELLLVL3`
{% endtab %}
{% endtabs %}

## Indicator settings

Relevant indicators for trading with stepgain.

These settings have a direct effect on trading with `stepgain`, this is where you configure the EMAs.

### Period

{% tabs %}
{% tab title="Description" %}
This sets the candlestick period used for trading, this affects all indicators within the strategy.

Only use [supported values](../../how-to-work-with-gunbot/basic-workings/period.md#supported-period-values).

Setting a short period allows you to trade on shorter trends, but be aware that these will be noisier than longer periods.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical??? represents candlestick size in minutes.

**Default value:** 15
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy sell | RT buy |
| Strategy buy | RT buyback |
|  | RT sell |
|  | Close |
|  | Stop limit |
|  | DCA buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `PERIOD`
{% endtab %}
{% endtabs %}

### Slow EMA

{% tabs %}
{% tab title="Description" %}
Set this to the amount of candlesticks you want to use for your slow EMA. The closing price for each candle is used in the slow EMA calculation.

For example: when you set `PERIOD` to 5, and want to use 2h for slow EMA ??? you need to set `EMA1` to 24 \(24 \* 5 mins\).
{% endtab %}

{% tab title="Values" %}
**Values:** numerical ??? represents a number of candlesticks.

**Default value:** 16
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy buy | RT buy |
| Strategy sell | RT buyback |
|  | RT sell |
|  | Close |
|  | Stop limit |
|  | DCA buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `EMA1`
{% endtab %}
{% endtabs %}

### Medium EMA

{% tabs %}
{% tab title="Description" %}
Set this to the amount of candlesticks you want to use for your medium EMA. The closing price for each candle is used in the fast EMA calculation.

For example: when you set `PERIOD` to 5, and want to use 1h for medium EMA ??? you need to set `EMA2` to 12 \(12 \* 5 mins\).
{% endtab %}

{% tab title="Values" %}
**Values:** numerical ??? represents a number of candlesticks.

**Default value:** 8
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy buy | RT buy |
|  | RT buyback |
|  | RT sell |
|  | Close |
|  | Stop limit |
|  | Strategy sell |
|  | DCA buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `EMA2`
{% endtab %}
{% endtabs %}

## Balance settings

{% page-ref page="../balance-settings.md" %}

## **Confirming indicators + advanced indicator settings**

{% page-ref page="../confirming-indicators.md" %}

## **Misc settings**

{% page-ref page="../misc-settings.md" %}

## Dollar cost avg settings

### DCA Method

{% tabs %}
{% tab title="Description" %}
When set to **TENKAN** Gunbot will place DCA orders in exactly the same way as in the [Tenkan ](tenkan.md)strategy. Each DCA order is in size of `TRADING_LIMIT`.
{% endtab %}

{% tab title="Values" %}
**Values:** NATIVE / TENKAN

**Default value:** TENKAN   
  
Setting NATIVE effectively disables DCA in this strategy
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| DCA  | Strategy buy |
|  | Strategy sell |
|  | Close |
|  | RT sell |
|  | Stop limit |
|  | RT buyback |
|  | RT buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `DCA_METHOD`
{% endtab %}
{% endtabs %}

## Reversal trading settings

RT is not intented to be used for margin trading.

## TrailMe settings

TrailMe is not intented to be used for margin trading.

{% page-ref page="../trailme.md" %}

## Placeholders

The following parameters in `config.js` have no function for this strategy and act as placeholder.

| Parameter | Description |
| :--- | :--- |
| `ATRX` | Placeholder. |
| `ATR_PERIOD` | Placeholder. |
| `BUY_LEVEL` | Placeholder. |
| `BUY_LEVEL` | Placeholder. |
| `BUY_RANGE` | Placeholder. |
| `DISPLACEMENT` | Placeholder. |
| `DOUBLE_CHECK_GAIN` | Placeholder. |
| `FAST_SMA` | Placeholder. |
| `GAIN` | Placeholder. |
| `GAIN` | Placeholder. |
| `HIGH_BB` | Placeholder. |
| `ICHIMOKU_PROTECTION` | Placeholder. |
| `KIJUN_BUY` | Placeholder. |
| `KIJUN_CLOSE` | Placeholder. |
| `KIJUN_PERIOD` | Placeholder. |
| `KIJUN_SELL` | Placeholder. |
| `KIJUN_STOP` | Placeholder. |
| `KUMO_BUY` | Placeholder. |
| `KUMO_CLOSE` | Placeholder. |
| `KUMO_SELL` | Placeholder. |
| `KUMO_SENTIMENTS` | Placeholder. |
| `KUMO_STOP` | Placeholder. |
| `LOW_BB` | Placeholder. |
| `MACD_LONG` | Placeholder. |
| `MACD_SHORT` | Placeholder. |
| `MACD_SIGNAL` | Placeholder. |
| `PP_BUY` | Placeholder. |
| `PP_SELL` | Placeholder. |
| `RENKO_ATR` | Placeholder. |
| `RENKO_BRICK_SIZE` | Placeholder. |
| `RENKO_PERIOD` | Placeholder. |
| `RT_BUY_LEVEL` | Placeholder. |
| `SELL_RANGE` | Placeholder. |
| `SENKOUSPAN_PERIOD` | Placeholder. |
| `SLOW_SMA` | Placeholder. |
| `TAKE_BUY` | Placeholder. |
| `TBUY_RANGE` | Placeholder. |
| `TENKAN_BUY` | Placeholder. |
| `TENKAN_CLOSE` | Placeholder. |
| `TENKAN_PERIOD` | Placeholder. |
| `TENKAN_SELL` | Placeholder. |
| `TENKAN_STOP` | Placeholder. |
| `TP_PROFIT_ONLY` | Placeholder. |
| `TP_RANGE` | Placeholder. |
| `TSSL_TARGET_ONLY` | Placeholder. |
| `USE_RENKO` | Placeholder. |

