# Student Loan vs. Invest Decision Engine

An interactive, browser-based financial model that helps students decide whether to aggressively pay off student loans or invest their monthly surplus. Unlike basic calculators, this tool runs a Monte Carlo simulation, accounts for tax treatment, and includes a behavioral utility score for the psychological value of being debt-free.

> Built on corporate finance principles: Time Value of Money · Capital Budgeting · Risk-Return  
> Course chapters: Ch. 7–9, 4, 5, 12, 15

---

## Live Demo

Open `index.html` directly in any browser — no server, no dependencies, no build step required.

---

## What Makes This Different

Most "pay off debt vs. invest" calculators give you a single deterministic answer based on one assumed return rate. This model goes further in three ways:

**1. Monte Carlo simulation (5,000 runs)**  
Rather than assuming a fixed market return, the model samples from a normal distribution using your expected return and volatility inputs. It reports the *probability* that investing beats paying off the loan — not just a point estimate.

**2. Student-loan specific logic**  
Models the full amortization schedule, captures the interest savings from aggressive early repayment, and correctly handles the "freed cash flow" that becomes available once the loan is eliminated ahead of schedule.

**3. Debt-free utility score**  
A 0–10 slider lets you assign personal weight to the psychological value of being debt-free. At high values (8+), the model overrides the pure math and recommends paying off the loan — because peace of mind is a legitimate financial variable.

---

## How It Works

### Strategy A — Aggressive Payoff
Each month, the standard payment plus the full monthly surplus is applied toward the loan principal. Once the loan is eliminated, the freed cash flow is invested in the market for the remainder of the horizon.

```
Extra Monthly Payment = Standard Payment + Monthly Surplus
Post-Payoff Investment = Freed Cash Flow × (1 + r_market)^t
Net Wealth A = Post-Payoff Portfolio × (1 − Capital Gains Tax Rate)
```

### Strategy B — Invest the Surplus
The standard monthly payment is made on the loan. The surplus is invested every month for the full duration of the horizon.

```
Monthly Investment = Surplus
Portfolio Value = Σ [Surplus × (1 + r_monthly)^(n−t)]
Net Wealth B = Portfolio × (1 − Capital Gains Tax Rate) − Remaining Loan Balance
```

### Break-Even Return
The model solves for the market return rate at which both strategies produce equal wealth — the threshold above which investing is mathematically superior.

```
Break-even r* = rate where Net Wealth B = Net Wealth A
If expected market return > r*  →  Invest surplus
If expected market return < r*  →  Pay off aggressively
```

### Monte Carlo Engine
The model simulates 5,000 independent market scenarios. Each monthly return is drawn from:

```
r_monthly = (μ/12) + (σ/12) × Z
Where Z ~ N(0, 1)  (Box-Muller transform)
```

The output is the percentage of scenarios in which Strategy B (invest) produces higher net wealth than Strategy A (payoff).

---

## Inputs

| Parameter | Description | Default |
|---|---|---|
| Loan balance | Outstanding principal | $45,000 |
| Interest rate | Annual rate on the loan | 6.5% |
| Loan term | Standard repayment period | 10 years |
| Monthly surplus | Extra cash available each month | $300 |
| Expected market return | Annualized return assumption | 8.0% |
| Return volatility | Annualized standard deviation | 14.0% |
| Capital gains tax | Tax rate applied to investment gains | 15% |
| Debt-free utility | Behavioral weight (0 = pure math, 10 = max psychological value of payoff) | 5 |

---

## Outputs

| Metric | Description |
|---|---|
| Break-even return | Market return at which both strategies tie |
| Interest saved | Total interest eliminated by aggressive payoff |
| Net wealth advantage | Dollar difference between strategies at base case |
| Prob. invest wins | % of Monte Carlo runs where investing beats payoff |

### Three Views
- **Wealth over time** — line chart comparing cumulative net wealth year-by-year
- **Scenario analysis** — bear / base / bull market comparison table
- **Monte Carlo distribution** — histogram of outcome differences across 5,000 simulations

---

## Verdict Logic

| Condition | Recommendation |
|---|---|
| Utility ≥ 8 | Pay off (behavioral override) |
| Market return > break-even AND Monte Carlo > 65% | Invest surplus |
| Market return ≤ break-even OR Monte Carlo < 50% | Pay off aggressively |
| Otherwise | Mixed — consider hybrid strategy |

---

## Files

```
/
├── index.html      The complete app (single file, zero dependencies)
└── README.md       This file
```

All logic, styling, and charting is self-contained in `index.html`. Chart.js is loaded from a CDN — an internet connection is required to render charts.

---

## Running Locally

```bash
# Clone the repo
git clone https://github.com/your-username/student-loan-vs-invest.git

# Open directly in browser
open index.html
```

No npm, no Python server, no build tools needed.

---

## Extending the Model

Some directions to take this further:

- **Federal loan forgiveness** — add an IBR/PSLF path where remaining balance is forgiven after N years
- **Employer 401k match** — model the guaranteed 50–100% match return as a priority investment before loan payoff
- **Inflation adjustment** — express all outputs in real (inflation-adjusted) dollars
- **Tax deductibility** — student loan interest is deductible up to $2,500/year for qualifying borrowers; factor this into the after-tax loan cost
- **Multi-loan support** — apply the debt avalanche or debt snowball method across multiple loans simultaneously

---

## Course Concepts

| Concept | Application in Model |
|---|---|
| Time Value of Money (Ch. 7–9) | Discounting future cash flows; present value of wealth under each strategy |
| Capital Budgeting (Ch. 4, 5) | Framing debt repayment as an investment decision with NPV and IRR logic |
| Risk-Return (Ch. 12, 15) | CAPM-informed return assumptions; volatility as a risk input; probability-weighted outcomes |
| Behavioral Finance (Notes) | Debt-free utility slider; modeling irrational but real preferences |

---

## Disclaimer

This tool is for educational purposes only. It does not constitute financial, tax, or legal advice. All projections are hypothetical and depend on user-supplied assumptions. Past market returns do not guarantee future results.
