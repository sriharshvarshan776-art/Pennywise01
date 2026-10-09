# PennyWise — Smart Personal Finance Tracker 🪙
> Designed for College Students & Young Professionals. Built with React 19, TypeScript, Tailwind CSS v4, Lucide Icons, and Supabase Backend Integration (with Model Context Protocol - MCP support).

---

## ⚡ Supabase Backend & MCP Architecture

PennyWise includes a complete production-grade backend setup using **Supabase** and the **Official Supabase Model Context Protocol (MCP) Server**:

1. **Authentication (Supabase Auth)**:
   - **Email & Password**: Secure user registration and sign-in with metadata (`full_name`, `persona`, `monthly_income_goal`).
   - **Google OAuth**: One-click sign-in via Google Cloud Provider credentials.
   - **Session Sync**: Automatic token refresh, session persistence, and real-time auth state listener (`onAuthStateChange`).
   - **Password Recovery**: Secure password reset flow with redirect to client.
2. **PostgreSQL Database Schema (`supabase_schema.sql`)**:
   - `profiles`: Linked 1:1 with `auth.users` with automated trigger function `handle_new_user()`.
   - `transactions`: Complete transaction ledger with user isolation.
   - `budgets`: Category ceilings per user.
   - **Row Level Security (RLS)**: Enforces strict data isolation so users can only view, insert, update, and delete their own records.
3. **Official Supabase MCP Server (`@supabase/mcp-server-supabase`)**:
   - Manifests configured in `.agents/mcp_config.json` and `mcp_config.json`.
   - Run directly with:
     ```bash
     npx -y @supabase/mcp-server-supabase@latest --access-token YOUR_SUPABASE_TOKEN
     ```
4. **Interactive In-App Supabase Setup**:
   - Click the **`Supabase (MCP)`** button in the top banner or Auth screen to paste your Project URL and Anon Key directly, copy the SQL schema, or test the connection!


## 🎯 Executive Summary & Problem Statement

Managing personal finances is one of the steepest learning curves for **college students** (juggling monthly allowances, cafeteria meals, bus passes, and course materials) and **young professionals** (managing first salaries, apartment rent, subscriptions, and lifestyle inflation).

Existing budgeting tools are either:
1. **Overly complicated and intimidating**, requiring bank account OAuth integrations, complex double-entry accounting, or intrusive subscriptions.
2. **Confusing with critical financial metrics**, notoriously confusing **Available Balance** (real cash in hand) with **Remaining Budget** (planned expense limit headroom).

**PennyWise** delivers a minimal, trustworthy, fast, and accessible fintech web application that organizes transactions, visualizes monthly spending trajectories, provides contextual budget alerts before overspending occurs, and offers actionable financial insights.

---

## 💎 Design Direction & System Tokens

PennyWise follows a clean, modern aesthetic inspired by high-end financial institutions (Stripe, Mercury, Monzo):

- **Palette**:
  - **Canvas Background**: `#F8FAFC` (Calm, soft off-white surface minimizing eye fatigue).
  - **Primary Text & Headings**: `#0F172A` (Deep Navy for high-contrast legibility).
  - **Income & Positive Indicators**: `#10B981` / `#059669` (Vibrant Emerald Green).
  - **Expenses & Outflows**: `#EF4444` / `#F43F5E` (Coral / Muted Red).
  - **Interactive Elements & CTAs**: `#4F46E5` / `#6366F1` (Indigo / Electric Violet).
  - **Warning Thresholds (80%–99%)**: `#F59E0B` (Amber).
- **Typography Scale**: Built on **Inter** and **Plus Jakarta Sans**, with monospaced figures for financial numbers.
- **Micro-interactions**: Subtle borders (`border-slate-200/80`), gentle elevation shadows (`shadow-[0_1px_3px_0_rgba(15,23,42,0.03)]`), rounded cards (`rounded-2xl`), and immediate reactive feedback.
- **Accessibility (WCAG AA)**: Status is never communicated by color alone. Every threshold pairs color with explicit text labels ("On Track", "Near Limit", "Exceeded") and semantic icons (`CheckCircle2`, `AlertTriangle`, `AlertOctagon`).

---

## 📊 Realistic Baseline Scenario (Exact Prompt Figures)

PennyWise initializes with authentic, mathematically coherent financial figures:

| Metric | Amount | UX Clarification |
| :--- | :--- | :--- |
| **Total Monthly Income** | **₹25,000** | Direct deposit salary / allowance |
| **Total Expenses** | **₹14,500** | Sum of 8 categorized expense records |
| **Available Balance** | **₹10,500** | **Real liquid cash** = Income (₹25,000) − Expenses (₹14,500) |
| **Monthly Expense Budget** | **₹18,000** | Planned monthly spending ceiling |
| **Budget Remaining** | **₹3,500** | **Spending headroom** = Budget (₹18,000) − Expenses (₹14,500) |
| **Budget Usage** | **80.6%** | `14,500 / 18,000 = 80.55%` (Triggers contextual Warning Alert) |

> 💡 **Core Fintech Rule**: Available Balance (`₹10,500`) is **NOT** remaining budget (`₹3,500`). Available balance represents money in the bank; remaining budget represents how much you are still allowed to spend this month before exceeding your cap.

---

## 🖥️ Screen-by-Screen Walkthrough

### Screen 1 — Authentication
- Toggle between **Sign In** and **Create Account**.
- Brand value proposition: *"Smart personal finance tracker built for college students & young professionals."*
- Password input with secure visibility eye toggle.
- **Forgot Password Modal**: Secure email reset link flow with instant feedback.
- **⚡ Quick Hackathon Demo Logins**:
  - `Student (₹25k)`: Instantly loads student scenario with allowance and university categories.
  - `Pro (₹55k)`: Instantly loads young professional scenario with tech salary, rent, and subscriptions.
- Secondary Google Sign-In button (styled as secondary outline).

### Screen 2 — Main Dashboard
- **Header**: Dynamic greeting (*"Good morning, Alex"*), Month selector (`October 2026`), Notification center with unread pill, and Profile avatar.
- **Contextual Budget Alert Banner**:
  - *"You have used 80.6% of your monthly budget. ₹3,500 remaining before reaching your ₹18,000 limit."*
  - Visual progress bar and one-click *"Review Budgets"* action.
- **4 Primary Summary Cards**: Total Income (`₹25,000`), Total Expenses (`₹14,500`), Available Balance (`₹10,500`), and Monthly Budget Remaining (`₹3,500`). Includes explanatory tooltips.
- **Interactive SVG Charts**:
  - **Daily Spending Line Chart**: Day-by-day trajectory across October with interactive hover tooltips and daily allowance benchmark line (`₹580/day`).
  - **Category Donut Chart**: Hoverable donut ring with center total and detailed category breakdown.
- **Recent Transactions Table**: Shows latest records with category badges, payment methods (`UPI`, `Card`, `Cash`, `Bank Transfer`), and amount styling.

### Screen 3 — Transactions Management
- Real-time **fuzzy search** across merchant titles, notes, and reference IDs.
- Comprehensive filters: **Category**, **Income/Expense Type**, and **Payment Method**.
- Sorting options: Newest first, Oldest first, Highest amount, Lowest amount, Title A-Z.
- **Filtered Summary Bar**: Shows real-time aggregated Inflow, Outflow, and Net Cashflow for whatever filters are active.
- Table Actions:
  - **View Details Modal**: Opens complete metadata (reference ID, timestamp, notes, tags).
  - **Edit Modal**: Update amounts or categories.
  - **Delete Modal**: Safe confirmation dialog.
- **Export to CSV**: Instant one-click download for offline spreadsheets.
- **Empty State**: Friendly graphics and one-click filter reset.

### Screen 4 — Add / Edit Transaction
- Accessible modal with keyboard shortcut support (`+` key).
- Segmented toggle: `🔴 Expense (Outflow)` vs `🟢 Income (Inflow)`.
- Currency formatted amount input with quick preset amount pills.
- Visual category grid (Food, Transport, Education, Shopping, Entertainment, Bills, Healthcare, Other).
- Payment method selector (`UPI`, `Card`, `Cash`, `Bank Transfer`).
- **Live Preview Card**: Shows an interactive preview of how the transaction row will look before saving.
- Required field validation and positive number checks.

### Screen 5 — Budget Management
- **Overall Spending Budget Card**: Total spent vs ₹18,000 cap with 80% warning marker.
- **Category Budget Grid**:
  - Food: ₹4,200 / ₹4,000 (**Exceeded 105.0% 🚨**)
  - Transportation: ₹1,650 / ₹2,000 (**Warning 82.5% ⚠️**)
  - Education: ₹1,999 / ₹3,000 (**Normal 66.6% ✅**)
  - Shopping: ₹2,100 / ₹2,500 (**Warning 84.0% ⚠️**)
  - Entertainment: ₹1,250 / ₹1,500 (**Warning 83.3% ⚠️**)
  - Bills: ₹2,300 / ₹3,000 (**Normal 76.7% ✅**)
  - Healthcare: ₹500 / ₹1,000 (**Normal 50.0% ✅**)
  - Other: ₹501 / ₹1,000 (**Normal 50.1% ✅**)
- **Edit Budget Limits Modal**: Customize overall or category-specific allocations in real time.

### Screen 6 — Reports & Insights
- **Month-over-Month Comparison**: September (`₹13,200`) vs October (`₹14,500`) = `+₹1,300 (+9.8%)`.
- **Key KPIs**: Top Category (Food: `29.0%`), Average Daily Spend (`₹467.74/day`), Savings Rate (`42.0%`).
- **Actionable Financial Insights**:
  - *"Your food expenses increased by 12% compared with the previous month."*
  - *"Shopping accounts for 14.5% of your total expenses — well within target."*
  - *"Your current spending is approaching your monthly budget (80.6% used)."*
- **50 / 30 / 20 Rule Analysis**: Compares actual expenditure with the gold-standard benchmark (Needs 50%, Wants 30%, Savings 20%).
- **Print / PDF Report**: Clean printer-optimized stylesheet.

### Screen 7 — Settings & Preferences
- Profile Information (Name, Email, Student/Pro persona switcher, Avatar).
- Currency Preference (Default: `INR ₹`, `USD $`, `EUR €`, `GBP £`).
- Theme Switcher (Light mode default, Dark mode, System).
- Security & Password change forms with mock 2FA switch.
- Data Management: Export full JSON backup, Reset to prompt baseline, Clear all data.

---

## 🛠️ Technology Stack & Architecture

- **Framework**: React 19 + TypeScript (Strict typing, zero errors).
- **Build Tool**: Vite 8.3 (Blazing 1.5s builds, instant HMR).
- **Styling**: Tailwind CSS v4 (`@tailwindcss/vite`), CSS variables, Inter font.
- **Icons**: Lucide React.
- **State Management**: React Context (`FinanceContext`) with persistent LocalStorage synchronization.
- **Data Visualizations**: Custom, accessible SVG charting components (`ExpenseTrendChart`, `CategoryDonutChart`, `MonthlyComparisonBarChart`).

---

## 🚀 Running the Project Locally

```bash
# 1. Install dependencies
npm install

# 2. Start Vite development server
npm run dev

# 3. Create production build
npm run build

# 4. Preview production build
npm run preview
```

---

## 🏆 Hackathon Presentation Features

1. **Top Quick-Control Banner**: Reviewers can switch between Student and Pro personas, open the Design System Token Explorer, or reset data to original prompt numbers at any time.
2. **Design System Token Explorer**: Accessible directly from the sidebar or header (`Tokens` button), showing live color swatches (with click-to-copy HEX codes), typography scales, and component specs.
3. **Realistic Financial Logic**: Every calculation is mathematically verified and reactive.
