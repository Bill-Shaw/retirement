# Retirement Planning Suite

A self-contained, single-file retirement planning toolkit that runs entirely in your browser — no server, no install, no data sent anywhere.

---

## Files

| File | Description |
|------|-------------|
| `retirement-suite.html` | **Unified app — use this one** |
| `retirement-calculator.html` | Original standalone cash flow calculator |
| `montecarlo-retirement.html` | Original standalone Monte Carlo simulator |

Open `retirement-suite.html` directly in any modern browser to get started.

---

## Retirement Suite (`retirement-suite.html`)

The suite merges both tools into one app with a **Shared Profile** at the top. Fill in your information once and apply it to any tool — no re-entering the same numbers.

### Shared Profile

The collapsible panel at the top holds all common inputs:

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
| Total portfolio balance | Total invested assets (used by Monte Carlo) |

Once filled in, click **Apply to Cash Flow →** or **Apply to Monte Carlo →** to push the values into each tool. The profile summary line (e.g. *Bill (50) · Jane (52) · $10k/mo exp · $2.00M portfolio*) updates in real time as you type.

---

### Tool 1 — Cash Flow Calculator

Projects year-by-year household income vs. expenses across the full horizon.

**What it models**
- Work income for each person (stops at their retirement age, optionally inflated with CPI)
- 401k / IRA withdrawals (switches from Rule-of-55 balance to full balance at age 59½)
- Social Security benefits for each person (starts at their claimed age, grows with CPI)
- Monthly expenses growing with inflation

**Outputs**
- **Metrics bar** — current-year income gap, break-even year, total projected shortfall, end-of-horizon gap
- **Cash flow chart** — stacked bar chart of income sources vs. the expense line
- **Surplus / shortfall chart** — year-by-year surpluses (green) and deficits (red)
- **Year-by-year detail table** — every income source, expense, and gap for each calendar year, with deficit rows highlighted

**Scenarios**

Run multiple what-if comparisons side by side. Use **+ Copy** to duplicate the current scenario and adjust individual assumptions (e.g. different retirement ages or withdrawal rates) without losing your baseline. Use the scenario bar to switch between them.

**Tool-specific settings** (per scenario, in the sidebar)
- Withdrawal rate
- Inflation rate
- Projection years
- Inflate work income with CPI (toggle)
- Inflate 401k withdrawals with CPI — aggressive option (toggle)

---

### Tool 2 — Monte Carlo Simulator

Stress-tests your retirement portfolio across thousands of randomized market paths to estimate the probability your money lasts the full horizon.

**What it models**
- **Accumulation phase** — portfolio grows with random annual returns plus contributions
- **Retirement phase** — portfolio funds inflation-adjusted spending net of other income
- Returns drawn from a log-normal distribution calibrated to your mean return and volatility inputs

**Sidebar inputs**

| Field | Notes |
|-------|-------|
| Current portfolio | Total invested assets |
| Current age | Used to label the x-axis |
| Annual contributions | Added each year during accumulation |
| Years until retirement | Length of the accumulation phase |
| Annual spending | Today's dollars; grows with inflation in retirement |
| Years in retirement | Planning horizon after stopping work |
| Other income (SS, etc.) | Today's dollars; grows with inflation |
| …starts in (years) | Years from now until that income begins |
| Allocation preset | Sets mean return + volatility; or choose Custom |
| Mean return / Volatility | Arithmetic annual figures |
| Inflation | Applied to spending and income |
| Number of paths | 500 – 50,000 simulations |

When applying from the Shared Profile, the tool auto-derives:
- **Spending** from monthly expenses × 12
- **Years to retirement** from Person 1's retirement age minus current age
- **Retirement years** from projection years minus years to retirement
- **Other income** from combined SS benefits × 12
- **SS start** from the earlier of the two SS claim ages

**Outputs**
- **Success rate** — % of paths where the portfolio never hits zero
- **Verdict** — STRONG (≥90%), WORKABLE (≥75%), FRAGILE (≥55%), AT RISK (<55%)
- **Stats** — median ending balance, 10th-percentile balance, and years money lasts in the worst 10% of scenarios
- **Fan chart** — 10–90th and 25–75th percentile bands with the median path highlighted; dashed line marks retirement
- **Histogram** — distribution of all final portfolio values; red bar = ran out of money

Click **Run Simulation** (or press Enter in any input) to run. Each run draws fresh random paths.

---

## Export & Import

The **Export JSON** / **Import JSON** buttons in the top-right header save and restore your entire session — shared profile, all calculator scenarios, and Monte Carlo settings — as a single `.json` file.

Your data is also automatically saved to `localStorage` in your browser and persists between sessions on the same machine.

---

## Adding a New Tool

The suite is built to be extended. To add a new tool:

1. **Add a tab button** in the `<!-- TOOL TABS -->` section:
   ```html
   <button class="tool-tab" id="tab-mytool" onclick="switchTool('mytool')">My Tool</button>
   ```

2. **Add a tool panel** below the existing panels:
   ```html
   <div id="tool-mytool" class="tool-panel">
     <div class="tool-layout">
       <div class="sidebar" id="mytool-sidebar"></div>
       <div class="main" id="mytool-main"></div>
     </div>
   </div>
   ```

3. **Write your logic** — prefix all functions and element IDs with your tool name to avoid collisions. Read shared values from the profile object `P`.

4. **Call your render function** from `init()` at the bottom of the script.

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
