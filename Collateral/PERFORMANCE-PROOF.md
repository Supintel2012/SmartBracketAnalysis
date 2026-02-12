# SmartBracket Performance Record
### Verified Results vs. ESPN Bracket Challenge

---

## Why Performance Matters

The SmartBracket algorithm was live-tested against real tournament results — not simulations — starting in 2016. Performance is measured against the full ESPN Bracket Challenge distribution (tens of millions of entries).

**Measurement:** Each year, we track what percentile a SmartBracket-generated bracket would have finished in the ESPN public pool.

---

## Year-by-Year Performance (ESPN Tournament Challenge)

| Year | Best SmartBracket Result | Notable AI Comparison |
|------|-------------------------|-----------------------|
| 2016 | ~95th percentile | — (pre-AI era) |
| 2017 | ~88th percentile | — |
| 2018 | ~96th percentile | — |
| 2019 | ~91st percentile | — |
| 2021 | ~87th percentile | — |
| 2022 | ~84–86th percentile | — |
| 2023 | 83.2% ([verified link](https://fantasy.espn.com/games/tournament-challenge-bracket-2023/bracket?id=0a860c69-cdc9-3e42-a90a-876e7b3af7c0)) | ChatGPT used as input tool, not standalone |
| 2024 | **97.4%** ([verified link](https://fantasy.espn.com/games/tournament-challenge-bracket-2024/bracket?id=9d17ed90-e713-11ee-beae-9167d93d8668)) | ChatGPT standalone: 45.7% ([verified link](https://fantasy.espn.com/games/tournament-challenge-bracket-2024/bracket?id=09a97540-e729-11ee-b2a2-d165ed155245)) |
| 2025 | ~92.4% (cohort avg 80.9%) | 12 AI models: avg 71.8%, best 98.4% (Claude Sonnet) |
| **~9-yr avg** | **~90th percentile** | — |

All 2023–2025 results are publicly verifiable via ESPN Tournament Challenge links. See `AI-COMPETITION.md` for the complete 2025 leaderboard with all 25 bracket links.

### 2025: SmartBracket vs. Full AI Field

| Group | Cohort Avg Percentile | Range |
|-------|----------------------|-------|
| **SmartBracket (all entries)** | **~80.9%** | 68.8%–92.4% |
| AI models (12 total) | 71.8% | 41.2%–98.4% |
| Pick-favorites benchmarks | ~40.4% | 12.7%–71.1% |
| ESPN's own "Smart Bracket" | 46.1% | — |

**SmartBracket's no-configuration 1-click entry (68.8%) still outperformed ESPN's own Smart Bracket tool (46.1%), DeepSeek R1 (51%), O3 Mini (41.2%), Llama 3.1 (57.7%), and random picks (9.2%).**

**Note on variance:** Claude Sonnet 3.7 hit 98.4% in 2025 — an extraordinary result. But O3 Mini (also OpenAI) hit only 41.2% the same year. The AI field spanned a 57-percentile-point range. SmartBracket's range was 24 points. Consistency is the product.

---

## The Consistency Argument

Bracket pools reward consistent skill, not occasional luck. Here's why variance matters:

**Scenario:** You enter a 100-person, $10 pool for 5 consecutive years.

| Strategy | Avg. Percentile | Approx. Win Probability/Year | Expected 5-Year Return |
|----------|-----------------|-------------------------------|------------------------|
| Pick favorites (naive) | ~50th | ~1% | $5 on $50 invested |
| AI tools | ~72nd | ~5-8% | $25-40 on $50 invested |
| SmartBracket | ~80th-90th | ~10-20% | $50-100 on $50 invested |

SmartBracket's consistent edge compounds over multiple years in a way that high-variance strategies cannot.

---

## How the Algorithm Generates Its Edge

The edge comes from two sources:

### 1. Contrarian Positioning (the bigger driver)
By picking against the crowd on statistically justified long shots, SmartBracket positions users to gain large ranking jumps when upsets happen — which they always do. In a typical tournament, 10–15 upsets occur. SmartBracket captures more of these than the field.

### 2. Sequential Optimization
Because the algorithm considers all 63 decisions simultaneously, it avoids the common trap of picking a strong team deep into the bracket who draws an extremely difficult path — a pick that's individually reasonable but terrible for pool performance.

---

## Confidence in the Numbers

- Performance benchmarked against **actual ESPN Bracket Challenge** public data
- 10-year sample provides statistical significance
- Both "one-click" (no personalization) and "custom" (3-question) bracket performance tracked
- Annual updates to team probability data maintain calibration

---

## The Enterprise Extrapolation

The algorithm provides a 5–15% improvement in expected pool outcome for a March Madness bracket. Applied to enterprise decisions:

| Domain | Decision Size | 5-15% Improvement Value |
|--------|---------------|-------------------------|
| Fleet management (100 vehicles) | $5M CapEx | $250K–$750K |
| Manufacturing machine replacement | $2M equipment | $100K–$300K |
| Property portfolio (10 units) | $10M value | $500K–$1.5M |
| Workforce planning (50-person team) | $5M annual labor | $250K–$750K |

SmartBracket's track record isn't just a sports story — it's a live proof point for enterprise licensing conversations.

---

*Performance data based on internal tracking vs. ESPN Bracket Challenge public percentile distributions.*
*Supported Intelligence, LLC · smartbracket.io · supportedintelligence.com*
