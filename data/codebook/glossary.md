# Variable Glossary — Plain Language Reference

> All variables used in this project, in alphabetical order.
> Written for readers without an economics or data science background.
> For technical details (sources, frequencies, transformations), see `variable-catalogue.md`.

---

## How to read this glossary

Each entry follows this format:

| Symbol | Full Name | Plain-language explanation |
|---|---|---|

**Tip:** Variables that end in `_lag1`, `_lag3`, `_lag6`, `_lag12` are simply *delayed versions* of the base variable (see the [Derived Features](#derived-features-patterns) section at the bottom).

---

## Base Variables — A to Z

| Symbol | Full Name | What it means in plain language |
|---|---|---|
| `CPI_BRA` | Consumer Price Index — Brazil | Measures how much prices of everyday goods (food, housing, transport) have changed in Brazil over time. When this number goes up, money buys less — that's inflation. |
| `CPI_CAN` | Consumer Price Index — Canada | Same as above, but for Canada. Tracks the cost of a typical "basket" of goods and services bought by Canadian households. |
| `CPI_MEX` | Consumer Price Index — Mexico | Same concept, for Mexico. Reflects how fast or slow prices are rising in the Mexican economy. |
| `CPI_USA` | Consumer Price Index — United States | The standard US inflation measure. Published monthly by the Bureau of Labor Statistics. When the Fed wants to control inflation, this is the number it watches. |
| `dummy_covid` | COVID-19 Crisis Dummy | A flag that equals **1** during the COVID-19 disruption period (March 2020 – June 2021) and **0** at all other times. It tells the model: "these months were extraordinary — treat them differently." |
| `dummy_gfc` | Global Financial Crisis Dummy | A flag that equals **1** during the Global Financial Crisis (September 2008 – June 2009) and **0** otherwise. Trade collapsed by ~25% in this period — the model needs to know that. |
| `exports_commodities` | US Exports — Commodities Sector | The estimated value (in US dollars) of raw materials and natural resources the US exported to the partner country each month. Includes agricultural products, minerals, oil, metals, and timber (HS chapters 1–15, 25–27, 41–47, 71–72). |
| `exports_high-tech` | US Exports — High-Tech Sector | The estimated value of high-technology goods the US exported each month. Includes computers, electronics, aircraft, medical equipment, precision instruments, and pharmaceuticals (HS chapters 30, 84, 85, 88, 90, 91). |
| `exports_manufactured_goods` | US Exports — Manufactured Goods Sector | The estimated value of manufactured products that don't fit into the Commodities or High-Tech categories — chemicals, plastics, clothing, vehicles, furniture, and similar goods. |
| `exports_total` | US Exports — Total | The total value of everything the US exported to the partner country that month, across all product categories. |
| `FEDFUNDS` | Federal Funds Rate | The interest rate at which US banks lend money to each other overnight. This is the main tool the US Federal Reserve uses to steer the economy — raising it slows growth and inflation; cutting it stimulates borrowing and spending. |
| `FX_USD_BRL` | USD/BRL Exchange Rate | How many Brazilian reais (BRL) you need to buy one US dollar. Example: if `FX_USD_BRL = 5.10`, then 1 USD costs R$ 5.10. When this number rises, the dollar got stronger (or the real got weaker). |
| `FX_USD_CAD` | USD/CAD Exchange Rate | How many Canadian dollars (CAD) you need to buy one US dollar. Example: if `FX_USD_CAD = 1.36`, then 1 USD costs C$ 1.36. |
| `FX_USD_MXN` | USD/MXN Exchange Rate | How many Mexican pesos (MXN) you need to buy one US dollar. Example: if `FX_USD_MXN = 17.50`, then 1 USD costs MX$ 17.50. |
| `GDP_BRA` | Real Gross Domestic Product — Brazil | The total economic output of Brazil in a quarter, adjusted for inflation (so it reflects actual growth, not just price increases). A proxy for how healthy the Brazilian economy is. |
| `GDP_CAN` | Real Gross Domestic Product — Canada | Same concept for Canada. Measures the total value of goods and services produced in Canada per quarter. |
| `GDP_MEX` | Real Gross Domestic Product — Mexico | Same concept for Mexico. Quarterly measure of Mexico's total economic output. |
| `GDP_USA` | Real Gross Domestic Product — United States | Total US economic output per quarter, inflation-adjusted. The broadest single measure of how the US economy is performing. |
| `imports_commodities` | US Imports — Commodities Sector | Estimated value of raw materials and natural resources the US *imported from* the partner country each month. |
| `imports_high-tech` | US Imports — High-Tech Sector | Estimated value of high-technology goods the US imported from the partner country each month. |
| `imports_manufactured_goods` | US Imports — Manufactured Goods Sector | Estimated value of manufactured goods the US imported from the partner country each month. |
| `imports_total` | US Imports — Total | Total value of everything the US imported from the partner country that month. |
| `INDPRO_BRA` | Industrial Production Index — Brazil | Measures how much industrial output Brazil produced relative to a reference year (2015 = 100). Captures factories, construction, and energy production. When this drops, Brazil's industry is slowing down. |
| `INDPRO_CAN` | Industrial Production Index — Canada | Same concept for Canada. Tracks the overall output of Canadian industry each month. |
| `INDPRO_MEX` | Industrial Production Index — Mexico | Same concept for Mexico. Specifically the Total Industry Including Construction index (2015 = 100). |
| `INDPRO_USA` | Industrial Production Index — United States | Monthly measure of US factory, mining, and utility output. A good real-time indicator of US economic activity (GDP comes out quarterly, this comes out monthly). |
| `Iron_ore` | Iron Ore Price | The world market price of iron ore (USD per dry metric ton), published monthly by the IMF. Iron ore is Brazil's largest export commodity — when prices rise, Brazil earns more from exports and the real may strengthen. |
| `month` | Calendar Month | The month number (1 = January, 12 = December). Included so the model can learn seasonal patterns — for example, US imports typically spike in September–October as retailers stock up for the holiday season. |
| `quarter` | Calendar Quarter | Which quarter of the year (1 = Jan–Mar, 2 = Apr–Jun, 3 = Jul–Sep, 4 = Oct–Dec). Captures broader seasonal rhythms in trade. |
| `RATE_BRA` | Central Bank Interest Rate — Brazil | The Selic rate — the benchmark overnight interest rate set by Brazil's central bank (Banco Central do Brasil). High rates attract foreign capital and can strengthen the real; low rates can weaken it. |
| `RATE_CAN` | Central Bank Interest Rate — Canada | The overnight rate set by the Bank of Canada. Works the same way as the Fed Funds Rate — it's the main lever for Canadian monetary policy. |
| `RATE_MEX_3M` | 3-Month Interbank Rate — Mexico | The interest rate at which Mexican banks lend to each other for 3-month periods. Used as a proxy for Mexico's monetary policy stance (the direct central bank rate is not available in FRED for Mexico). |
| `REER_BRA` | Real Effective Exchange Rate — Brazil | A single number that summarizes how the Brazilian real compares against all its major trading partners at once, adjusted for inflation differences. Higher = the real is relatively stronger (Brazilian exports become more expensive). |
| `REER_CAN` | Real Effective Exchange Rate — Canada | Same concept for Canada. Captures how competitive Canadian goods are in international markets. |
| `REER_MEX` | Real Effective Exchange Rate — Mexico | Same concept for Mexico. A rise means Mexican products became more expensive for foreign buyers. |
| `REER_USA` | Real Effective Exchange Rate — United States | How the US dollar performs against a basket of all major trading partners, adjusted for inflation. A stronger REER_USA generally makes US exports more expensive and imports cheaper — tending to widen the trade deficit. |
| `Soybean` | Soybean Price | The world market price of soybeans (USD per metric ton), monthly. Highly relevant for Brazil (one of the world's largest soybean exporters) and for US agricultural exports. |
| `UNRATE_USA` | Unemployment Rate — United States | The percentage of the US labor force that is actively looking for work but doesn't have a job. A low unemployment rate signals a strong economy; a high rate signals weakness. |
| `WTI_oil` | WTI Crude Oil Price | The price of West Texas Intermediate crude oil (USD per barrel) — the main US oil price benchmark. Affects production costs across almost every sector of the economy and directly drives Commodities trade flows. |
| `year` | Calendar Year | The year of the observation (2010–2024). Helps the model recognize long-term structural trends that can't be explained by the other variables alone. |

---

## Derived Features — Patterns

Rather than listing every lag and moving average individually (that would be 57 extra rows), this section explains the naming patterns. Once you know what `FX_USD_BRL` means, the variants below follow automatically.

---

### `{variable}_lag1`, `_lag3`, `_lag6`, `_lag12`

**What it is:** The value of the variable from 1, 3, 6, or 12 months in the past.

**Why it exists:** Trade doesn't react instantly to a currency shock. If the Brazilian real suddenly depreciates, Brazilian exporters need time to renegotiate contracts, adjust production, and find new buyers. Research shows the full effect on trade volumes takes 6–12 months to materialize — the so-called *J-curve effect*. By giving the model the exchange rate from several months ago alongside today's trade flow, it can learn these delayed relationships.

**Example:** `FX_USD_BRL_lag6` in June 2020 = the value of `FX_USD_BRL` from December 2019.

Applied to: `FX_USD_CAD`, `FX_USD_MXN`, `FX_USD_BRL`, `REER_USA`, `REER_CAN`, `REER_MEX`, `REER_BRA`

---

### `{variable}_ma3`, `_ma6`, `_ma12`

**What it is:** The average of the variable over the past 3, 6, or 12 months (rolling mean).

**Why it exists:** Day-to-day (or month-to-month) exchange rate fluctuations are noisy. A trader doesn't change their pricing strategy because of a one-week blip. The moving average smooths out that noise and reveals the *underlying trend* — which is what actually drives decisions. If `FX_USD_BRL_ma6` is rising, the dollar has been strengthening against the real for half a year, which is a signal businesses take seriously.

**Example:** `FX_USD_CAD_ma12` in December 2020 = the average of `FX_USD_CAD` from January through December 2020.

Applied to: `FX_USD_CAD`, `FX_USD_MXN`, `FX_USD_BRL`

---

### `{variable}_pct`

**What it is:** The month-over-month percentage change: how much the variable moved relative to last month.

**Why it exists:** The *level* of the exchange rate (e.g., 5.10 BRL/USD) reflects years of accumulated history. The *change* (e.g., +2.3% this month) is the new information — the shock that might cause a buyer to revise an order or a seller to adjust prices. Percentage changes also make different exchange rates comparable on the same scale.

**Example:** `FX_USD_BRL_pct` in March 2020 ≈ +20% (the real lost ~20% of its value in a single month as COVID hit).

Applied to: `FX_USD_CAD`, `FX_USD_MXN`, `FX_USD_BRL`, `REER_USA`

---

### `log_{target}` (e.g., `log_exports_total`)

**What it is:** The natural logarithm of a trade flow value: `log(value + 1)`.

**Why it exists:** Trade values are in the billions of dollars and are heavily skewed — a few extraordinary months (like post-COVID rebound) are much larger than typical months. The logarithm compresses the scale so large values don't dominate the model. It also means the model works with *percentage changes* rather than absolute dollar amounts, which is more economically meaningful. "Exports grew 5%" is more comparable across countries than "exports grew $3 billion."

Applied to: all 8 trade flow target columns

---

### `diff_log_{target}` (e.g., `diff_log_exports_total`)

**What it is:** The month-over-month change of the log value: `log_value[month] − log_value[month − 1]`. This is approximately the monthly growth rate in percentage terms.

**Why it exists:** The ARIMA model requires *stationary* series — series that don't have a systematic upward or downward trend over time. US trade flows have grown substantially from 2010 to 2024, which violates this requirement. Taking the first difference removes the trend, leaving just the monthly fluctuation — which is stationary. Think of it as the difference between "the total exports" (which grows every year) and "did exports go up or down this month compared to last month?" (which bounces around a stable average).

Applied to: all 8 log-transformed trade flow columns. **Note:** The first row of each series is always NaN (there is no "previous month" for January 2010).

---

## Excluded Variables

These were collected but not used in modeling:

| Symbol | Full Name | Why excluded |
|---|---|---|
| `DTWEXBGS` | Trade Weighted US Dollar Index | Measures the same thing as `REER_USA` — both track the dollar against a basket of currencies. Using both would confuse the model (multicollinearity). |
| `GDP_per_capita_USD` | GDP per capita (World Bank) | Only one data point per year. Converting annual data to monthly by interpolation would create artificial values with no real information. The same economic reality is already captured by the monthly/quarterly GDP from FRED. |
| `GDP_total_USD` | Total GDP (World Bank) | Same reason as above. |
| `Population` | Population total (World Bank) | Grows by roughly the same amount every year. Not useful for predicting monthly trade fluctuations. |
| `Inflation_pct` | Annual inflation % (World Bank) | Annual frequency + overlaps with the monthly CPI already included. |
| `Current_account_USD` | Current account balance (World Bank) | Annual frequency only. Would require artificial interpolation. |
| `FDI_inflows_USD` | Foreign direct investment inflows (World Bank) | Annual frequency only. Would require artificial interpolation. |

---

*Last updated: 2026-03-29*
