# Finance track

Goal: relearn finance/accounting foundations on their own terms, before client discussions. The first pass is pure finance/accounting: no data warehousing, BI, dbt, dashboard, or AI framing until the foundations are stable.

## Rule for this track

For the first pass, keep finance separate from analytics engineering.

- No BigQuery, dbt, star-schema, semantic-layer, or dashboard examples unless explicitly reintroduced in a later bridge module.
- No “financial statements as data products” framing at the start.
- First build the standalone financial mental model and vocabulary used by finance teams.

## Recommended lesson order

### Block A — The mental model

1. `lesson-01-what-is-a-company-financially.md` — What is a company financially?
   - A company as an economic entity that earns, spends, owns, owes, and is financed.
   - First distinctions: revenue vs cash, expense vs payment, asset vs cost, liability vs expense, profit vs cash.
2. `lesson-02-the-accounting-equation.md` — Assets = liabilities + equity.
   - Why every resource has a financing source.
   - The balance sheet as a snapshot of resources and claims.
3. `lesson-03-financial-statements-overview.md` — The three primary statements and what each one answers.
   - Balance sheet: what the company controls and owes at a date.
   - Income statement: performance over a period.
   - Cash flow statement: cash movements over a period.

### Block B — Performance, position, and cash

4. `lesson-04-income-statement.md` — Revenue, expenses, and profit.
   - Gross profit, operating profit, net profit; margins; recurring vs non-recurring items.
5. `lesson-05-balance-sheet.md` — Assets, liabilities, and equity.
   - Current/non-current, working capital items, debt, retained earnings.
6. `lesson-06-cash-flow-statement.md` — Cash movements and why profit is not cash.
   - Operating, investing, and financing cash flows.

### Block C — Recognition rules that explain the differences

7. `lesson-07-accrual-accounting.md` — Why accounting recognizes events before/after cash moves.
   - Recognition, matching logic, receivables, payables, deferred revenue/prepayments.
8. `lesson-08-working-capital.md` — Receivables, payables, inventory, and operating cash pressure.
   - Cash conversion cycle intuition; why growth can consume cash.
9. `lesson-09-capex-opex-depreciation.md` — Investment, expense, and spreading cost over time.
   - Capitalization, depreciation/amortization, asset lives, impairment intuition.

### Block D — Management use and client-discussion vocabulary

10. `lesson-10-budget-forecast-actuals.md` — Planning, forecasting, and variance.
    - Budget vs forecast vs actuals; run-rate; variance analysis.
11. `lesson-11-financial-kpis.md` — Margins, EBITDA, liquidity, leverage, and returns.
    - What each KPI tries to isolate; what it hides.
12. `lesson-12-management-reporting.md` — How finance is used for decisions.
    - Board/management packs, P&L ownership, cost centers, business partnering.

## Later bridge, not now

After the finance foundations are solid, create separate integration lessons under `integrated-cases/`, for example:

- `finance-to-reporting-models.md`
- `finance-dashboard-requirements.md`
- `financial-kpi-definitions.md`
- `monthly-reporting-automation.md`

## Sources used for terminology

- IFRS Foundation, *Conceptual Framework for Financial Reporting* page: used to align the elements of financial statements and the objective of financial reporting. <https://www.ifrs.org/issued-standards/list-of-standards/conceptual-framework/>
- IAS Plus summary of IAS 1, *Presentation of Financial Statements*: used to verify statement names, accrual basis, and complete set of financial statements under IFRS. <https://www.iasplus.com/en/standards/ias/ias1>
- OpenStax, *Principles of Accounting, Volume 1*: used as an accessible cross-check for accounting equation and introductory accounting terminology. <https://openstax.org/details/books/principles-financial-accounting>
