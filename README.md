# 2026 March Madness Calcutta — Team Worrell

## Quick Start — GitHub Pages Deployment

### One-time setup (2 minutes):
```bash
# 1. Create a new repo on GitHub (e.g., "march-madness-2026")
#    Go to github.com → New Repository → name it → Public → Create

# 2. In your local folder containing these files, run:
git init
git add tournament_tracker.html calcutta_dashboard_2026.html 2026_calcutta_values.csv README.md
git commit -m "Initial tournament tracker and auction dashboard"
git remote add origin https://github.com/YOUR_USERNAME/march-madness-2026.git
git branch -M main
git push -u origin main

# 3. Enable GitHub Pages:
#    Go to repo Settings → Pages → Source: "Deploy from a branch" → Branch: main → Save
#    Your live URL: https://YOUR_USERNAME.github.io/march-madness-2026/tournament_tracker.html
```

### To update results after each round:
```bash
# Option A: Edit locally and push
git add tournament_tracker.html
git commit -m "Update R1 results"
git push

# Option B: Edit the file directly on GitHub.com (easiest for quick updates)
#    Navigate to the file → pencil icon → edit the data → commit
```

### Share with your group:
Send them the GitHub Pages URL. Works on phones. Bracket picks are stored locally per device.

---

## Context
- **Event:** 11th Annual March Madness Calcutta, hosted by Shravan Thadani & Cam Dunn
- **Auction:** March 18, 2026, 8:00 PM (online, Victory's Auction Pro)
- **Actual Pot Size:** $468,987 (below $625K estimate; fewer buying groups than prior years)
- **Our Spend:** $92,950 (19.8% of pot) across 13 teams
- **Overall Portfolio Premium:** -8.9% (bought below EV on average — positive EV portfolio)

## Our Teams (Team Worrell)
| Team | Region | Seed | Price Paid | EV @ Actual Pot | Premium | Key Notes |
|------|--------|------|-----------|-----------------|---------|-----------|
| Iowa State | Midwest | 2 | $24,000 | $23,768 | +1.0% | Our anchor. 6% champ odds. |
| Illinois | South | 3 | $19,100 | $20,066 | -4.8% | Hautlist. 4.3% champ. |
| Vanderbilt | South | 5 | $12,250 | $9,896 | +23.8% | Overpaid vs EV but deep run upside. |
| Alabama | Midwest | 4 | $8,000 | $11,872 | -32.6% | Steal. Strong E8 odds. |
| Wisconsin | West | 5 | $6,900 | $7,567 | -8.8% | Value. Solid R2 potential. |
| UCLA | East | 7 | $5,100 | $5,873 | -13.2% | Hautlist. Underpriced for seed. |
| Kentucky | Midwest | 7 | $3,900 | $5,311 | -26.6% | Steal. Name-brand discount. |
| Texas | West | 11 | $3,400 | $2,884 | +17.9% | Hautlist. Play-in winner (beat NC State). |
| Georgia | Midwest | 8 | $2,800 | $3,356 | -16.6% | Value. 8/9 game coin flip. |
| Ohio State | East | 8 | $2,500 | $4,151 | -39.8% | Steal of the auction. |
| Clemson | South | 8 | $2,000 | $2,786 | -28.2% | Value. Plays Iowa (not ours). |
| UCF | East | 10 | $2,000 | $1,935 | +3.4% | Fair price. Plays UCLA (ours). |
| Villanova | West | 8 | $1,000 | $2,611 | -61.7% | Absurd steal. 62% discount. |

## Payout Structure (at $468,987 pot)
| Round | % of Pot | Per-Team Payout | Teams Eligible |
|-------|----------|----------------|----------------|
| R1 Win | 0.75% | $3,517 | 32 |
| R2 Win | 1.25% | $5,862 | 16 |
| Sweet 16 Win | 2.50% | $11,725 | 8 |
| Elite 8 Win | 4.25% | $19,932 | 4 |
| Final Four Win | 5.50% | $25,794 | 2 |
| Championship | 7.00% | $32,829 | 1 |
| Biggest Blowout Loss (R1) | 1.00% | $4,690 | 1 |

## Pot History
| Year | Pot | YoY Growth | Buying Groups |
|------|-----|------------|--------------|
| 2022 | $225,000 | — | ~8 |
| 2023 | $270,700 | +20% | ~9 |
| 2024 | $368,500 | +36% | ~11 |
| 2025 | $505,700 | +37% | 11 |
| 2026 | $468,987 | -7% | ~9 |

## Files in This Folder
| File | Description |
|------|-------------|
| `tournament_tracker.html` | **Primary deliverable.** Live tournament tracker with viewing guide, interactive bracket, P&L tracking, and scenario analysis. Share this URL with your group. |
| `calcutta_dashboard_2026.html` | Pre-auction valuation dashboard with pot slider, historical backtesting, seed analysis, and live auction tracker. |
| `2026_calcutta_values.csv` | Team valuations at 9 pot sizes ($400K-$800K) with all 4 source values. |
| `.claude_config.md` | Initialization file for future Claude sessions — contains all context needed to resume work. |
| `README.md` | This file. |
| `internal work/` | Historical variability sheets (2023-2026) with round-by-round probabilities from 4 sources. |
| `auction results/` | Past auction PDFs from Victory's Auction Pro (2023-2025). |
| `tournament results/` | Past tournament outcome Excel files with full P&L by team/owner (2023-2025). |
| `examples/` | Example output formats from 2025 (price sheets at multiple pot sizes, teams to watch). |
| `rules/` | Auction rules email and 2025 tracker template. |

## Methodology
- **4-source composite:** KenPom, Massey Ratings, ESPN BPI, Silver Bulletin (Nate Silver)
- Round-by-round win probabilities averaged across sources
- Expected value = sum of (P(win round N) x payout for round N) across all rounds + blowout probability
- Historical analysis normalized to 2025 pot ($505,700) for cross-year comparison
- Auction implied pot = (price paid for team) / (team's % share of EV)

## Key Insights
- Market systematically overpays for 1-seeds and 2-seeds (brand premium / emotional bidding)
- Best value historically in seeds 4-7 where market underprices deep run potential
- 13-16 groups occasionally offer blowout bonus value (~$4,700 at this pot)
- Fewer buying groups = lower pot but also less competition for mid-tier teams = late-auction bargains
- This year's portfolio is positive EV (-8.9% average premium = bought below fair value)
- Break-even requires ~$93K in payouts which needs deep runs beyond R1 (R1 alone caps at ~$46K with all 13 winning)

## Tournament Schedule
- **R1:** March 19-20 | **R2:** March 21-22 | **Sweet 16:** March 26-27
- **Elite 8:** March 28-29 | **Final Four:** April 4 | **Championship:** April 6
