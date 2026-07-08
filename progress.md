# FinTrack — Personal Finance Tracker

## Project Architecture

```
fintrack/
├── index5.html            # Main app v5 (stable)
├── index6.html            # Main app v6 (new features)
├── test-experiments.html  # Test/experiment playground
└── progress.md            # This file
```

## Current Status (v6)

### ✅ Features (v6 adds on v5)
- **Dark mode** toggle with CSS variables, persisted to localStorage
- **Undo** — last 10 state changes can be undone via header button
- **Search** entries by note or tag text
- **Tags** — comma-separated tags on each entry, searchable
- **Category management** — add/rename/delete income & expense categories in Settings tab
- **Budget rollover** — unused budget from last month carries forward
- **Recurring transactions** — daily/weekly/monthly auto-generated entries; managed in Auto tab
- **Settings tab** — manage categories
- **Auto tab** — manage recurring entries with pause/resume

### v5 Features (carried forward)
- Dashboard with financial health score, summary, smart insights
- Income/Expense tracking with category breakdown
- Cash & Bank/UPI balance tracking with mode badge
- Transfer between Bank/UPI and Cash
- Daily (date picker), Weekly (week picker), Monthly views
- Advanced filtering (type, payment mode, category, search)
- Budget management with rollover & alerts
- Savings goals tracker
- Investment guidance (SIP calc, asset allocation, instrument cards, emergency fund)
- Export CSV/JSON, Import JSON backup
- Custom SVG chart system
- Mobile-responsive

### 🔜 Future Ideas
- PWA offline support (service worker + manifest)
- Multi-currency support
- Cloud backup / sync
- Export to PDF
- Bill reminders / due date tracking
- Net worth tracker
- Recurring auto-pause when balance is low
