# FinTrack — Personal Finance Tracker

## Project Architecture

```
fintrack/
├── index5.html            # Main app (v5) - React SPA
├── test-experiments.html  # Test/experiment playground
└── progress.md            # This file
```

## Current Status (v5)

### ✅ Completed Features
- Dashboard with financial health score, monthly summary, smart insights
- Income/Expense tracking with category breakdown
- Cash & Bank/UPI balance tracking with mode badge on each entry
- Transfer between Bank/UPI and Cash (bank withdrawal/deposit)
- Monthly, weekly (custom week picker), daily (custom date picker) views
- Advanced filtering (type, payment mode, category)
- Budget management with limit alerts
- Savings goals tracker
- Investment guidance (SIP calc, asset allocation, instrument cards, emergency fund)
- Export CSV/JSON, Import JSON backup
- Custom SVG chart system (bar, horizontal bar, line, donut/pie)
- Mobile-responsive with bottom tab navigation
- Persistent localStorage

### 🔬 Bug Fixes (Current Session)
1. **Edit mode preserves payment mode** — openEdit now copies `mode`, `fromMode`, `toMode` from entry
2. **Analytics shows all expense categories** — removed `.slice(0,6)` limit
3. **Transfer feature added** — new entry type "transfer" with from/to mode selectors; auto-creates paired transferOut/transferIn entries, updates balances correctly
4. **Daily view has date picker** — select any date; Weekly view has week-start date picker

### 🔜 Suggested Next Features
- Dark mode toggle
- Recurring transactions (auto-add monthly/ weekly entries)
- PWA offline support (service worker + manifest)
- Multi-currency support
- Cloud backup / sync (Firebase or localStorage → GitHub)
- Pie chart color legend
- Export to PDF
- Tag/label system for entries
- Search/full-text filter on notes
- Undo last action
- Category management (add/rename/delete categories)
- Budget rollover (unused budget carries to next month)
- Bill reminders / due date tracking
- Net worth tracker (include assets like property, investments, loans)
