# Retirement Calculator

A self-contained retirement income projection tool. No install, no server, no dependencies — just open the HTML file in any browser.

## Usage

Download `retirement-calculator.html` and open it in any modern browser (Chrome, Edge, Firefox, Safari). Nothing else is needed — the file works offline.

Or use the hosted version via GitHub Pages:
[https://bill-shaw.github.io/retirement/retirement-calculator.html](https://bill-shaw.github.io/retirement/retirement-calculator.html)

## Features

- **Two-person household** — independent retirement ages, income, Social Security, and 401k fields per person. Names are editable so you can personalize the labels.
- **Rule of 55 support** — separate 401k balance fields for the amount available at retirement vs. the full balance of all retirement accounts at 59½.
- **Social Security** — configurable claim age (62–70) and monthly benefit per person, with COLA inflation applied automatically.
- **Inflation modeling** — expenses inflate every year. Optional toggles to also inflate work income and/or 401k withdrawals with CPI.
- **Multiple scenarios** — copy and modify the current scenario to compare options side by side (e.g. retire at 54 vs. 56 vs. 58). Scenarios are named and independently editable.
- **Export / Import** — save all scenarios to a `.json` file and reload them later, or share with an advisor.
- **Charts** — stacked cash flow chart (income by source vs. expenses) and surplus/shortfall chart. X-axis shows calendar years with ages below.
- **Year-by-year table** — full detail view with deficit rows highlighted.

## Fields

### Per person
| Field | Description |
|---|---|
| Current age | Age today |
| Retire at age | Age when work income stops |
| Claim SS at age | Age to begin Social Security (62–70) |
| SS benefit / mo | Estimated monthly Social Security benefit |
| Monthly income | Gross monthly income while working |
| 401k balance at retirement | Amount accessible at retirement — use Rule of 55 amount if retiring before 59½ |
| All retirement accounts at 59½ | Total 401k + IRA + other accounts accessible at 59½ |

### Assumptions
| Field | Description |
|---|---|
| Monthly expenses | Current monthly household expenses |
| Withdrawal rate | Annual percentage drawn from retirement accounts (default 5%) |
| Inflation rate | Annual CPI assumption (default 2.5%) |
| Projection years | How many years to project forward (default 35) |
| Inflate work income | Apply CPI to salaries each year (default on) |
| Inflate 401k withdrawals | Increase withdrawals with CPI to maintain purchasing power (default off — conservative) |

## Notes

- Social Security benefits are inflated automatically via COLA each year regardless of the inflation toggles.
- If retiring before 59½, the **401k balance at retirement** field should reflect only the amount accessible under the Rule of 55 (must be from your current employer's plan and you must separate from service in or after the year you turn 55).
- This tool is for planning discussions only. It is not financial advice. Consult a fee-only CFP for personalized guidance.

## License

MIT
