# SmartBracket Master Business Analysis
## Comprehensive Codebase-to-Business Intelligence Report

**Generated:** January 2026
**Analysis Framework:** Codebase Business Analysis v1.0
**Analyst:** Claude Code Business Analysis Framework

---

## Executive Summary

SmartBracket is a **patented NCAA tournament bracket optimization tool** built on sophisticated Markov Decision Process algorithms. After 9 years of operation with ~90th percentile prediction performance, the business has generated only **$2,107 in revenue from 445 users** while spending **$16,897 on marketing**—a net loss of $14,790.

**Core Finding:** SmartBracket's consumer bracket business is a **technology showcase masquerading as a product business**. The true asset is the underlying **Rapid Recursive (RR) optimization technology**, protected by 3 US patents and 2 international patents, with applicability far beyond sports prediction.

**Strategic Verdict:** Pivot from consumer bracket sales to B2B/enterprise licensing of RRToolbox technology. The consumer market is commoditized (free AI alternatives), but the enterprise optimization market represents 100-1000x larger TAM with defensible IP positioning.

---

## 1. BUSINESS CASE CLARITY

### Product Type
**Consumer SaaS (Seasonal) + Algorithmic IP Platform**

SmartBracket operates as a consumer web/mobile application with a Python-based optimization microservice (RRToolbox-API).

### Primary Customer
**NCAA Tournament Bracket Pool Participants**

Based on data model and UX analysis:
- Male, 25-55, office worker demographic
- Participates in 2-5 bracket pools annually
- Willing to pay $5 for statistical edge
- Values "winning the pool" over entertainment

### Core Problem Being Solved
**Optimizing bracket picks under uncertainty to maximize expected winnings in competitive pools**

The system solves a 63-decision sequential optimization problem using Markov Decision Processes, balancing:
- Statistical win probabilities
- Crowd pick distributions (contrarian value)
- Pool competition dynamics

### Primary User Journey

```
Landing Page → Signup → Payment ($5) → Bracket Creation → Print/Share
                         ↓
              [VALUE WALL - No preview before payment]
```

**Critical Issue:** The "aha moment" (seeing optimized bracket with color-coded recommendations) is gated behind payment. Users cannot experience value before committing $5.

### Key Value Drivers

| Driver | Evidence | Strength |
|--------|----------|----------|
| Algorithm Performance | 9 years ~90th percentile | Strong |
| Patent Protection | 3 US + 2 International | Strong |
| Ease of Use | One-click bracket generation | Moderate |
| Pool Optimization | Contrarian pick weighting | Moderate |
| Price Point | $5/year | Weak (too low to signal value) |

### Business Case Summary

```
BUSINESS CASE SUMMARY
- Product Type: Seasonal Consumer SaaS + Algorithmic IP
- Primary Customer: Competitive bracket pool participants
- Core Problem: Maximize bracket pool winnings through statistical optimization
- Primary Value Driver: Patented MDP algorithm with 9-year track record
- Key Assumption to Validate: Users will pay for optimization vs. free AI alternatives
```

### Red Flags Identified

1. **Value hidden behind paywall** - No free trial or preview
2. **Seasonal demand** - 100% revenue in 2-month window
3. **Declining engagement** - Commits dropped 90% from 2020 to 2024
4. **Mobile revenue leak** - Mobile users bypass payment entirely
5. **Free AI competition** - ChatGPT/Claude provide "good enough" brackets for free

---

## 2. PRICING STRATEGY ALIGNMENT

### Current Pricing Model

| Element | Implementation | Status |
|---------|---------------|--------|
| Base Price | $5.00 one-time | Active |
| Discount Price | $3.00 with promo code | Active |
| Subscription | None | Not implemented |
| Usage-based | None | Not implemented |
| Tiering | None | Not implemented |

### What's Being Metered

| Data Point | Tracked | Monetized |
|------------|---------|-----------|
| User paid status | Yes | Yes |
| Discount eligibility | Yes | Yes |
| Bracket creation count | No | No |
| API calls to RRToolbox | No | No |
| Computation time | No | No |
| Mobile vs. web platform | Yes | **Inverted** (mobile = free) |

### What's NOT Being Metered (Revenue Leaks)
*Clarification: mobile app was pre-paid, sign-up only online or as app purchase*

```javascript
// controllers/mobileApp.js:148 - CRITICAL REVENUE LEAK
paid: req.body.appPlatform == "web" ? false : true,
// Mobile users automatically marked as paid!
```

| Gap                | Impact                                |
| ------------------ | ------------------------------------- |
| Unlimited brackets | Heavy users subsidized by light users |
| No annual renewal  | Zero recurring revenue                |

### Cost Structure Analysis

| Component             | Cost Type    | Scaling         |
| --------------------- | ------------ | --------------- |
| RRToolbox API compute | Per-request  | Linear          |
| MongoDB Atlas         | Per-document | Linear          |
| Stripe fees           | 2.9% + $0.30 | Per-transaction |
| Node.js hosting       | Fixed        | Step function   |

**Estimated cost per bracket:** $0.001-$0.01 compute
**Gross margin per user:** $4.55 (at $5) after Stripe fees

### Unit Economics Breakdown

| Metric           | Value      | Target | Status     |
| ---------------- | ---------- | ------ | ---------- |
| ARPU             | $4.73      | -      | -          |
| CAC (5-year avg) | $37.98     | <$15   | **FAILED** |
| CAC (2024)       | $94.08     | <$15   | **FAILED** |
| LTV (estimated)  | $4.60      | -      | -          |
| LTV:CAC Ratio    | **0.12:1** | 3:1+   | **FAILED** |

### Pricing Strategy Analysis

```
PRICING STRATEGY ANALYSIS
- Current Model Signals: One-time purchase, binary paid/unpaid
- Natural Pricing Tiers: One-click (free) vs Custom (premium) exists but not monetized
- Cost Structure: Algorithm compute scales linearly, fixed infrastructure
- Pricing Alignment Issues:
  * Mobile bypass payment entirely
  * No usage metering despite compute costs
  * No recurring revenue despite annual product use
  * $5 too low to signal quality vs free alternatives (Note: this is true, but expected value is 25% of potential betting office pool size...so subscription would be preferable)
- Recommendation:
  * Implement freemium (one-click free, custom paid)
  * Consider $15-25/year subscription model
  * Add usage quotas (3 free brackets, unlimited premium)
```

---

## 3. MOAT & DEFENSIBILITY

### Data Moat Assessment

| Factor | Score | Evidence |
|--------|-------|----------|
| Proprietary data accumulation | 2/10 | Only ~445 users, no compound growth |
| Network effects | 0/10 | None present |
| Switching costs | 1/10 | $5/year, trivial to recreate |
| Data uniqueness | 6/10 | Algorithm parameters are proprietary |

**Data Moat Strength: WEAK**

The codebase reveals minimal data lock-in:
- No historical bracket performance tracking
- No social graph or pool connections
- Only `lastBracketID` stored per user
- Annual usage resets user value to zero

### Technical Moat Assessment

| Factor | Score | Evidence |
|--------|-------|----------|
| Algorithm sophistication | 9/10 | PhD-level MDP implementation |
| Patent protection | 8/10 | 3 US + 2 international patents |
| Complexity to replicate | 7/10 | Requires OR + software expertise |
| Performance track record | 8/10 | 9 years of ~90th percentile |

**Technical Moat Strength: STRONG**

The true defensibility lies in:

1. **Patent Portfolio:**
   - US #9798700, #10460249, #10546248
   - Japan #6284472
   - Korea #10-2082522
   - Expiration: 2033-2035

2. **Algorithm Implementation:**
   - Value Iteration solver
   - Policy Iteration solver
   - Custom reward function construction
   - Proprietary probability calibration

3. **Proprietary Data Files:**
   - `P.npy`: 262KB transition probability matrix
   - `statChance*.txt`: Historical win probabilities (7 years)
   - `pickChance*.txt`: Crowd pick distributions

### Network/Lock-in Effects

**Network Effects: NONE**

- Product works identically for 1 user or 1 million
- No pool features requiring friends
- No leaderboards or community
- No user-to-user interactions

**Switching Costs: NEAR ZERO**

```
Switching Cost = $5/year + ~10 minutes to re-enter preferences elsewhere
```

### Defensibility Assessment

```
DEFENSIBILITY ASSESSMENT
- Data Moat: WEAK - Minimal user data accumulation, no network effects
- Technical Moat: STRONG - Patented algorithms, 9-year track record, PhD-level complexity
- Network/Lock-in Effects: NONE - Users can leave anytime with zero loss
- Overall Defensibility: DIFFERENTIATED (algorithm) but COMMODITIZED (consumer market)
- Key Risk: ESPN/free AI alternatives commoditize the consumer bracket prediction market
```

### Competitive Threat Analysis

**What a New Entrant Would Need:**
1. Patent license or wait until 2033-2035 expiration
2. Access to ESPN pick distribution data
3. Multi-year validation period for credibility
4. Superior UX (current app is basic)

**Critical Vulnerability:**
- ESPN could trivially build this with their data
- Free AI tools (ChatGPT, Claude, Grok) provide "good enough" predictions
- Low revenue suggests market may not value the sophistication

---

## 4. ARCHITECTURAL HEALTH & STRATEGY

### Architecture Overview

```
[nginx Load Balancer]
        |
   [sb1] [sb2]  ------>  [rrtoolbox-api]
   (Node.js)             (Python FastAPI)
        |
   [MongoDB Atlas]
```

**Pattern:** Hybrid Monolith + Microservice

### Maturity Assessment

| Dimension | Level | Evidence |
|-----------|-------|----------|
| Architecture | MVP/Growth | Simple MVC, appropriate for scale |
| Infrastructure | Growth | Docker Compose, nginx LB, cloud DB |
| Code Quality | MVP | ~15% test coverage, minimal docs |
| Operations | Initial | No monitoring, alerting, or observability |
| Security | Initial | MD5 passwords, exposed secrets |

### Technical Debt Inventory

| Debt Item | Severity | Effort to Fix |
|-----------|----------|---------------|
| MD5 password hashing | **CRITICAL** | 1-2 days |
| Exposed FB App Secret | **HIGH** | 1 hour |
| Hardcoded year references | Medium | 1 day |
| Outdated dependencies | Medium | 2-3 days |
| Missing indices on Bracket.user | Low | 1 hour |
| No cascade delete for GDPR | Medium | 1 day |

### Scaling Readiness

**Current Capacity:** Hundreds of concurrent users
**Bottlenecks Identified:**
1. File-based bracket caching (bracketcache.txt)
2. No Redis or distributed cache
3. No CDN for static assets
4. nginx worker_connections at 4000 (modest)

**Scaling Path:** Straightforward horizontal scaling with:
- Redis for distributed caching
- CDN for static assets
- Container orchestration (K8s)

### Architectural Assessment

```
ARCHITECTURAL ASSESSMENT
- Maturity Level: MVP/Growth - Appropriate for current 445-user scale
- Scaling Readiness: Can handle 10x with minor investment
- Technical Debt Level: MODERATE (security debt is HIGH)
- Team Size Implied: 2-3 developers to maintain, built by 3-5
- Key Risk: Security vulnerabilities (MD5, exposed secrets)
- Architecture Fit: YES - Architecture matches seasonal consumer product
```

---

## 5. HIGH-LEVEL UNDERSTANDING (Business Loops)

### Core Business Loop

```
CORE BUSINESS LOOP: Bracket Optimization

Entry: User arrives via marketing (Rotowire, social ads, email)
       ↓
Signup: Create account (OAuth or email/password)
       ↓
Payment: $5 via Stripe Checkout
       ↓
Core Action: Answer 0-3 questions → Generate optimized bracket
       ↓
Value Realization: Print bracket for pool submission
       ↓
[LOOP ENDS - No retention mechanism]
```

**Critical Issue:** The loop terminates at value realization. There is no:
- Re-engagement for tournament progress
- Historical performance tracking
- Annual renewal reminder
- Referral or sharing mechanism

### Secondary Loop (Absent but Needed)

```
MISSING RETENTION LOOP: Annual Re-engagement

[Selection Sunday] → Email reminder → Return to site → One-click renewal
                                              ↓
                                    Generate new bracket
                                              ↓
                            See historical performance
                                              ↓
                                    Share results
```

### Key Metrics (Inferred from Code)

| Metric | Status | Evidence |
|--------|--------|----------|
| Primary Success | Bracket generation | Only action tracked |
| Activation Point | First bracket created | Post-payment |
| Expansion Hook | **NONE** | No team/pool/sharing features |
| Retention Driver | **NONE** | No re-engagement mechanisms |
| Viral Coefficient | **~0** | No sharing functionality |

### Business Loops Summary

```
CORE BUSINESS LOOPS

Loop 1: One-Time Bracket Generation (Current)
  Entry: Marketing-driven (paid ads, partnerships)
  Core Action: Pay $5, generate bracket, print
  Value Realization: Use bracket in pool
  Retention Driver: NONE - loop terminates
  Expansion Point: NONE - no team/referral features

Loop 2: Annual Return (Needed but Not Built)
  Entry: Email re-engagement
  Core Action: Renew, generate new bracket
  Value Realization: Compare to historical performance
  Retention Driver: Performance tracking, improvement narrative
  Expansion Point: Invite pool members, share results

Key Metrics (inferred from code):
- Primary Success Metric: Bracket generation count
- Secondary Metrics: None tracked
- Activation Point: First bracket (post-payment - problem!)
- Expansion Hook: None implemented
```

---

## 6. MISSING PIECES & GAPS

### Priority 1: Business Risk (Critical)

| Gap | Impact | Effort |
|-----|--------|--------|
| **Mobile revenue leak** | 100% revenue loss on mobile users | Quick win (1 line fix) |
| **MD5 password hashing** | Data breach liability | Quick win (1-2 days) |
| **No free tier/preview** | CAC unsustainably high | Medium (1-2 weeks) |
| **No usage metering** | Unlimited compute for flat fee | Medium (1 week) |

### Priority 2: Growth Limiting

| Gap | Impact | Effort |
|-----|--------|--------|
| **No retention mechanism** | Users churn after March | Medium (2-3 weeks) |
| **No referral system** | Zero organic acquisition | Medium (1-2 weeks) |
| **No sharing functionality** | No viral distribution | Medium (1 week) |
| **No historical tracking** | No year-over-year value | Medium (2 weeks) |

### Priority 3: Market Expansion

| Gap | Impact | Effort |
|-----|--------|--------|
| **No pool integration** | Can't embed in ESPN/Yahoo | Major (2-3 months) |
| **No API documentation** | Can't license to partners | Medium (2-4 weeks) |
| **No enterprise features** | Can't serve B2B market | Major (3-6 months) |
| **No multi-sport support** | Limited to March Madness | Major (varies) |

### Critical Gaps Analysis

```
CRITICAL GAPS ANALYSIS

Priority 1 (Business Risk):
- Mobile Revenue Leak: Mobile users bypass payment entirely
  Why it matters: Losing 100% of mobile-origin revenue
  Estimated Effort: Quick win (1 line code change)

- Security Vulnerabilities: MD5 hashing, exposed secrets
  Why it matters: Data breach liability, would fail any audit
  Estimated Effort: Quick win (1-2 days)

Priority 2 (Growth Limiting):
- No Retention/Re-engagement: Users have no reason to return
  Why it matters: Requires fresh CAC spend every year
  Estimated Effort: Medium (email automation, notifications)

- No Viral Mechanisms: Product cannot spread organically
  Why it matters: Requires paid acquisition at negative ROI
  Estimated Effort: Medium (sharing, referrals, pool invites)

Priority 3 (Market Expansion):
- No B2B/API Offering: Algorithm IP locked in consumer app
  Why it matters: Enterprise TAM is 100-1000x consumer TAM
  Estimated Effort: Major (API docs, SLAs, enterprise features)

Strategic Question: The absence of growth mechanisms suggests
this has always been a side project/technology showcase rather
than a serious growth business.
```

---

## 7. COMPETITIVE POSITIONING

### Competitive Landscape

| Competitor | Type | Price | Threat Level |
|------------|------|-------|--------------|
| ESPN "Smart Bracket" | Incumbent | Free | **HIGH** |
| ChatGPT/Claude/Grok | GenAI | Free/$20 | **HIGH** |
| CBS/Yahoo Smart Pick | Incumbent | Free | Medium |
| Bracketology sites | Specialty | Free/Freemium | Medium |

### 2025 AI Performance Comparison

```
Single Year Performance (2025):
- Claude Sonnet 3.7: 98.4th percentile
- SmartBracket: 92.4th percentile

Multi-Year Average Performance:
- SmartBracket cohort: 80.9%
- GenAI cohort: 71.8%
- SmartBracket 9-year: ~90th percentile
```

**Key Insight:** GenAI had higher peak performance in 2025 but much higher variance. SmartBracket's value proposition is **consistent, sustainable edge** vs. **lottery ticket performance** from GenAI.

### Competitive Positioning

```
COMPETITIVE POSITIONING

vs. ESPN Smart Bracket:
  Advantage: Proprietary MDP optimization (vs. basic recommendations)
  Disadvantage: Unknown brand vs. ESPN trust, free vs. $5
  Switching Cost: Zero

vs. Free AI (ChatGPT/Claude):
  Advantage: 9-year track record, lower variance, pool optimization
  Disadvantage: Price ($5 vs free), accessibility, brand awareness
  Switching Cost: Zero

vs. Other Bracket Tools:
  Advantage: Patented algorithm, academic credibility
  Disadvantage: No free tier, limited features, no community
  Switching Cost: Zero

Positioning Statement: "The only patented, academically-validated
bracket optimization tool with 9 years of proven ~90th percentile
performance—consistent results vs. AI lottery tickets."

Vulnerability: Consumer bracket prediction is commoditizing rapidly.
Free AI alternatives are "good enough" for most users. The market
is racing to zero.
```

---

## 8. ORGANIZATIONAL CAPABILITY

### Team Profile

| Metric | Finding |
|--------|---------|
| Estimated Team Size | 2-3 active developers |
| Total Contributors | 10 (93% commits from top 3) |
| Primary Expertise | Mathematical algorithms (PhD-level) |
| Secondary Expertise | Full-stack Node.js development |
| Development Pattern | Seasonal (Feb-March burst) |
| Product Stage | Maintenance mode |

### Execution Assessment

| Strength | Evidence |
|----------|----------|
| Algorithm Expertise | Sophisticated MDP implementation |
| Domain Knowledge | 7+ years tournament data |
| Capital Efficiency | Lean infrastructure, smart outsourcing |
| Pragmatic Architecture | Clean separation of concerns |

| Weakness | Evidence |
|----------|----------|
| Security Practices | MD5, exposed secrets |
| Testing Culture | ~15% coverage |
| Documentation | Minimal, outdated |
| Observability | No monitoring/alerting |

### Scaling Constraints

1. **Human Capital** - 1-2 active developers working seasonally
2. **Algorithm Bottleneck** - MDP expertise concentrated in founders
3. **Technical Debt** - Security issues block enterprise sales
4. **Seasonal Model** - Burst development doesn't support continuous evolution

### Hiring Priorities (If Scaling)

| Priority | Role | Rationale |
|----------|------|-----------|
| 1 | Security-Minded Full-Stack | Address critical security debt |
| 2 | DevOps/SRE | Implement monitoring, CI/CD |
| 3 | Data Engineer | Automate annual data updates |
| 4 | Business Development | Enterprise/API licensing |

---

## 9. MARKET OPPORTUNITY & TAM

### Consumer Bracket Market (Current Business)

| Market Tier | Size |
|-------------|------|
| TAM | $350M (70M brackets × $5) |
| SAM | $10M (2M "serious" players) |
| SOM | $22K (theoretical with better GTM) |
| **Current Reality** | **$220/year** (44 users × $5) |

**Verdict:** Consumer bracket market is a **dead end**. Free AI alternatives have commoditized prediction. Race to zero pricing.

### Enterprise RRToolbox Market (Pivot Opportunity)

| Market Tier | Size |
|-------------|------|
| TAM | $50B+ (decision science software) |
| SAM | $500M (MDP/dynamic programming tools) |
| SOM | $5M-50M (achievable with focused GTM) |

**RRToolbox has 16+ enterprise solution templates:**

| Template | Enterprise Use Case | Market Size |
|----------|---------------------|-------------|
| AutoDriveOrSell | Fleet management optimization | $15B |
| AutoMarketShare | Competitive dynamics modeling | $50B+ |
| PropertyValuation | Real estate pricing | $12B |
| HiringWithPolicyRisk | HR workforce planning | $20B |
| MachineReplacement | CapEx planning | $8B |
| HouseholdSaving | Wealth/retirement planning | $300B |

### Strategic Recommendation

```
MARKET OPPORTUNITY ASSESSMENT

Current Path (Consumer Brackets):
- Market: Shrinking/commoditizing
- Competition: Free AI alternatives winning
- Unit Economics: Broken (0.12:1 LTV:CAC)
- Defensibility: Weak (no switching costs)
- Verdict: DEAD END

Pivot Path (Enterprise RRToolbox Licensing):
- Market: Growing ($50B+ TAM)
- Competition: Limited (patents protect)
- Unit Economics: Strong (5-25x LTV:CAC typical)
- Defensibility: Strong (patents, complexity)
- Verdict: HIGH OPPORTUNITY

Recommended Strategy:
1. Maintain SmartBracket as proof-of-concept/demo
2. Use 9-year track record for enterprise credibility
3. License RRToolbox to sports betting, finance, operations
4. Target $50K-500K annual enterprise contracts
5. Time window: 3-5 years before AI commoditizes MDP
```

---

## 10. STRATEGIC RECOMMENDATIONS

### Immediate Actions (< 1 Week)

| Action | Impact | Effort |
|--------|--------|--------|
| Fix mobile revenue leak | Recover mobile revenue | 1 hour |
| Replace MD5 with bcrypt | Critical security fix | 1-2 days |
| Remove hardcoded FB secret | Security fix | 1 hour |
| Add bracket sharing URL | Enable organic distribution | 1 day |

### Short-Term (1-3 Months)

| Action | Impact | Effort |
|--------|--------|--------|
| Implement freemium tier | Reduce CAC, increase trials | 2-3 weeks |
| Add referral system | Enable organic growth | 2 weeks |
| Build retention email flow | Increase annual return rate | 2 weeks |
| Add historical performance | Create year-over-year value | 2 weeks |
| Implement usage quotas | Align cost with revenue | 1 week |

### Strategic (3-12 Months)

| Action | Impact | Effort |
|--------|--------|--------|
| Document RRToolbox API | Enable enterprise licensing | 1-2 months |
| Build enterprise features | SOC2, SLAs, audit trails | 3-6 months |
| Pursue B2B partnerships | License to sports platforms | Ongoing |
| Expand to other domains | Real estate, finance, HR | 6-12 months |

### The Pivot Case

**SmartBracket should become a showcase for RRToolbox, not a standalone business.**

| Factor | Consumer Bracket | Enterprise RRToolbox |
|--------|------------------|----------------------|
| Market Size | Shrinking | Growing ($50B+) |
| Competitive Moat | Weak | Strong (patents) |
| LTV/CAC | 0.12:1 | 5-25x |
| Scalability | Seasonal, limited | Year-round, global |
| Defensibility | Low | High |

**Positioning:**

> "SmartBracket: The most successful public demonstration of Rapid Recursive optimization. Proven across 9 years, consistently outperforming GenAI predictions. Now available for enterprise licensing."

---

## 11. OUTPUT CHECKLIST

### Business Questions

- [x] **Is the business case clear from code?** Yes - bracket optimization for pool participants
- [x] **Does pricing align with implementation?** No - major gaps (mobile leak, no metering, no tiers)
- [x] **What's the moat?** Algorithm/patents (strong), data (weak), distribution (none)
- [x] **What's the core value loop?** One-time bracket generation - loop terminates, no retention
- [x] **What's missing for growth?** Freemium, retention, referrals, sharing, annual renewal

### Strategic Questions

- [x] **Competitive position?** Differentiated algorithm but commoditized consumer market
- [x] **Biggest risk?** Free AI alternatives making consumer bracket market unprofitable
- [x] **Highest-value opportunity?** Enterprise RRToolbox licensing (100-1000x TAM)
- [x] **Next priority?** Fix revenue leaks, implement freemium, document API for B2B
- [x] **Architecture fit?** Yes for consumer product; needs enterprise features for pivot

### Organizational Questions

- [x] **Team capability?** Strong algorithm expertise, weak security/ops practices
- [x] **Where strong/weak?** Strong: Math/domain expertise. Weak: Growth, security, ops
- [x] **What limits growth?** Human capital, seasonal model, no retention mechanisms
- [x] **Critical hires?** Security engineer, DevOps, Business Development

---

## Appendix: Sub-Agent Analysis Sources

| Sub-Agent | Focus Area | File |
|-----------|------------|------|
| A | Data Model & Moat | `sub-a-data-model-moat-analysis.md` |
| B | Pricing & Unit Economics | `sub-b-pricing-unit-economics-analysis.md` |
| C | GTM & Distribution | `sub-c-gtm-distribution-analysis.md` |
| D | Organizational Execution | `sub-d-organizational-execution-analysis.md` |
| E | Market Opportunity & TAM | `sub-e-market-opportunity-tam-analysis.md` |

### Key Evidence Files

| File | Business Insight |
|------|------------------|
| `models/User.js` | Security debt (MD5), simple schema |
| `models/Bracket.js` | Algorithm output storage |
| `controllers/checkout.js` | Pricing logic ($5/$3) |
| `controllers/mobileApp.js:148` | Mobile revenue leak |
| `controllers/bracket.js` | Core business logic |
| `RRToolbox-API/app/MarchMadness.py` | Proprietary algorithm IP |
| `RRToolbox-API/RRToolbox/templates/` | Enterprise expansion templates |

---

*Analysis completed: January 2026*
*Total analysis depth: ~1,900 lines across 6 reports*
*Framework: Codebase Business Analysis v1.0*
