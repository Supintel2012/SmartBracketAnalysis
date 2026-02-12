# SmartBracket Pricing Model & Unit Economics Analysis
## Sub-Agent B Analysis Report

**Generated:** January 2026
**Analyst:** Claude Code Business Analysis Framework

---

## Executive Summary

SmartBracket operates a fundamentally broken unit economics model where **Customer Acquisition Cost (CAC) of $18-$94 per user** vastly exceeds the **Lifetime Value (LTV) of ~$4.60** per user, resulting in a **-$14,790 net loss** over 5 years despite $16,897 in marketing spend. The technical implementation supports only a simple one-time payment model with no mechanisms for recurring revenue, expansion, or usage-based pricing.

---

## 1. Usage Instrumentation

### What IS Being Tracked/Metered

| Data Point | Location | Monetization Status |
|------------|----------|---------------------|
| User paid status | `models/User.js:11` | Yes - gates bracket access |
| Discount eligibility | `models/User.js:21` | Yes - $3 vs $5 pricing |
| Bracket creation timestamp | `models/Bracket.js:19` | Not monetized |
| User app platform | `models/User.js:13` | Not monetized |
| Last bracket ID | `models/User.js:12` | Not monetized |

### What is NOT Being Tracked (Revenue Leaks)

| Gap | Impact |
|-----|--------|
| Bracket generation count | Unlimited brackets after $5 payment |
| API call frequency | No metering of RRToolbox calls |
| Computation time | Algorithm runs untracked |
| One-click vs custom brackets | No differentiation |
| Season/year usage | No multi-year engagement tracking |

**Critical Code** (controllers/bracket.js:172):
```javascript
if (req.user.paid == true) {
  // Proceeds to call expensive RRToolbox API with no metering
}
```

---

## 2. Cost Drivers

### Infrastructure Costs

| Component | Cost Type | Scaling Behavior |
|-----------|-----------|------------------|
| RRToolbox API | Per-call compute | Linear with bracket generation |
| Node.js web app | Fixed | Horizontal scaling needed |
| MongoDB | Per-document | Linear with users/brackets |
| Stripe processing | 2.9% + $0.30 | Per-transaction |

### RRToolbox Algorithm Cost

From `RRToolbox-API/app/MarchMadness.py`:

```python
# Loads data files PER REQUEST
self.P = np.load(input_data_loc + "P.npy")
self.pickChance = np.loadtxt(...)
self.statChance = np.loadtxt(...)

# Value iteration - O(S^2 * A) complexity
vi = ValueIteration(self.P, self.R, self.beta, threshold=thresh)
```

**Estimated cost per bracket**: $0.001-$0.01 compute

### Unit Economics Summary

| Metric                    | Value                           |
| ------------------------- | ------------------------------- |
| Price per user (full)     | $5.00                           |
| Price per user (discount) | $3.00-$4.00                     |
| Stripe fees               | ~$0.45                          |
| Gross margin per user     | $4.55 (full) / $2.55 (discount) |
| CAC (5-year average)      | $37.98                          |
| CAC 2024                  | $94.08                          |
| **LTV:CAC Ratio**         | **0.12:1** (Target: 3:1+)       |

---

## 3. Pricing Model Fit

### Current Model: Single-Tier, One-Time Purchase

```javascript
// controllers/checkout.js:19
unit_amount: 500,  // Fixed $5.00

// controllers/checkout.js:94
amount: discount ? 300 : 500,  // Binary: $3 or $5
```

**No evidence of:**
- Subscription billing
- Usage-based pricing
- Feature tiers
- Volume pricing
- Enterprise/team plans

### Coupon Architecture

- `allow_promotion_codes: true` in Stripe config
- WELCOMEBACK, ROTOWIRE, PIDAY codes via manual campaigns
- Only two price points supported ($3 or $5)

### Pricing Model Mismatch

| Cost Structure | Current Pricing | Alignment |
|----------------|-----------------|-----------|
| Per-bracket compute | One-time payment | MISALIGNED |
| Seasonal usage | Year-round access | MISALIGNED |
| Algorithm R&D (patented) | Low price point | MISALIGNED |
| Mobile app users | Free (paid: true by default) | REVENUE LEAK |

**Critical Mobile Revenue Leak** (controllers/mobileApp.js:148):
```javascript
paid: req.body.appPlatform == "web" ? false : true,
// Mobile users marked paid automatically!
```

---

## 4. Pricing Architecture

### Natural Product Tiers (Embedded but Not Monetized)

| Tier Feature | Current Status |
|--------------|----------------|
| One-Click Bracket | Free with paid |
| Custom Bracket | Free with paid |
| Locked Picks | Free with paid |
| Refresh/Recalculate | Free with paid |
| Mobile API Access | Free (bypasses payment) |
| Shareable Pages | Free |

### Potential Tier Structure

```
Free: One-click bracket only (cached, zero compute)
Basic ($5): Limited custom brackets (3-5 per season)
Pro ($15): Unlimited brackets + locked picks
Premium ($25): API access + shareable links + historical data
```

### Quota System: **None Implemented**

```javascript
// controllers/bracket.js - NO LIMITS
exports.postCreateBracket = async (req, res) => {
  // Creates bracket immediately, no quota check
}
```

---

## 5. Monetization Gaps

### Revenue Leak Areas

| Gap | Impact |
|-----|--------|
| Mobile users free | 100% revenue loss on mobile |
| Unlimited brackets | High compute, fixed revenue |
| No annual renewal | Zero recurring revenue |
| No referral/affiliate | Missed organic acquisition |
| Public shareable brackets | Value shared without conversion |

### Undermonetized Features

1. **Algorithm sophistication** - Patented RRToolbox priced at commodity level
2. **Historical data** - Years of probability data not tiered
3. **Bracket sharing** - Social feature exists but doesn't gate/monetize
4. **One-click bracket** - Could be freemium hook but given away

### Missing Expansion Revenue Mechanisms

| Mechanism | Implemented | Opportunity |
|-----------|-------------|-------------|
| Subscription/renewal | NO | Annual $5-15/year |
| Team/group plans | NO | $25/group |
| Premium features | NO | API, exports, analytics |
| White-label/B2B | NO | License to sports sites |

---

## 6. Financial Verdict

### Pricing Model Alignment Score: 2/10

| Criterion | Score |
|-----------|-------|
| Cost-revenue alignment | 1/10 |
| Value capture | 2/10 |
| Expansion potential | 1/10 |
| Customer segmentation | 2/10 |
| Competitive positioning | 4/10 |

### Cost Structure Risks

1. **CAC Spiral**: Marketing returns negative ROI consistently
2. **Mobile Revenue Hole**: Mobile users bypass payment
3. **Seasonality**: 100% revenue in Feb-April, costs year-round
4. **Algorithm Scaling**: More users = more compute, fixed revenue

---

## Revenue Optimization Opportunities (Priority Order)

### 1. Plug Mobile Revenue Leak (High Impact, Low Effort)
```javascript
// Fix in mobileApp.js
paid: false,  // Require payment on all platforms
```

### 2. Implement Freemium Model (High Impact, Medium Effort)
- Free: One-click bracket (cached, zero compute)
- Paid: Custom optimization
- Reduces CAC through organic trial

### 3. Add Usage Metering (Medium Impact, Medium Effort)
```javascript
// Add to User model
bracketCount: { type: Number, default: 0 },
bracketLimit: { type: Number, default: 5 },
```

### 4. Annual Subscription (High Impact, Medium Effort)
- Convert one-time to annual
- Add `paidYear: Number` field to User
- Gate access to current year's data

### 5. Tiered Pricing (High Impact, High Effort)
- Implement feature flags based on tier
- Create Pro/Premium at $15/$25
- Gate locked picks, refresh, API access

---

## Why CAC is Unsustainable

### Root Causes

1. **No organic acquisition loop** - Product has no viral/referral mechanism
2. **No conversion funnel** - All-or-nothing paywall with no trial
3. **Timing mismatch** - Marketing spend year-round, conversions in 2 months
4. **Price-value disconnect** - $5 feels "too cheap to matter"
5. **No retention mechanism** - Users churn after March Madness

### Recommended Pricing Strategy

| Current | Recommended |
|---------|-------------|
| $5 one-time | $5/month or $15/year subscription |
| Binary paid/unpaid | Free tier + paid tier |
| Unlimited brackets | 3 free + unlimited paid |
| No mobile monetization | In-app purchase parity |
| No referral | "Give $5, Get $5" referral credit |

---

## Critical Files

| File | Purpose |
|------|---------|
| `controllers/checkout.js` | Pricing logic, Stripe integration |
| `models/User.js` | Paid boolean - needs tier, usage counters |
| `controllers/bracket.js` | Bracket creation - needs metering |
| `controllers/mobileApp.js` | Mobile API - line 148 revenue leak |
| `config/bracketCache.js` | Cached brackets - freemium foundation |
