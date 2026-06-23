# Private Credit Underwriting Engine

A self-contained quantitative underwriting engine for direct-lending / private-credit
decisions. It answers: **given this borrower, this loan structure, and this capital
stack — should we lend, at what price, under what terms, and with what risks?**

## Run it

Double-click **`credit_underwriting_engine.html`** — it opens in any browser. No install,
no server, no internet. All computation runs locally in the page.

The form is pre-filled with the example borrower, so results appear immediately. Edit any
input and click **Run Underwriting Analysis** to recompute.

## What it does

| Stage | Output |
|-------|--------|
| Borrower input | Clean financial profile |
| Loan terms | All-in coupon, debt/interest/fee schedule |
| Capital structure | Priority of claims, lender's position |
| Financial ratios | Leverage, net leverage, coverage, DSCR, FCF conv., margin, liquidity |
| Credit scoring | Weighted scorecard → 5-yr PD → implied rating, strengths/weaknesses |
| Projection assumptions | Upside / base / downside cases |
| Financial projection | Year-by-year revenue, EBITDA, FCF, debt, credit metrics |
| Covenant analysis | Per-year breach test + simulated breach probability |
| Deterministic IRR / MOIC | Lender returns in each scenario |
| **Monte Carlo** | 10,000+ simulated futures → IRR distribution, PD, EL, recovery |
| Default & loss | LGD, EAD, expected loss |
| Capital waterfall | Recovery by tranche on distressed EV |
| Loan pricing | Required vs offered spread, spread gap, recommended coupon |
| Decision rules | Approve / Reprice / Reject |
| Final report | IC-style recommendation dashboard |

### Inputs vs. computed values

Only genuine underwriting inputs are editable (revenue, EBITDA, spread, covenant
thresholds, volatilities, etc.). Values the engine *derives* from those inputs — the
all-in coupon, the assembled capital stack, residual equity, all ratios and simulation
outputs — are shown as clearly-marked read-only "🔒 computed" displays, never typed.

## How the quant core works

For each Monte Carlo path the engine draws correlated annual shocks to revenue growth,
EBITDA margin, and the base rate; projects the company forward; tests covenants and
default triggers each year; on default computes recovery through the priority waterfall on
distressed enterprise value (EBITDA × distressed multiple); discounts the lender's realized
cash flows to an IRR. Aggregating across all paths yields probability of default, expected
loss, breach probability, recovery, and the full IRR distribution shown in the histogram.

Pricing: `required spread = expected loss + target excess return + liquidity premium + risk buffer`.
The decision compares risk metrics to committee thresholds and the offered spread to the
required spread.

## Notes & assumptions

- A single-name model for decision support — **not** investment advice. Outputs are only as
  good as the inputs; stress the volatility, recovery, and default-trigger assumptions.
- D&A is approximated as ≈ capex (steady state); existing debt is treated as fixed-rate; the
  proposed loan floats with the base rate. PIK, exit fees, and second-lien positions are
  supported.
- Scorecard PD (Module 5) and simulated PD (Module 10) are shown side by side; the simulated
  PD drives the decision.
