# Variable Catalogue — US Trade Flow Forecasting

> Complete documentation of all variables used in this project.
> Organized by data source (raw) and by transformation type (processed).
> Project: Forecasting the Effects of Exchange Rate Fluctuations on US Trade Flows
> Author: Francisco Giordano Rigon — UNISINOS

---

## RAW DATA

Variables as collected from their original sources, before any transformation.

---

### FRED — Federal Reserve Economic Data
> API: `api.stlouisfed.org` | Library: `fredapi` | File prefix: `fred_`

#### Exchange Rates — `fred_exchange_rates.csv`
| Symbol | Full Name | Frequency | Period | Treatment in Preparation |
|---|---|---|---|---|
| `DEXCAUS` | USD/CAD Spot Exchange Rate | Daily | 2000–2026 | Monthly mean → renamed `FX_USD_CAD` |
| `DEXMXUS` | USD/MXN Spot Exchange Rate | Daily | 2000–2026 | Monthly mean → renamed `FX_USD_MXN` |
| `DEXBZUS` | USD/BRL Spot Exchange Rate | Daily | 2000–2026 | Monthly mean → renamed `FX_USD_BRL` |

Missing values: ~268 NaN (weekends and US federal holidays — FRED reports business days only). Handled by monthly mean aggregation.

#### US Macroeconomic Indicators — `fred_macro.csv`
| Symbol | Full Name | Frequency | Period | Treatment in Preparation |
|---|---|---|---|---|
| `FEDFUNDS` | Federal Funds Effective Rate | Monthly | 2000–2026 | Kept as-is → `FEDFUNDS` |
| `CPIAUCSL` | Consumer Price Index for All Urban Consumers | Monthly | 2000–2026 | Kept as-is → `CPI_USA` |
| `GDPC1` | Real Gross Domestic Product (USA) | **Quarterly** | 2000–2026 | Forward-fill to monthly → `GDP_USA` |
| `UNRATE` | Unemployment Rate | Monthly | 2000–2026 | Kept as-is → `UNRATE_USA` |
| `INDPRO` | Industrial Production Index | Monthly | 2000–2026 | Kept as-is → `INDPRO_USA` |

#### Trade Indices — `fred_trade_indices.csv`
| Symbol | Full Name | Frequency | Period | Treatment in Preparation |
|---|---|---|---|---|
| `DTWEXBGS` | Trade Weighted US Dollar Index (Broad) | Daily | 2000–2026 | **EXCLUDED** — high multicollinearity with `REER_USA` |

#### Partner Country Interest Rates — `fred_rates_partners.csv`
| Symbol | Full Name | Frequency | Period | Treatment in Preparation |
|---|---|---|---|---|
| `RATE_CAN` | Bank of Canada Overnight Rate (`IRSTCB01CAM156N`) | Monthly | 2000–2026 | Kept as-is |
| `RATE_MEX_3M` | Mexico 3-Month Interbank Rate (`IR3TIB01MXM156N`) | Monthly | 2000–2026 | Kept as-is — proxy for monetary policy stance (standard substitute; OECD central bank series not available in FRED for Mexico) |
| `RATE_BRA` | Brazil Central Bank Rate (`IRSTCB01BRM156N`) | Monthly | 2000–2026 | Kept as-is |

#### Partner Country CPI — `fred_cpi_partners.csv`
| Symbol | Full Name | Frequency | Period | Treatment in Preparation |
|---|---|---|---|---|
| `CPI_CAN` | CPI — Canada (`CANCPIALLMINMEI`) | Monthly | 2000–2025 | Kept as-is |
| `CPI_MEX` | CPI — Mexico (`MEXCPIALLMINMEI`) | Monthly | 2000–2025 | Kept as-is |
| `CPI_BRA` | CPI — Brazil (`BRACPIALLMINMEI`) | Monthly | 2000–2025 | Kept as-is |

#### Partner Country GDP — `fred_gdp_partners.csv`
| Symbol | Full Name | Frequency | Period | Treatment in Preparation |
|---|---|---|---|---|
| `GDP_CAN` | Real GDP — Canada (`CANGDPNQDSMEI`) | **Quarterly** | 2000–2025 | Forward-fill to monthly |
| `GDP_MEX` | Real GDP — Mexico (`MEXGDPNQDSMEI`) | **Quarterly** | 2000–2025 | Forward-fill to monthly |
| `GDP_BRA` | Real GDP — Brazil (`NGDPRSAXDCBRQ`) | **Quarterly** | 2000–2025 | Forward-fill to monthly |

#### Real Effective Exchange Rates (REER) — `fred_reer.csv`
> Source: Bank for International Settlements (BIS) via FRED

| Symbol | Full Name | Frequency | Period | Treatment in Preparation |
|---|---|---|---|---|
| `REER_USA` | Real Effective Exchange Rate — USA (`RBUSBIS`) | Monthly | 2000–2026 | Kept as-is |
| `REER_CAN` | Real Effective Exchange Rate — Canada (`RBCABIS`) | Monthly | 2000–2026 | Kept as-is |
| `REER_MEX` | Real Effective Exchange Rate — Mexico (`RBMXBIS`) | Monthly | 2000–2026 | Kept as-is |
| `REER_BRA` | Real Effective Exchange Rate — Brazil (`RBBRBIS`) | Monthly | 2000–2026 | Kept as-is |

#### Partner Country Industrial Production — `fred_indpro_partners.csv`
> Source: OECD Main Economic Indicators via FRED

| Symbol | Full Name | Frequency | Period | Treatment in Preparation |
|---|---|---|---|---|
| `Canada` | Industrial Production Index — Canada (`CANPROINDMISMEI`) | Monthly | 2000–2025 | Renamed → `INDPRO_CAN` |
| `Mexico` | Total Industry incl. Construction — Mexico (`MEXPRINTO02IXOBSAM`) | Monthly | 2000–2025 | Renamed → `INDPRO_MEX` — standard OECD series (`MEXPROINDMISMEI`) not available in FRED |
| `Brazil` | Industrial Production Index — Brazil (`BRAPROINDMISMEI`) | Monthly | 2000–2025 | Renamed → `INDPRO_BRA` |

#### Commodity Prices — `fred_commodities.csv`
| Symbol | Full Name | Frequency | Period | Treatment in Preparation |
|---|---|---|---|---|
| `DCOILWTICO` | WTI Crude Oil Price | Daily | 2000–2026 | Monthly mean → renamed `WTI_oil` |
| `PSOYBUSDM` | Soybean Price (USD/metric ton) | Monthly | 2000–2026 | Kept as-is → renamed `Soybean` |
| `PIORECRUSDM` | Iron Ore Price (USD/dry metric ton) | Monthly | 2000–2026 | Kept as-is → renamed `Iron_ore` |

---

### UN Comtrade Plus
> API: `comtradeplus.un.org` | Library: `comtradeapicall` | Reporter: USA (code 842)

#### Monthly Total Trade Flows — `comtrade_monthly_total.csv`
| Column | Description | Treatment in Preparation |
|---|---|---|
| `period` | Reference period (YYYYMM format) | Converted to `datetime` |
| `partnerISO` | Partner country ISO code (CAN / MEX / BRA) | Used as group key |
| `flowCode` | Trade direction: X = Export, M = Import | Used as group key |
| `primaryValue` | Trade value in current USD | Target variable — split by direction; sectoral proportion applied |

Period: 2010-01 to 2024-12 only (monthly HS2 data unavailable before 2010 in Comtrade Plus API).

#### Annual Trade Flows by HS2 Sector — `comtrade_annual_hs2.csv`
| Column | Description | Treatment in Preparation |
|---|---|---|
| `period` | Reference year (YYYY) | Used to compute annual proportions |
| `partnerISO` | Partner country ISO code | Used as group key |
| `flowCode` | Trade direction: X or M | Used as group key |
| `cmdCode` | HS2 chapter code (1–97) | Classified into 3 sectors (see HS2 classification below) |
| `primaryValue` | Trade value in current USD | Used to compute sector share per year |

**HS2 Sectoral Classification** (aligned with TCC1, Section 2.3.2):

| Sector | HS2 Chapters |
|---|---|
| Commodities | 1–15, 25, 26, 27, 41, 43, 44, 45, 46, 47, 71, 72 |
| High-Tech | 30, 84, 85, 88, 90, 91 |
| Manufactured Goods | All remaining chapters |

#### Annual Total Trade Flows — `comtrade_annual_total.csv`
Aggregate annual bilateral trade flows, 2000–2024. Used for historical trend analysis and sectoral decomposition visualizations in EDA. Not used directly in modeling.

---

### World Bank
> API: `api.worldbank.org` | Library: `wbdata` | File: `worldbank_indicators.csv`

| Indicator ID | Full Name | Frequency | Treatment in Preparation |
|---|---|---|---|
| `NY.GDP.PCAP.CD` | GDP per capita (current USD) | Annual | **EXCLUDED** — annual frequency + overlaps with FRED GDP |
| `NY.GDP.MKTP.CD` | GDP total (current USD) | Annual | **EXCLUDED** — annual frequency + overlaps with FRED GDP |
| `SP.POP.TOTL` | Population total | Annual | **EXCLUDED** — changes too slowly; not informative at monthly frequency |
| `FP.CPI.TOTL.ZG` | Inflation, CPI (annual %) | Annual | **EXCLUDED** — annual frequency + overlaps with monthly CPI from FRED |
| `BN.CAB.XOKA.CD` | Current account balance (USD) | Annual | **EXCLUDED** — annual frequency; artificial interpolation to monthly not justified |
| `BX.KLT.DINV.CD.WD` | FDI inflows (USD) | Annual | **EXCLUDED** — annual frequency; artificial interpolation to monthly not justified |

**Reason for full exclusion of World Bank data:** All indicators are published annually. Converting annual data to monthly via interpolation would create 11 artificial observations per year, introducing noise without real information. Equivalent variables with higher frequency are already available from FRED.

---

## PROCESSED DATA

Variables in the final modeling datasets (`data/processed/dataset_{can|mex|bra}.csv`).
Each dataset: 180 rows (2010-01 to 2024-12) × 97 columns.

---

### Base Features (34 columns)

Directly derived from raw FRED data after frequency alignment:

| Column | Source Symbol | Transformation |
|---|---|---|
| `FX_USD_CAD` | `DEXCAUS` | Daily → monthly mean |
| `FX_USD_MXN` | `DEXMXUS` | Daily → monthly mean |
| `FX_USD_BRL` | `DEXBZUS` | Daily → monthly mean |
| `REER_USA` | `RBUSBIS` | Already monthly |
| `REER_CAN` | `RBCABIS` | Already monthly |
| `REER_MEX` | `RBMXBIS` | Already monthly |
| `REER_BRA` | `RBBRBIS` | Already monthly |
| `FEDFUNDS` | `FEDFUNDS` | Already monthly |
| `CPI_USA` | `CPIAUCSL` | Already monthly |
| `GDP_USA` | `GDPC1` | Quarterly → forward-fill to monthly |
| `UNRATE_USA` | `UNRATE` | Already monthly |
| `INDPRO_USA` | `INDPRO` | Already monthly |
| `RATE_CAN` | `IRSTCB01CAM156N` | Already monthly |
| `RATE_MEX_3M` | `IR3TIB01MXM156N` | Already monthly |
| `RATE_BRA` | `IRSTCB01BRM156N` | Already monthly |
| `CPI_CAN` | `CANCPIALLMINMEI` | Already monthly |
| `CPI_MEX` | `MEXCPIALLMINMEI` | Already monthly |
| `CPI_BRA` | `BRACPIALLMINMEI` | Already monthly |
| `GDP_CAN` | `CANGDPNQDSMEI` | Quarterly → forward-fill to monthly |
| `GDP_MEX` | `MEXGDPNQDSMEI` | Quarterly → forward-fill to monthly |
| `GDP_BRA` | `NGDPRSAXDCBRQ` | Quarterly → forward-fill to monthly |
| `INDPRO_CAN` | `CANPROINDMISMEI` | Already monthly |
| `INDPRO_MEX` | `MEXPRINTO02IXOBSAM` | Already monthly |
| `INDPRO_BRA` | `BRAPROINDMISMEI` | Already monthly |
| `WTI_oil` | `DCOILWTICO` | Daily → monthly mean |
| `Soybean` | `PSOYBUSDM` | Already monthly |
| `Iron_ore` | `PIORECRUSDM` | Already monthly |

---

### Target Variables — Raw Trade Values (8 columns)

Derived from UN Comtrade Plus. Monthly totals × annual HS2 sector proportions.

| Column | Source | Description |
|---|---|---|
| `exports_total` | `comtrade_monthly_total` | Total US exports to partner (USD) |
| `exports_commodities` | `comtrade_monthly_total` × HS2 annual share | Estimated exports — Commodities sector |
| `exports_manufactured_goods` | `comtrade_monthly_total` × HS2 annual share | Estimated exports — Manufactured Goods sector |
| `exports_high-tech` | `comtrade_monthly_total` × HS2 annual share | Estimated exports — High-Tech sector |
| `imports_total` | `comtrade_monthly_total` | Total US imports from partner (USD) |
| `imports_commodities` | `comtrade_monthly_total` × HS2 annual share | Estimated imports — Commodities sector |
| `imports_manufactured_goods` | `comtrade_monthly_total` × HS2 annual share | Estimated imports — Manufactured Goods sector |
| `imports_high-tech` | `comtrade_monthly_total` × HS2 annual share | Estimated imports — High-Tech sector |

**Methodological note:** Monthly sectoral flows are *estimated* using annual sector proportions from `comtrade_annual_hs2.csv`. This assumes intra-year sector shares are constant within each year. Documented as a limitation in the thesis.

---

### Crisis Dummies (2 columns)

Binary flags for structural break periods:

| Column | Value = 1 | Rationale |
|---|---|---|
| `dummy_gfc` | Sep 2008 – Jun 2009 (10 months) | Global Financial Crisis — sharp trade contraction, USD appreciation |
| `dummy_covid` | Mar 2020 – Jun 2021 (16 months) | COVID-19 pandemic — supply chain disruption, demand shock |

---

### Log-Transformed Target Variables (8 columns)

Applied `log1p(x)` = log(x + 1) to all raw trade value columns. Safe for zero values.

| Column | From |
|---|---|
| `log_exports_total` | `exports_total` |
| `log_exports_commodities` | `exports_commodities` |
| `log_exports_manufactured_goods` | `exports_manufactured_goods` |
| `log_exports_high-tech` | `exports_high-tech` |
| `log_imports_total` | `imports_total` |
| `log_imports_commodities` | `imports_commodities` |
| `log_imports_manufactured_goods` | `imports_manufactured_goods` |
| `log_imports_high-tech` | `imports_high-tech` |

**Why:** Trade values span billions of USD with strong positive skew. Log transformation reduces heteroskedasticity, normalizes the distribution, and converts coefficients to percentage interpretations — standard in international trade econometrics.

---

### First-Differenced Log Target Variables (8 columns)

Applied `.diff()` (x[t] − x[t−1]) to all log-transformed target columns.

| Column | From |
|---|---|
| `diff_log_exports_total` | `log_exports_total` |
| `diff_log_exports_commodities` | `log_exports_commodities` |
| `diff_log_exports_manufactured_goods` | `log_exports_manufactured_goods` |
| `diff_log_exports_high-tech` | `log_exports_high-tech` |
| `diff_log_imports_total` | `log_imports_total` |
| `diff_log_imports_commodities` | `log_imports_commodities` |
| `diff_log_imports_manufactured_goods` | `log_imports_manufactured_goods` |
| `diff_log_imports_high-tech` | `log_imports_high-tech` |

**Why:** ARIMA requires stationary series. Log trade flows have a positive trend (non-stationary). First differencing removes the trend, yielding the month-over-month growth rate. ADF test confirmed stationarity of differenced series.

**Note:** First row is NaN by construction (no prior month available). Dropped during model training.

---

### Lagged Exchange Rate Features (28 columns)

Past values of exchange rates to capture delayed trade adjustment effects (J-curve).
Lags: 1, 3, 6, 12 months. Applied to: `FX_USD_CAD`, `FX_USD_MXN`, `FX_USD_BRL`, `REER_USA`, `REER_CAN`, `REER_MEX`, `REER_BRA`.

| Pattern | Example | Meaning |
|---|---|---|
| `{col}_lag1` | `FX_USD_BRL_lag1` | Exchange rate value 1 month ago |
| `{col}_lag3` | `FX_USD_BRL_lag3` | Exchange rate value 3 months ago |
| `{col}_lag6` | `FX_USD_BRL_lag6` | Exchange rate value 6 months ago |
| `{col}_lag12` | `FX_USD_BRL_lag12` | Exchange rate value 12 months ago |

**Why:** Trade contracts are negotiated in advance. Exchange rate shocks take 3–12 months to fully propagate to trade volumes (J-curve effect).

---

### Moving Average Features (9 columns)

Rolling means of bilateral exchange rates. Windows: 3, 6, 12 months.
Applied to: `FX_USD_CAD`, `FX_USD_MXN`, `FX_USD_BRL`.

| Pattern | Example | Meaning |
|---|---|---|
| `{col}_ma3` | `FX_USD_CAD_ma3` | 3-month rolling mean of exchange rate |
| `{col}_ma6` | `FX_USD_CAD_ma6` | 6-month rolling mean of exchange rate |
| `{col}_ma12` | `FX_USD_CAD_ma12` | 12-month rolling mean of exchange rate |

**Why:** Smooths short-term volatility; captures medium-term currency trend that better represents the "persistent" exchange rate environment faced by traders.

---

### Percentage Change Features (4 columns)

Month-over-month rate of change: (x[t] − x[t−1]) / x[t−1].
Applied to: `FX_USD_CAD`, `FX_USD_MXN`, `FX_USD_BRL`, `REER_USA`.

| Column | Meaning |
|---|---|
| `FX_USD_CAD_pct` | Monthly % appreciation/depreciation of CAD vs USD |
| `FX_USD_MXN_pct` | Monthly % appreciation/depreciation of MXN vs USD |
| `FX_USD_BRL_pct` | Monthly % appreciation/depreciation of BRL vs USD |
| `REER_USA_pct` | Monthly % change in US real effective exchange rate |

**Why:** Captures the *shock* (month's new information) rather than the level (which includes all historical accumulation).

---

### Calendar Features (3 columns)

| Column | Values | Purpose |
|---|---|---|
| `month` | 1–12 | Captures monthly seasonality (e.g., Q4 import surge before holidays) |
| `quarter` | 1–4 | Captures quarterly patterns (agricultural cycles, earnings seasons) |
| `year` | 2010–2024 | Long-term trend control |

---

## EXCLUDED VARIABLES — SUMMARY

| Variable | Source | Reason for Exclusion |
|---|---|---|
| `DTWEXBGS` | FRED | Multicollinearity with `REER_USA` (both measure USD strength vs. basket) |
| `GDP_per_capita_USD` | World Bank | Annual frequency; overlaps with FRED GDP |
| `GDP_total_USD` | World Bank | Annual frequency; overlaps with FRED GDP |
| `Population` | World Bank | Changes too slowly; uninformative at monthly frequency |
| `Inflation_pct` | World Bank | Annual frequency; overlaps with monthly CPI |
| `Current_account_USD` | World Bank | Annual frequency; interpolation would be artificial |
| `FDI_inflows_USD` | World Bank | Annual frequency; interpolation would be artificial |

---

## TRAIN / TEST SPLIT

| Set | Period | Months |
|---|---|---|
| Training | Jan 2010 – Dec 2021 | 144 months |
| Test | Jan 2022 – Dec 2024 | 36 months |

Chronological split — no shuffling. Ensures no data leakage from future to past.

---

*Last updated: 2026-03-27*
