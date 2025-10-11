# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Kertos Growth Model - A single-page HTML/CSS/JavaScript calculator for Q4 2024 marketing spend and pipeline projections. The application models marketing funnel performance across multiple channels (Google Ads, LinkedIn, Retargeting, Content/SEO) and calculates key metrics like CAC, LTV, ARR, and ROI.

## Architecture

**Single-File Application**: `model/index.html` contains all HTML structure, CSS styles, and JavaScript logic in one file (~830 lines).

### Key Components

1. **Channel Configuration** (lines 509-542): Each channel has specific metrics:
   - `cpl`: Cost per lead (€)
   - `cr1`, `cr2`, `cr3`: Conversion rates (Lead→MQL, MQL→SQL, SQL→Customer)
   - `avgDays`: Average sales cycle length in days

2. **Core Calculation Logic** (lines 583-800):
   - Calculates separate funnels for each channel using channel-specific conversion rates
   - Aggregates paid channels, applies blended rates to organic/reactivation
   - Multiplies monthly inputs by 3 to generate quarterly (Q4) totals
   - Splits revenue impact between 2024 and 2025 based on sales cycle timing

3. **Budget Constraints**:
   - Retargeting: Max 10% of Google + LinkedIn combined (`validateRetargeting()`)
   - Content/SEO: Hard cap at €5,000/month (`validateSEO()`)

4. **Revenue Year Split** (lines 664-715):
   - Uses 92-day Q4 period (Oct-Dec 2024)
   - Calculates which % of leads close in 2024 vs 2025 based on `avgDays`
   - Formula: `2024% = max(0, (92 - avgDays) / 92)`

## How to Use

**Open the calculator**: Simply open `model/index.html` in any modern web browser. No build step, server, or dependencies required.

**Modify channel metrics**: Edit the `channels` object (lines 509-542) to update CPL or conversion rates.

**Adjust defaults**: Default input values are set in the HTML `value` attributes (lines 288-340).

## Development Notes

- No version control is currently initialized (not a git repository)
- All calculations trigger on `oninput` events for real-time updates
- Product mix defaults to 90% ISO/GDPR (€8k ARPU) and 10% NIS2 (€20k ARPU)
- LTV calculated as `ARPU × 2` (2-year customer lifetime assumption)
