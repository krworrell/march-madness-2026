# Tournament Tracker — Daily Update Process

> Use this file each day to guide the update of `tournament_tracker.html` after games are played.

## Step 1: Gather Results

1. **Go to ESPN scoreboard** for the game date:
   `https://www.espn.com/mens-college-basketball/scoreboard/_/date/YYYYMMDD/group/100`
2. **Extract scores** — use JS console or read the page:
   ```js
   // Run in ESPN scoreboard page console:
   const games = [];
   document.querySelectorAll('.ScoreboardScoreCell').forEach(cell => {
     const teams = cell.querySelectorAll('.ScoreCell__TeamName');
     const scores = cell.querySelectorAll('.ScoreCell__Score');
     if (teams.length >= 2 && scores.length >= 2) {
       games.push(teams[0]?.textContent + ' ' + scores[0]?.textContent + ' - ' + teams[1]?.textContent + ' ' + scores[1]?.textContent);
     }
   });
   console.log(games.join('\n'));
   ```
3. **Get next-round schedule + spreads** from the ESPN odds page:
   `https://www.espn.com/mens-college-basketball/odds/_/date/YYYYMMDD`
   Scroll to find the relevant day's games. Note: time, channel, and spread for each.

## Step 2: Update `CONFIRMED` Object

Add new entries for each completed game. Format:
```js
"REGION-rN-INDEX": {winner:"Team", loser:"Team", wScore:XX, lScore:XX},
// Add ot:true if overtime
```

### Key mapping for bracket indices:
Each region has 8 R1 games (index 0-7), 4 R2 games (index 0-3), 2 S16 games (0-1), 1 E8 game (0).

**R2 index mapping** (from R1 pairs):
- R2-0 = winners of R1 games [0, 1]
- R2-1 = winners of R1 games [2, 3]
- R2-2 = winners of R1 games [4, 5]
- R2-3 = winners of R1 games [6, 7]

**S16 index mapping** (from R2 pairs):
- S16-0 = winners of R2 games [0, 1]
- S16-1 = winners of R2 games [2, 3]

**E8**: winners of S16 [0, 1]

**Final Four**:
- ff-0 = SOUTH E8 winner vs EAST E8 winner
- ff-1 = WEST E8 winner vs MIDWEST E8 winner
- champ-0 = ff-0 winner vs ff-1 winner

## Step 3: Update `R2_SCHEDULE` (or add `S16_SCHEDULE`, etc.)

For the next round's scheduled games, add entries:
```js
"REGION-r2-INDEX": {t1:"Team1", t2:"Team2", date:"Sat 3/21", time:"7:45 PM", ch:"CBS", spread:"TEAM -X.5"},
```

As tournament progresses, add `S16_SCHEDULE`, `E8_SCHEDULE`, etc. with the same format. Update the viewing guide rendering to include these.

## Step 4: Update `ELIMINATED` Logic

The existing code auto-derives eliminated teams from `CONFIRMED`. No manual changes needed — just add results to `CONFIRMED`.

## Step 5: Update the Viewing Guide

The guide auto-generates from `BRACKET` data and `CONFIRMED` results:
- R1 games with confirmed results are automatically excluded
- New round schedule sections need to be added as rounds progress

**For Round 3+ updates**, add new sections in `renderGuide()` following the R2 pattern:
1. Create a schedule constant (e.g., `S16_SCHEDULE`)
2. Add a section divider and card in the guide
3. Loop through scheduled games with the same formatting

## Step 6: Portfolio Table — How It Works

The portfolio table has these columns:
- **Won to Date**: Actual confirmed payouts from `CONFIRMED` results (auto-calculated by `getActualWinnings()`)
- **Potential Winnings**: Additional payouts from bracket *predictions* (picks beyond confirmed rounds). Driven by `getTeamPayouts()` minus `getActualWinnings()` to avoid double-counting. Updates live as users click teams through in the Interactive Bracket tab. Shows in italics to indicate it's speculative.
- **Net Balance**: `Won to Date + Potential Winnings - Price Paid`. Reflects both real and predicted earnings.
- **Status**: Auto-derived — Eliminated / Won R1 / Plays Today / Alive

The header counter also shows Won + Potential + Net in real-time.

No manual changes needed for the portfolio — it auto-updates from `CONFIRMED` and the picks system.

## Step 7: Update Scenario Analysis

After each day, revise the scenarios in `renderScenarios()`:
- Update the "Day X recap" text
- Adjust scenario payouts based on confirmed wins/losses
- Recalculate probabilities based on remaining teams
- Update the "Quick Math" table

## Step 7: Update Portfolio Won-to-Date

The `getActualWinnings()` function auto-calculates from `CONFIRMED`. As you add R2, S16, etc. results, the payouts auto-update.

## Step 8: Push to GitHub

```bash
cd /path/to/repo
git add tournament_tracker.html
git commit -m "Update: Day N results (round info)"
git push
```

## Our 13 Teams (Reference)

| Team | Region | Seed | Price | Haut List |
|------|--------|------|-------|-----------|
| Iowa St. | Midwest | 2 | $24,000 | Yes |
| Illinois | South | 3 | $19,100 | Yes |
| Vanderbilt | South | 5 | $12,250 | Yes |
| Alabama | Midwest | 4 | $8,000 | No |
| Wisconsin | West | 5 | $6,900 | No |
| UCLA | East | 7 | $5,100 | Yes |
| Kentucky | Midwest | 7 | $3,900 | No |
| Texas | West | 11 | $3,400 | Yes |
| Georgia | Midwest | 8 | $2,800 | No |
| Ohio St. | East | 8 | $2,500 | No |
| Clemson | South | 8 | $2,000 | No |
| UCF | East | 10 | $2,000 | No |
| Villanova | West | 8 | $1,000 | No |

## Status After Day 1 (Thu 3/19)

- **Won R1:** Vanderbilt, Illinois, Texas (3 wins = $10,552)
- **Eliminated:** Wisconsin (lost to High Point 82-83), Ohio St. (lost to TCU 64-66), Georgia (lost to Saint Louis 77-102)
- **Play Friday:** Iowa St., Alabama, UCLA, Kentucky, Clemson, UCF, Villanova
- **Teams remaining:** 10/13 | Haut List: 5/5
