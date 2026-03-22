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

## Status After R1 Complete (Thu 3/19 + Fri 3/20)

- **Won R1 (7):** Vanderbilt 🔥, Illinois 🔥, Texas 🔥 (Thu) · Iowa St. 🔥, Alabama, UCLA 🔥, Kentucky (Fri)
- **Eliminated (6):** Wisconsin (82-83 vs High Point), Ohio St. (64-66 vs TCU), Georgia (77-102 vs Saint Louis), Clemson (61-67 vs Iowa), UCF (71-75 vs UCLA), Villanova (76-86 vs Utah State)
- **R1 Earnings:** 7 × $3,517 = $24,622 | Still need: $68,328 to break even
- **R2 Saturday (3 of our teams):** Vanderbilt vs Nebraska, Illinois vs VCU, Texas vs Gonzaga
- **R2 Sunday (4 of our teams, 6 games):** Kentucky vs Iowa St. (OURS vs OURS), Alabama vs Texas Tech, UCLA vs UConn
- **Teams remaining:** 7/13 | Haut List: 5/5

## Status After R2 Day 1 (Sat 3/21)

- **Won R2 (2):** Illinois 🔥 (76-55 vs VCU), Texas 🔥 (74-68 vs Gonzaga)
- **Eliminated in R2:** Vanderbilt 🔥 (72-74 vs Nebraska) — first Haut List casualty
- **S16 Locked:** Illinois vs Houston (Thu 3/26, HOU -2.5) · Texas vs Arkansas (TBD)
- **R2 Sunday (4 of our teams):** Kentucky vs Iowa St. 🔥 (ISU -4.5, OURS vs OURS) · Alabama vs Texas Tech (TTU -1.5) · UCLA 🔥 vs UConn (CONN -4.5)
- **Earnings to date:** 7 R1 ($24,622) + 2 R2 ($11,725) = $36,347 | Still need: $56,603
- **Teams remaining:** 6/13 | Haut List: 4/5

## Mid-Round Update Notes

When updating during a round that spans two days (e.g., R2 played Sat+Sun), there are key differences from a new-round update:

### What changes mid-round vs new round:
1. **CONFIRMED**: Add results only for completed games (e.g., Saturday R2 results). Do NOT add Sunday games until they're played.
2. **R2_SCHEDULE** (or equivalent round schedule): Already fully populated at round start. No new entries needed mid-round — just add CONFIRMED results as games finish.
3. **Viewing Guide**: Games that are now CONFIRMED should either be removed from the guide or shown with results. Remaining games (e.g., Sunday) still show as upcoming. The guide dynamically skips `CONFIRMED` games via the `if (CONFIRMED[key]) return;` check.
4. **PLAYS_TODAY / PLAYS_TOMORROW**: These auto-derive from R2_SCHEDULE + CONFIRMED. After Saturday games, Saturday teams move to ELIMINATED or R2_WINNERS. Sunday teams remain in PLAYS_TOMORROW (which becomes "today" on Sunday).
5. **Bracket**: Confirmed R2 results should lock in the bracket using the same confirmed-winner/confirmed-loser pattern used for R1. The bracket code already checks `CONFIRMED[r2Key]` — currently all R2 games render as interactive picks, but as results are added they'll lock automatically.
6. **Scenarios**: Update probabilities and descriptions after each day's results. Remove scenarios that are no longer possible.

### Checklist for mid-round update:
- [ ] Add completed games to `CONFIRMED` (same key format: `REGION-r2-INDEX`)
- [ ] Verify viewing guide correctly hides completed games
- [ ] Update subtitle timestamp
- [ ] Update scenario analysis text + probabilities
- [ ] Add change log entry
- [ ] Push to GitHub

### Checklist for new round update (e.g., transitioning from R2 → S16):
- [ ] All games from current round added to `CONFIRMED`
- [ ] Create new schedule object (e.g., `S16_SCHEDULE`)
- [ ] Update viewing guide `renderGuide()` to remove old round, add new round sections
- [ ] Update `PLAYS_TODAY` / `PLAYS_TOMORROW` logic to reference new schedule
- [ ] Add S16 confirmed-result rendering to bracket (similar pattern to R1/R2)
- [ ] Update portfolio status labels (e.g., "Won R2" → "R2 Today" → "S16 Saturday")
- [ ] Rewrite scenarios for remaining teams
- [ ] Update subtitle, change log, push to GitHub

## Change Log

| Date | Changes |
|------|---------|
| 3/19 (Day 1) | Initial results update: added `CONFIRMED` object with 16 R1 results, `R2_SCHEDULE` with 8 Saturday games + spreads, `HAUT_LIST` with fire emoji throughout, `ELIMINATED`/`R1_WINNERS`/`PLAYS_TODAY` derived sets, teams remaining counters, split viewing guide into R1 remaining + R2 sections, locked confirmed bracket games, updated scenario analysis, created UPDATE_PROCESS.md |
| 3/19 (patch) | Restored "Potential Winnings" column to portfolio table (driven by bracket picks). Net Balance = Won to Date + Potential - Price. Header counter shows Won + Potential + Net. Added hint text explaining potential winnings are from bracket predictions. |
| 3/19 (patch 2) | Added day headers ("FRIDAY, MARCH 20" / "SATURDAY, MARCH 21") inside the viewing guide game lists for clarity. |
| 3/21 (R2 prep) | Added 16 Friday R1 results to CONFIRMED (all 32 R1 games now locked). Added 8 Sunday R2 games to R2_SCHEDULE (now has all 16 R2 games). Removed R1 viewing guide section; replaced with R2 Day 1 (Sat 3/21) + R2 Day 2 (Sun 3/22) sections with filter toggles. Added R1 recap card. Updated PLAYS_TODAY/PLAYS_TOMORROW to derive from R2_SCHEDULE. Portfolio status now shows "R2 Today"/"R2 Sunday" instead of "Won R1"/"Plays Today". Added S16 lookahead notes to viewing guide (root for lower seed in adjacent game). Rewrote scenario analysis for 7 remaining teams with R2 matchup context. Updated cascadeRemove to respect confirmed R2 results. Added mid-round vs new-round update notes to UPDATE_PROCESS.md. |
| 3/21 (R2 Day 1 results) | Mid-round update: Added 8 Saturday R2 results to CONFIRMED (Illinois 76-55 VCU, Texas 74-68 Gonzaga, Vanderbilt 72-74 Nebraska, etc.). Replaced Saturday viewing guide section with R2 Day 1 recap card showing our 2W/1L results, S16 matchups locked (ILL vs HOU, TEX vs ARK). Updated Sunday spreads from ESPN odds (TTU -1.5 vs Alabama flipped from ALA -1.5, ARIZ -12.5 vs Utah State). Updated S16 lookahead to use confirmed R2 winners when partner game is done. Added R2_WINNERS derived set for portfolio status ("Won R2 ✓"). Portfolio status now shows "R2 Today" for Sunday games, "Won R2 ✓" for Saturday winners. Rewrote scenarios for 6 remaining teams post-Saturday. Updated Quick Math table. |
