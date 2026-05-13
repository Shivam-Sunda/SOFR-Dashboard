# SOFR SR1 / SR3 Dashboard Documentation

## Overview

The SOFR SR1 / SR3 Dashboard is a professional Streamlit-based rates monitoring and futures pricing dashboard designed for tracking:

- SOFR fixings
- GC Repo rates
- ICAP projected rates
- SR1 (1-Month SOFR Futures)
- SR3 (3-Month SOFR Futures)
- Multi-scenario rate projections

The dashboard is designed to function as a live rates desk tool with automatic cloud-based data loading through Google Sheets and real-time calculation of futures settlement estimates.

---

# Core Features

## 1. Automatic Cloud Data Integration

The dashboard connects directly to Google Sheets and automatically loads:

- SOFR fixings
- ICAP projected fixings
- GC Repo rates

No manual Excel uploads are required after deployment.

Supported columns:

| Column | Description |
|---|---|
| date | Business date |
| sofr | Actual SOFR fixing |
| icap | ICAP projected fixing |
| gc | GC repo rate |

The dashboard refreshes directly from the online Google Sheet.

---

# 2. SR1 Futures Pricing Engine

The SR1 module calculates:

- Average SOFR over the contract month
- Implied SR1 futures price
- Multi-case scenario pricing

Formula:

SR1 Rate = arithmetic average of daily SOFR rates over all calendar days.

SR1 Price:

SR1 Price = 100 - SR1 Rate

Key characteristics:

- Includes weekends through forward-filled fixings
- Automatically handles month-start weekends
- Supports historical actuals and future projected fixings
- Uses business-day-only editable tables while preserving correct calendar accrual logic

---

# 3. SR3 Futures Pricing Engine

The SR3 module calculates:

- Compounded SOFR over the 3-month reference period
- Implied SR3 futures settlement price
- Multi-case scenario projections

Reference period:

- Starts: Third Wednesday of contract month
- Ends: Third Tuesday three months later
- Both dates inclusive

Compounding formula:

factor = 1 + (rate / 100) × (day_count / 360)

Final compounded rate:

Compounded Rate = ((compound_index - 1) × (360 / total_days)) × 100

SR3 Price:

SR3 Price = 100 - Compounded Rate

Important implementation details:

- Weekend carry handled through calendar-gap day counts
- Friday rows absorb weekend accrual automatically
- No weekend rows displayed
- Terminal accrual convention handled correctly
- CME-style business-day compounding structure

---

# 4. Multi-Case Scenario Framework

The dashboard supports:

- Case1
- Case2
- Case3
- Case4
- Case5

Each case acts as an independent forward fixing scenario.

Use cases:

- Funding stress scenarios
- Hawkish/dovish Fed paths
- Repo dislocations
- Event-driven rate shifts
- Internal desk forecasts

All cases calculate independent SR1 and SR3 settlement projections simultaneously.

---

# 5. Locked Historical Data

Historical business days are automatically locked.

Rules:

- Dates <= yesterday are non-editable
- Future dates remain editable
- Actual SOFR values always override projected values

This ensures:

- Historical integrity
- Stable settlement calculations
- Prevention of accidental back-editing

---

# 6. ICAP Integration

The dashboard supports ICAP projected rates.

Features:

- Separate ICAP display column
- ICAP settlement calculations
- One-click copy:

ICAP → Case1

This allows rapid baseline scenario generation.

---

# 7. GC Repo Monitoring

GC Repo rates are integrated alongside SOFR.

Features:

- Dedicated GC chart
- Overlay with actual SOFR
- Funding condition monitoring
- Repo/SOFR spread observation

Useful for:

- Funding stress analysis
- Quarter-end dynamics
- Treasury collateral pressure
- Balance sheet effects

---

# 8. Interactive Charts

The dashboard includes:

- SOFR fixing charts
- Case comparison charts
- GC Repo charts
- Historical actual fixing charts

Features:

- Interactive zooming
- Hover tooltips
- Dynamic scaling
- Dark-theme optimized visuals

---

# 9. Fast Fill Functionality

The sidebar includes a Fast Fill engine.

Users can:

- Select contract
- Select case
- Apply one projected rate across all remaining future business days

Useful for:

- Quick scenario building
- Fed-path assumptions
- Flat-forward projections

---

# 10. Shift Engine

The dashboard includes a basis-point shift engine.

Users can:

- Select contract
- Select case
- Shift all future projected fixings by X bps

Examples:

+5 bps hawkish repricing
-10 bps easing scenario

Only future dates are affected.

---

# 11. Past-Month Historical Mode

When a contract month is fully historical:

- Editable cases disappear
- Dashboard switches to actual-fixing-only mode
- Final settlement values are displayed

This creates a clean separation between:

- Live trading periods
- Historical settled contracts

---

# 12. Dark Trading Desk UI

The dashboard uses a professional dark theme optimized for trading desk usage.

Features:

- IBM Plex typography
- Low-eye-strain colors
- High-contrast pricing cards
- Professional chart styling
- Compact institutional layout

---

# Architecture

## Frontend

- Streamlit
- Altair charts
- Custom CSS styling

## Backend

- Python
- Pandas
- NumPy

## Cloud Data Source

- Google Sheets

## Deployment

- Streamlit Community Cloud

---

# Data Flow

Google Sheets → Streamlit Loader → Rate Engine → Futures Calculations → Charts & Tables

---

# File Structure

| File | Purpose |
|---|---|
| dash.py | Main dashboard application |
| requirements.txt | Python dependencies |
| Google Sheet | Cloud data source |

---

# Supported Workflows

## Daily Rates Monitoring

- Track SOFR
- Monitor repo conditions
- Observe ICAP forwards
- Estimate settlements

## Trading Scenario Analysis

- Shift future fixings
- Build multiple Fed paths
- Compare settlement outcomes

## Futures Pricing

- SR1 pricing
- SR3 compounding
- Settlement estimation

## Funding Analysis

- GC vs SOFR behavior
- Repo pressure analysis
- Liquidity condition monitoring

---

# Key Design Principles

The dashboard was built with emphasis on:

- Accurate futures math
- CME-style SR3 compounding
- Clean institutional UI
- Fast trader workflow
- Cloud-based accessibility
- Minimal operational overhead
- Real-time usability

---

# Future Enhancements

Potential future upgrades:

- Permanent cloud persistence for cases and notes
- Multi-user support
- Bloomberg/API integration
- Historical analytics
- Spread calculators
- Basis monitors
- Treasury futures integration
- Fed meeting scenario engine
- Export functionality
- Risk dashboards

---

# Summary

The SOFR SR1 / SR3 Dashboard is a professional cloud-deployed rates monitoring and futures pricing system designed for:

- SOFR futures monitoring
- Funding market analysis
- Scenario generation
- Settlement estimation
- Live desk workflows

It combines:

- automated cloud data ingestion,
- institutional pricing logic,
- scenario analysis,
- and interactive visualization

into a single streamlined dashboard environment.
