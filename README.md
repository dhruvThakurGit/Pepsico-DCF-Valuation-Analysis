# 📈 PepsiCo DCF Valuation Model

A full intrinsic value DCF model for **PepsiCo Inc. (NASDAQ: PEP)**, built as of **30 September 2020**. The model uses 3.5 years of historical financials (FY2016–H1 2020), projects forward 4.5 years to FY2024, and derives equity value per share via two terminal value methods — Gordon Growth and Exit Multiple — each with a 2D sensitivity table.

---

## Workbook Structure

```
Pepsico_DCF_Valuation.xlsx
├── Revenue Segments      ← 7-segment revenue breakdown & organic growth projection
├── PepsiCo-IS            ← Income Statement: historical (FY2016–H1 2020) + projected (H2 2020–FY2024)
├── PepsiCo-BS            ← Balance Sheet: historical + projected
├── PepsiCo-CFS           ← Cash Flow Statement: historical + projected
├── Fixed Assets Schedule ← PPE turnover, depreciation rate, capex build
├── Debt Schedule         ← Mandatory & discretionary debt, circular reference workaround
├── WACC                  ← Cost of equity (CAPM), cost of debt, bottom-up beta derivation
├── Beta Calculation      ← Weekly return regression: PepsiCo vs. 8 peers vs. S&P 500
├── DCF                   ← FCFF build, discounting, terminal value, equity bridge, sensitivity
└── STEP BY STEP          ← Annotated methodology notes on model construction
```
<img width="1579" height="808" alt="image" src="https://github.com/user-attachments/assets/bc64b654-ddec-4f88-93cc-1603fdde17fc" />

<img width="1165" height="741" alt="image" src="https://github.com/user-attachments/assets/92e3a472-e055-4a25-a9bf-e115dd2488c6" />

---

## Data Sources

| Input | Source |
|---|---|
| Historical financials (IS, BS, CFS) | PepsiCo 10-K (Dec 2019), 10-Q (June 2020) |
| Segment revenue | PepsiCo 10-K (Dec 2019, Pg 9) — 7 geographic/product segments |
| Mandatory debt schedule | PepsiCo 10-K financial notes (maturity schedule) |
| Peer levered betas | Competitor 10-K filings (Coca-Cola, Kraft Heinz, Mondelez, Monster Beverage) |
| Equity Risk Premium | **Damodaran ERP — 6.01%** |
| Risk-free rate | **10-year US Treasury yield — 0.90%** (YTD average as of valuation date) |
| Pre-tax cost of debt | Rating-based default spread — 1.88% |
| Shares outstanding | PepsiCo 10-Q (exact diluted shares on 13 June 2020: 1,384.6M) |
| Non-operating assets | PepsiCo 10-Q (investments in non-controlled affiliates: $2,911M) |

---

## Methodology

### Revenue (Revenue Segments sheet)
Revenue is broken down into **7 reporting segments**: FLNA, QFNA, PBNA, LatAm, Europe, AMESA, and APAC. Each segment is projected using a blend of organic growth (price + volume), with H2 2020 estimated using H1 2020 vs. H1 2019 growth rates and FY2021–FY2024 using FY2016–FY2019 CAGR. Special items are stripped from reported figures throughout.

| | FY2016 | FY2017 | FY2018 | FY2019 | FY2020E | FY2021E | FY2022E | FY2023E | FY2024E |
|---|---|---|---|---|---|---|---|---|---|
| Net Revenue ($M) | 62,799 | 63,525 | 64,661 | 67,161 | 68,902 | 71,678 | 74,608 | 77,700 | 80,964 |

### Income Statement
All line items projected as a % of sales except schedule-driven lines (depreciation, amortization, interest) which flow from their respective schedules. Tax rate transitions linearly from the effective rate of ~21.5% in H2 2020 to the marginal rate of 27% by FY2024.

### Fixed Assets Schedule
- **PPE:** Projected using the PPE Turnover Ratio (Net Revenue / Opening PPE) derived from historical years. Capex = change in required PPE + depreciation. Depreciation rate back-calculated from historical filings and applied to opening gross block.
- **Intangibles:** Amortizable intangibles depreciated from opening balance; no new additions assumed (acquisitions excluded). Goodwill and indefinite-life intangibles held constant.

### Debt Schedule
Split into **mandatory** (scheduled repayments per 10-K notes) and **discretionary** (residual sweep):
- Interest is charged on **opening debt balance** to break the circular reference (interest → NI → cash → debt raised → interest)
- Discretionary debt is raised when period cash (after minimum cash reserve of 2% of sales) is insufficient; repaid to the extent of opening discretionary balance when cash is surplus
- Minimum cash: 2% of sales (industry convention for large-cap US consumer staples)

### Cash Flow Statement
Built via the indirect method from Net Income. Interest expense is classified under financing activities in the FCFF build. Share-based compensation projected as % of SG&A and added back as a non-cash item.

---

## WACC

**WACC: 5.22%**

### Cost of Equity — 6.09% (CAPM)

| Component | Value | Source |
|---|---|---|
| Risk-free rate | 0.90% | 10-year US Treasury yield (YTD avg, Sep 2020) |
| Equity Risk Premium | 6.01% | Damodaran ERP |
| Beta (Bottom-up) | 0.863 | Unlevered peer average re-levered to PepsiCo's structure |
| Additional Risk Premium | 0% | — |
| **Cost of Equity** | **6.09%** | Rf + β × ERP |

### Bottom-Up Beta Construction

Peers used (per PepsiCo 10-K competitor list): Coca-Cola, Kraft Heinz, Mondelez, Monster Beverage. Campbell, Conagra, Kellogg, and Keurig Dr Pepper were excluded due to low R² from regression.

| Peer | Levered β | D/(D+E) | Unlevered β | R² |
|---|---|---|---|---|
| Coca-Cola | 0.902 | 20.2% | 0.761 | 0.59 |
| Kraft Heinz | 0.797 | 38.2% | 0.549 | 0.25 |
| Mondelez | 0.757 | 21.9% | 0.629 | 0.52 |
| Monster Beverage | 1.025 | 0.0% | 1.025 | 0.48 |
| **Industry Average** | | | **0.741** | |

PepsiCo's unlevered industry beta of 0.741 is then re-levered using PepsiCo's **target capital structure** (Debt: 18.4%, Equity: 81.6%) → Relevered β = **0.863**

### Cost of Debt & Capital Structure

| Component | Value |
|---|---|
| Pre-tax cost of debt | 1.88% (rating-based, current issuance rate) |
| After-tax cost of debt | 1.37% (marginal tax rate 27%) |
| Debt weight | 18.4% (market value basis) |
| Equity weight | 81.6% |
| **WACC** | **5.22%** |

---

## DCF Valuation

**Projection period:** 4.5 years (H2 2020 → FY2024), using **mid-year discounting**

**FCFF Build:**

| | H2 2020 | FY2021 | FY2022 | FY2023 | FY2024 |
|---|---|---|---|---|---|
| EBIT ($M) | 6,280 | 11,542 | 12,020 | 12,524 | 13,055 |
| Tax Rate | 21.5% | 22.6% | 24.1% | 25.5% | 27.0% |
| NOPAT ($M) | 4,931 | 8,931 | 9,125 | 9,325 | 9,530 |
| + D&A | 1,267 | 2,633 | 2,734 | 2,841 | 2,955 |
| + ΔWorking Capital | 2,918 | 375 | 146 | 154 | 163 |
| − Capex | (2,349) | (2,100) | (3,479) | (3,635) | (3,799) |
| **FCFF ($M)** | **6,768** | **9,838** | **8,526** | **8,686** | **8,849** |

**PV of Explicit Period:** $34,679M

### Method 1 — Gordon Growth Model

| Item | Value |
|---|---|
| Terminal Year FCFF | $9,027M |
| Long-term Growth Rate | 2.01% (≈ US nominal GDP growth) |
| Terminal Value | $281,303M |
| PV of Terminal Value | $225,871M |
| DCF Firm Value | $260,550M |
| + Non-operating assets | $2,911M |
| − Gross Debt | $44,978M |
| + Cash | $8,927M |
| − Minority Interest | $96M |
| **Equity Value** | **$227,314M** |
| Diluted Shares | 1,384.6M |
| **Intrinsic Value per Share** | **$164.17** |

### Method 2 — Exit Multiple (EV/EBITDA)

| Item | Value |
|---|---|
| LTM EV/EBITDA Multiple | 14x |
| Terminal Year EBITDA | $16,011M |
| Terminal Value | $224,147M |
| **Intrinsic Value per Share** | **$131.02** |

---

## Sensitivity Analysis

Both terminal value methods include a **2D sensitivity table** in the DCF sheet.

**Gordon Growth — Post-tax IRR (WACC × LTGR)**

Rows: WACC (4.5% → 6.5%) | Columns: Long-term growth rate (1.0% → 2.75%)
> Base case: WACC 5.22%, LTGR 2.01% → **$164.17/share**

**Exit Multiple — Price per share (WACC × EV/EBITDA)**

Rows: WACC (4.5% → 6.5%) | Columns: Exit EV/EBITDA multiple (11x → 19x)
> Base case: WACC 5.22%, 14x → **$131.02/share**

---

## File Info

| Detail | Value |
|---|---|
| Format | `.xlsx` (Microsoft Excel) |
| Sheets | 10 |
| Currency | USD (millions, except per share data) |
| Valuation Date | 30 September 2020 |
| Last Actual Data Point | 13 June 2020 (PepsiCo 10-Q) |
| Projection Horizon | H2 2020 – FY2024 (4.5 years) |
| Subject | PepsiCo Inc. (NASDAQ: PEP) |
