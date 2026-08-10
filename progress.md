# FinTrack — Personal Finance Tracker

## Project Architecture

```
fintrack/
├── index5.html            # Main app v5 (stable)
├── index6.html            # Main app v6 (dark mode, undo, tags, recurring)
├── index7.html            # Main app v7 (FINAL mobile-first feature-complete build)
├── test-react.html        # Minimal React CDN test
└── progress.md            # This file
```

> **index7.html is the final, feature-complete build** intended for wrapping into an Android APK (web-to-app).
> All new work goes here. index5/index6 kept as stable references.

## Data & Storage
- Entries/budgets/goals: localStorage key `fintrack_v6`
- PIN: localStorage key `fintrack_pin`
- Starred entries: `fintrack_starred`
- Loans: `fintrack_loans` · Net worth items: `fintrack_networth`
- Theme: `fintrack_theme` · PWA manifest injected as inline blob URL
- ⚠️ All data is **local only** — uninstalling the APK wipes everything. Export JSON backup before upgrading.

## ✅ Features in index7.html (v7)

### Core (from v5/v6)
- Dashboard: health score, summary cards, smart insights, cash/bank balances
- Income/Expense/Transfer tracking, daily/weekly/monthly views, advanced filters
- Budgets with rollover & alerts, Savings goals, Investment guidance (SIP, allocation, cards)
- Dark mode, Undo (10 steps), Tags, Search, Category management, Recurring transactions
- Export CSV/JSON, Import JSON, Custom SVG charts, Mobile-responsive

### NEW in v7
1. **🔐 PIN lock** — optional 4-digit PIN; Set/Unlock from header or Settings. Forgot PIN → reset ALL data.
2. **👁 Quick-glance hide** — eye toggle in header masks every amount app-wide (uses global `INR`/`sINR` hide flag).
3. **📊 Running balance** — each tracker entry shows cumulative balance after that entry.
4. **🗓 Spending heatmap** — dashboard calendar grid colored by daily spend intensity (today marked).
5. **📋 Periodic report** — copies a plain-text monthly summary to clipboard (Data tab).
6. **💳 Loans / Debt tracker** — add/edit loans; auto EMI calculator (P×r×(1+r)ⁿ); next-EMI date with due banner + "Due" badge.
7. **🏦 Net worth tracker** — Assets (PPF, MF, FD, SGB, NPS, Real Estate, etc.) & Liabilities (loans, cards) via dropdown; SIP recurring + projected value; SIP-due reminders.
8. **🧾 Tax calculator (India)** — New/Old regime, 80C/80D/other deductions, cess, effective rate.
9. **👆 Swipe-to-delete** — swipe left on an entry row to reveal Delete (mobile gesture).
10. **⭐ Favourite entries** — star icon pins entries with gold border.
11. **📦 Batch entry** — paste `Amount,Category,Note,Mode` lines; bulk add.
12. **🔔 Auto-backup reminder** — toast on 1st of each month.
13. **📱 PWA manifest** — inline blob manifest for standalone APK behavior.

### Tabs (10, scrollable on mobile)
Dashboard · Tracker · Analytics · Budget · Goals · Loans · NW · Invest · Tax · Settings

## 🐛 Bug Fixes Applied (v7)
- **Babel parse error**: `scale` SVG icon had two `<path>` without fragment wrapper → wrapped in `<>…</>`.
- **PIN set broken**: header "Set PIN" button missing `setLocked(true)` → added so lock overlay appears.
- **PIN set cancel**: added ✕ close button on PIN setup overlay (only when no PIN set yet).
- **NW tab crash**: `React.useState` inside conditional render function violated rules of hooks → moved `editNwId`/`editLoanId` state to App level.
- **Forgot PIN reset silent**: confirmation popup `zIndex:9999` was behind PIN overlay `zIndex:99999` → raised confirm to `zIndex:100000`.
- **Quick-glance partial**: `INR`/`sINR` now honour global `window.__hideAmt` so ALL amounts hide.
- **NW dropdown not bound to type**: dropdown showed all asset+liability options regardless of toggle, and "Other Liability" appeared twice (duplicate). Now filtered by selected type; switching toggle clears the name; "+ Other (custom name)" reveals a text input. Option arrays trimmed of embedded "Other" entries.
- **Dead `renderRecurring` removed**: orphaned function (no tab rendered it) contained hooks → eliminated hook-smell.
- **Unified reminders**: `reminders` useMemo at App level computes SIP-due (±3d of contribution day, month-boundary aware) and EMI-due (≤7d of nextDate) → shown as Dashboard "Due Soon" banner, per-tab banners (Loans/NW), and a toast on app open. Global `calcEMI` helper added.
- **`calcEMI` was local inside renderLoans** (inaccessible from App-level code) but already existed as global `const` at top of file → removed the local copy; all callers use the global.
- Added `bell` SVG icon for reminder UI.

## NEW in v7 (continued)
- **SIP separated from one-time assets**: NW tab now has two distinct sections — "Assets & Liabilities" (one-time items, no SIP fields, edit/pencil icon) and "SIP / Recurring Investments" (separate form with monthly + rate + startDate + current value). Two `showNwForm`/`showSipForm` modals. SIP projected value still counted toward total assets.
- **Custom-name for assets/SIPs**: explicit `custom` boolean on each form; select uses `__custom` sentinel value reliably; text input appears only when custom selected.
- **Auto-payment detection** (`checkAutoPayment`): called in `saveForm` for new expenses — matches amount to EMI (±2%, within -3/+7d window) → advances `nextDate`, increments `paid`; matches to SIP monthly amount (±2%, within ±3d) → sets `lastPaid` so reminder skips that cycle.
- **Reminders skip paid SIP**: SIP reminders compute due date per cycle; if `item.lastPaid` shares the same month-year, the reminder is excluded that cycle.
- **Splash screen**: plain HTML/CSS overlay (`#splash`) in `<body>` shows instantly before React loads; hidden by `useEffect` on mount (fade-out after 100ms). Improves perceived load time.
- **Edit buttons in NW lists**: pencil icon added to all asset/liability/SIP cards alongside delete for inline editing.

## NEW in v7 (continued)
- **Loan "Completed" toggle**: each active loan card has a green checkmark button to mark the loan as completed (with dimmed card + strikethrough + "Completed" section) and a reopen button to restore it. Completed loans are excluded from EMI reminders (`!l.completed` in both App-level reminders and `checkAutoPayment`) and shown in a separate faded section at the bottom.
- **SIP "Closed" toggle**: each active SIP card has a green checkmark button to mark it as closed (dimmed, strikethrough, separate "Closed SIPs" section) with a reopen button. Closed SIPs are excluded from projection (`calcSIPValue` not applied — only current `i.value` counted), excluded from SIP reminders (`!i.closed`), and skipped in auto-payment detection.

## Verified
- `calcEMI` is a single global `const` (line ~47); no redeclaration.
- `checkAutoPayment` uses `loans`, `netWorthItems`, `saveLoans`, `saveNetWorth`, `calcEMI` — all in App closure or global.
- Splash overlay shows/hides correctly via `classList.add("hide")`.
- Braces/parens balanced; no hooks inside render functions; loads as standalone APK via inline manifest.

## 🔜 Not planned (out of scope for local-only app)
- Cloud sync / server backend
- PDF export (use CSV/JSON + share instead)
- Multi-currency
