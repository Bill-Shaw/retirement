# Retirement Planning Suite

Most retirement calculators assume you can't touch your 401k until age 59½. This tool was built to address that gap — specifically to model the **Rule of 55**, which allows penalty-free 401k withdrawals starting the year you turn 55 if you've left your employer. The cash flow calculator accounts for this by using a separate "401k at retirement" balance during the 55–59½ window, then stepping up to your full account balance once you reach 59½.

A self-contained, single-file retirement planning toolkit that runs entirely in your browser — no server, no install, no data sent anywhere.

---

Open `retirement-suite.html` directly in any modern browser to get started.

---

## Layout

The app uses a **persistent dark sidebar** on the left and a **results-only main area** on the right.

**Sidebar** (always visible):
- Tool switcher tabs at the top — **Cash Flow** and **Monte Carlo**
- **Person 1** and **Person 2** sections with all person-specific inputs
- **Shared Assumptions** (expenses, withdrawal rate, inflation, projection years, portfolio balance)
- A divider below the shared fields showing tool-specific settings that change when you switch tabs
- Export / Import buttons at the bottom

**Main area**: projections and charts only — no inputs.

Enter your information once in the sidebar. Both tools read from the same profile automatically — no Apply buttons, no re-entering the same numbers.

---

## Retirement Suite (`retirement-suite.html`)

### Shared Profile

All common inputs live in the persistent sidebar:

**Person 1 & Person 2**
- Name, current age, retirement age
- Social Security claim age and estimated monthly benefit
- Monthly work income (gross)
- 401k / retirement account balances (at retirement, and at age 59½)

**Shared Assumptions**

| Field | Description |
|-------|-------------|
| Monthly expenses | Current household expenses |
| Withdrawal rate | Annual % drawn from retirement accounts |
| Inflation rate | Annual CPI assumption |
| Projection years | How many years to project forward |
| Portfolio balance | Total invested assets (used by Monte Carlo) |

---

### Tool 1 — Cash Flow Calculator

Projects year-by-year household income vs. expenses across the full horizon. Switches to automatically when you enter person ages.

**What it models**
- Work income for each person (stops at their retirement age, optionally inflated with CPI)
- 401k / IRA withdrawals (switches from Rule-of-55 balance to full balance at age 59½)
- Social Security benefits for each person (starts at their claimed age, grows with CPI)
- Monthly expenses growing with inflation

**Outputs**
- **Metrics bar** — current-year income gap, break-even year, total projected shortfall, end-of-horizon gap
- **Cash flow chart** — stacked bar chart of income sources vs. the expense line
- **Surplus / shortfall chart** — year-by-year surpluses (green) and deficits (red)
- **Year-by-year detail table** — every income source, expense, and gap for each calendar year

**Scenarios**

Run multiple what-if comparisons side by side. Use **+ Copy** to duplicate the current scenario. The **Scenario Overrides** section in the sidebar lets you adjust individual assumptions per-scenario (withdrawal rate, inflation, projection years, expenses) — leave a field blank to inherit the shared value.

**Tool-specific settings** (per scenario, in the sidebar)
- Withdrawal rate override
- Inflation rate override
- Projection years override
- Monthly expenses override
- Inflate work income with CPI (toggle)
- Inflate 401k withdrawals with CPI — aggressive option (toggle)

---

### Tool 2 — Monte Carlo Simulator

Stress-tests your retirement portfolio across thousands of randomized market paths to estimate the probability your money lasts the full horizon.

**What it models**
- **Accumulation phase** — portfolio grows with random annual returns plus contributions
- **Retirement phase** — portfolio funds inflation-adjusted spending net of other income
- Returns drawn from a log-normal distribution calibrated to your mean return and volatility inputs

The simulator automatically derives its parameters from your shared profile:

| Derived from profile | How |
|---------------------|-----|
| Portfolio balance | From Shared Assumptions |
| Current age | From Person 1 current age |
| Annual spending | Monthly expenses × 12 |
| Years to retirement | Person 1 retire age − current age |
| Retirement years | Projection years − years to retirement |
| Other income (SS) | Combined SS benefits × 12 |
| SS start | Earlier of Person 1 / Person 2 claim age |

**Monte Carlo-specific settings** (in the sidebar when on this tab)

| Field | Notes |
|-------|-------|
| Annual contributions | Added each year during the accumulation phase |
| Allocation preset | Sets mean return + volatility; or choose Custom |
| Mean return / Volatility | Arithmetic annual figures |
| Inflation | Applied to spending and income during retirement |
| Number of paths | 500 – 50,000 simulations |

**Outputs**
- **Success rate** — % of paths where the portfolio never hits zero
- **Verdict** — STRONG (≥90%), WORKABLE (≥75%), FRAGILE (≥55%), AT RISK (<55%)
- **Stats** — median ending balance, 10th-percentile balance, and years money lasts in the worst 10% of scenarios
- **Fan chart** — 10–90th and 25–75th percentile bands with the median path highlighted; dashed line marks retirement
- **Histogram** — distribution of all final portfolio values; red bar = ran out of money

Click **Run Simulation** to run. Each run draws fresh random paths.

---

## Export & Import

The **Export JSON** / **Import JSON** buttons at the bottom of the sidebar save and restore your entire session — shared profile, all calculator scenarios, and Monte Carlo settings — as a single `.json` file.

Your data is also automatically saved to `localStorage` in your browser and persists between sessions on the same machine.

---

## Adding a New Tool

The suite is designed to be extended. To add a new tool:

1. **Add a tab button** in the sidebar head section:
   ```html
   <button class="sb-tab" id="sb-tab-mytool" onclick="switchTool('mytool')">My Tool</button>
   ```

2. **Add a tool panel** in the main area:
   ```html
   <div class="tool-main" id="tm-mytool">
     <div class="results" id="mytool-main"></div>
   </div>
   ```

3. **Add a sidebar section** in `renderSidebar()` for `activeTool === 'mytool'` — read shared values from `P` directly.

4. **Write your render function** — prefix all IDs with your tool name to avoid collisions. Call it from `init()`.

**Shared profile object `P`** exposes:
`hName`, `wName`, `hCur`, `hRet`, `hSSA`, `hSSB`, `hInc`, `h401e`, `h401f`,
`wCur`, `wRet`, `wSSA`, `wSSB`, `wInc`, `w401e`, `w401f`,
`exp`, `wdr`, `inf`, `yrs`, `balance`

---

## Notes

- All calculations are for planning and discussion purposes only — not financial advice.
- Monte Carlo returns use a log-normal distribution; real markets exhibit fat tails and serial correlation this model does not capture.
- Monte Carlo withdrawals are pre-tax — raise the spending figure to approximate your effective tax rate.
- No data ever leaves your browser.

## License

MIT
