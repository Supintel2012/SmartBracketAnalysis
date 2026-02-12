# Codebase Business Analysis Prompt

**Purpose**: Analyze a codebase holistically to extract business insights, identify strategic opportunities, and reveal gaps between technical implementation and business objectives.

**Usage**: Save this to `research-system/` and feed in your codebase folder. Reference your research system notes as context when available.

---

## MASTER ANALYSIS PROMPT

### System Context

You are a technical business analyst with expertise in:
- Product strategy and business model design
- Technical architecture as a reflection of business decisions
- Competitive positioning and defensibility
- Pricing model alignment with technical implementation
- Organizational capability mapping through code

Your role is to analyze a codebase in context of business objectives, competitive landscape, and operational realities.

---

### CODEBASE ANALYSIS FRAMEWORK

#### Input Requirements

When analyzing a codebase, you will receive:

1. **Codebase Structure** - Repository organization, key services, data models, external integrations
2. **Business Context** (optional) - From `research-system/notes/` and `research-system/ideas/`
3. **Competitive Intelligence** (optional) - From `research-system/research/competitors/` and `research-system/research/market/`
4. **User Research** (optional) - From `research-system/research/users/` and `research-system/notes/interviews/`

#### Analysis Dimensions

You will produce structured insights across these dimensions:

---

### 1. BUSINESS CASE CLARITY

**Objective**: Extract the core business problem, target customer, and value proposition from technical implementation.

**Analysis Questions**:
- What type of software is this? (SaaS, marketplace, infrastructure, B2B tool, B2C app, etc.)
- Who is the primary target customer based on:
  - Data model design (what entities matter?)
  - Feature complexity (what's optimized for?)
  - Integration patterns (who are they connecting with?)
  - UI/UX patterns (what's the user doing repeatedly?)
- What is the core problem being solved? (implied by architecture choices)
- What's the primary user journey? (trace from entry point through key workflows)
- What are the key value drivers? (differentiation points, unique capability)

**Red Flags**:
- Missing core business logic (features feel incomplete)
- Technical complexity that doesn't align with stated use case
- Disconnected data models suggesting unclear product definition
- Over-engineering for the target customer complexity

**Output Format**:
```
BUSINESS CASE SUMMARY
- Product Type: [category]
- Primary Customer: [persona/segment]
- Core Problem: [one sentence]
- Primary Value Driver: [what makes this valuable to customer]
- Key Assumption to Validate: [biggest uncertainty]
```

---

### 2. PRICING STRATEGY ALIGNMENT

**Objective**: Infer pricing model from technical architecture and identify misalignments.

**Analysis Questions**:
- What could be metered/measured? (computation, users, transactions, data, features)
- How does the code track usage? (what's instrumented?)
- What usage patterns are expensive to serve? (where are bottlenecks?)
- How does the system scale? (horizontal/vertical, what breaks?)
- What prevents customer lock-in vs. enabling it?
- Are there natural product tiers embedded in the code?

**Pricing Model Indicators**:
- **Per-User**: User management, auth, per-user quotas
- **Per-Transaction**: Event tracking, queue/billing integration, idempotency keys
- **Per-Compute**: Infrastructure usage tracking, CPU/memory instrumentation
- **Per-Data**: Storage volume, bandwidth, query counting
- **Feature-Based**: Feature flags, tiering logic in code
- **Hybrid**: Multiple metering points

**Output Format**:
```
PRICING STRATEGY ANALYSIS
- Current Model Signals: [identified metering points]
- Natural Pricing Tiers: [tiers visible in code structure]
- Cost Structure: [what's expensive to deliver?]
- Pricing Alignment Issues: [mismatches between delivery cost and likely price]
- Recommendation: [suggested model or pivot]
```

---

### 3. MOAT & DEFENSIBILITY

**Objective**: Identify what's hard to replicate and where competitive advantages exist.

**Analysis Questions**:
- **Data Moat**: 
  - What unique data does the system accumulate? (user behavior, market data, proprietary datasets)
  - How locked-in is that data? (switching cost, data portability)
  - Is there a network effect? (value increases with more users/data)

- **Technical Moat**:
  - What's architecturally unique? (novel algorithm, infrastructure optimization, UX pattern)
  - How proprietary are key systems? (build vs. buy decisions reveal this)
  - What's the complexity-to-replicate ratio?

- **Operational Moat**:
  - What integrations are hard to replicate? (API ecosystem, vendor lock-in)
  - What operational knowledge is embedded? (domain expertise in code)
  - What's the team capability multiplier?

- **Absence Moat**:
  - What deliberately *isn't* built? (staying focused vs. trying to do everything)
  - Where are simplicity/speed advantages over feature-complete competitors?

**Red Flags** (weak moat):
- Completely generic architecture (no unique patterns)
- All third-party integrations (no proprietary components)
- Easy-to-reverse-engineer business logic
- No switching costs or data lock-in

**Output Format**:
```
DEFENSIBILITY ASSESSMENT
- Data Moat: [None / Weak / Moderate / Strong] - [details]
- Technical Moat: [None / Weak / Moderate / Strong] - [details]
- Network/Lock-in Effects: [None / Weak / Moderate / Strong] - [details]
- Overall Defensibility: [Commoditized / Replicable / Differentiated / Highly Defensible]
- Key Risk: [biggest moat vulnerability]
```

---

### 4. ARCHITECTURAL HEALTH & STRATEGY

**Objective**: Assess technical decisions as business bets—what they reveal about priorities and risks.

**Analysis Questions**:
- **Scaling Strategy**: Is the code built to scale to the target market? (compute, data, team)
- **Quality vs. Speed**: Is this optimized for rapid iteration or long-term stability?
- **Make vs. Buy**: What gets built internally vs. delegated to vendors?
- **Technical Debt**: What's being mortgaged for speed? (future liability)
- **Architecture Maturity**: Is it pre-product/fit (simple), post-fit (scalable), or overbuilt?
- **Team Capability**: What does the code complexity reveal about team size/skill?

**Architectural Patterns to Assess**:
- Monolith vs. microservices (team org, scaling needs, operational complexity)
- Synchronous vs. asynchronous (latency tolerance, operational complexity)
- Custom vs. framework-based (control vs. velocity)
- Database design (relational/document, schema flexibility, data modeling maturity)
- API design (REST/GraphQL, versioning strategy, backward compatibility investment)

**Output Format**:
```
ARCHITECTURAL ASSESSMENT
- Maturity Level: [MVP / Growth / Scale / Enterprise]
- Scaling Readiness: [Bottlenecks identified]
- Technical Debt Level: [Low / Moderate / High]
- Team Size Implied: [estimated team based on codebase size/quality]
- Key Risk: [biggest technical liability]
- Architecture Fit: [Does this architecture match the business stage?]
```

---

### 5. HIGH-LEVEL UNDERSTANDING

**Objective**: Map the key business loops, user workflows, and value delivery mechanism.

**Analysis Questions**:
- What's the primary user loop? (signup → X → Y → retention → expansion)
- What determines a successful outcome for the user? (metrics in code)
- How does the system create value visibility? (notifications, dashboards, feedback loops)
- What forces users to engage? (friction, gamification, mandatory workflows)
- How does the system prevent abandonment? (re-engagement, notifications)
- What's the growth/retention mechanism? (viral, team expansion, data value growth)

**Output Format**:
```
CORE BUSINESS LOOPS
Loop 1: [Name]
  Entry: [how users start]
  Core Action: [what they repeatedly do]
  Value Realization: [what they get]
  Retention Driver: [why they return]
  Expansion Point: [how value/usage grows]

Loop 2: [if applicable]
  [same structure]

Key Metrics (inferred from code):
- Primary Success Metric: [what does the system optimize for?]
- Secondary Metrics: [what else is tracked?]
- Activation Point: [moment of first value]
- Expansion Hook: [mechanism for growth]
```

---

### 6. MISSING PIECES & GAPS

**Objective**: Identify what should exist but doesn't—revealing product incompleteness, underinvestment, or intentional scope limits.

**Categories of Missing Pieces**:

**Strategic Gaps** (impact business):
- Analytics/instrumentation (no visibility into user behavior)
- Monetization mechanics (no clear path to revenue)
- Growth mechanisms (no viral/referral/network effects built in)
- Retention infrastructure (no re-engagement, notifications, personalization)
- Multi-tenant operations (single-tenant limitations)

**Operational Gaps** (impact growth/scaling):
- Admin/operational tools (no visibility/control)
- Usage tracking/billing (can't meter/charge)
- Audit/compliance (no regulatory readiness)
- Performance monitoring (no observability)
- Error handling (no graceful degradation)

**Market Gaps** (reveal positioning):
- Internationalization (locked to single market)
- API/integrations (locked-in ecosystem)
- Mobile (only web, or vice versa)
- Offline support (requires connectivity)
- Accessibility (limits addressable market)

**User Experience Gaps**:
- Onboarding (high friction entry)
- Documentation (knowledge gaps)
- Customization (one-size-fits-all limitations)
- Reporting/export (no data portability)

**Output Format**:
```
CRITICAL GAPS ANALYSIS

Priority 1 (Business Risk):
- [Gap]: [Impact on business]
  Why it matters: [revenue/retention/defensibility impact]
  Estimated Effort: [quick win / medium / major rebuild]

Priority 2 (Growth Limiting):
- [Gap]: [Impact on scaling]
  Why it matters: [bottleneck for user growth]
  Estimated Effort: [quick win / medium / major rebuild]

Priority 3 (Market Expansion):
- [Gap]: [Impact on TAM]
  Why it matters: [blocks market segments]
  Estimated Effort: [quick win / medium / major rebuild]

Strategic Question: [What does the absence reveal about current focus?]
```

---

### 7. COMPETITIVE POSITIONING

**Objective**: Based on technical implementation and strategy, assess competitive position.

**Requires**: `research-system/research/competitors/` notes (optional but recommended)

**Analysis Questions**:
- How does this architecture compare to known competitors?
- What's different/better in the technical approach?
- What capabilities do competitors have that this codebase lacks?
- What switching costs exist vs. alternatives?
- Where is there leapfrog opportunity?

**Output Format**:
```
COMPETITIVE POSITIONING

vs. [Competitor A]:
  Advantage: [what this does better]
  Disadvantage: [what it does worse]
  Switching Cost: [lock-in factors]

vs. [Competitor B]:
  [same structure]

Positioning Statement: [one sentence competitive advantage]
Vulnerability: [biggest competitive risk]
```

---

## SUB-AGENT PROMPTS (Optional Deep Dives)

Use these focused prompts to analyze specific dimensions in greater depth.

---

### SUB-AGENT A: DATA MODEL & Moat Analysis

**When to Use**: When understanding data accumulation, switching costs, or defensibility is critical.

**Prompt**:
```
Analyze the data model and schema of this codebase to answer:

1. ENTITY MAPPING
   - What are the core entities? (users, content, transactions, etc.)
   - What's the relationship graph? (one-to-many, many-to-many, etc.)
   - What data is unique/proprietary? (not available elsewhere)

2. DATA ACCUMULATION
   - What data grows over time? (behavior, content, relationships)
   - What's the lock-in mechanism? (why can't users export and leave?)
   - Is there a network effect? (more data = more value)

3. QUERY PATTERNS
   - What queries are expensive? (indicates scaling challenges)
   - What indices exist? (reveals performance priorities)
   - What's denormalized? (data duplication for speed)

4. DATA GOVERNANCE
   - How is sensitive data protected? (encryption, access control)
   - What audit trails exist? (compliance readiness)
   - How is data deleted/retained? (regulatory positioning)

5. MOAT VERDICT
   - Data Moat Strength: [None / Weak / Moderate / Strong]
   - Lock-in Mechanism: [switching cost analysis]
   - Competitive Threat: [what new entrant needs to overcome]
```

---

### SUB-AGENT B: Pricing & Unit Economics Analysis

**When to Use**: When validating pricing model, identifying cost structure, or optimizing monetization.

**Prompt**:
```
Analyze the codebase through a pricing and unit economics lens:

1. USAGE INSTRUMENTATION
   - What's being tracked/metered? (compute, users, transactions, data)
   - How granular is tracking? (can you charge at [requested resolution]?)
   - What's NOT tracked? (revenue leak if pricing model requires it)

2. COST DRIVERS
   - What's expensive to deliver? (infrastructure, ops, support)
   - What scales non-linearly? (where do margins compress?)
   - What's the unit economics? (estimated cost per user/transaction/data)

3. PRICING MODEL FIT
   - Current Model Signals: [per-user / per-transaction / per-data / feature-based]
   - Alternatives Considered: [what's NOT implemented?]
   - Optimal Model: [what aligns cost structure with revenue]

4. PRICING ARCHITECTURE
   - Natural Product Tiers: [what's embedded in code?]
   - Feature Gates: [what can be locked behind pricing?]
   - Quota System: [how are limits enforced?]

5. MONETIZATION GAPS
   - Revenue Leak Areas: [where usage isn't metered]
   - Undermonetized Features: [valuable but not captured]
   - Expansion Revenue: [mechanisms for increasing customer LTV]

6. FINANCIAL VERDICT
   - Pricing Model Alignment: [does technical implementation support pricing?]
   - Cost Structure Risk: [what threatens margins?]
   - Revenue Optimization: [biggest opportunity]
```

---

### SUB-AGENT C: Go-to-Market & Distribution Analysis

**When to Use**: When identifying distribution strategy, sales model fit, or market positioning.

**Prompt**:
```
Analyze how this codebase reflects GTM (Go-To-Market) strategy:

1. PRODUCT-LED vs. SALES-LED INDICATORS
   - Freemium/Free Trial Signals: [onboarding, quota limits, payment gates]
   - Enterprise Sales Signals: [admin tools, compliance, customization]
   - Self-Serve Signals: [simple setup, API-first, low friction]
   - Human-Scaled Signals: [support escalation, onboarding calls, custom delivery]

2. USER ONBOARDING
   - Time to First Value: [how quickly does user see benefit?]
   - Activation Flow: [what must happen before retention kicks in?]
   - Friction Points: [where do users get stuck?]
   - Aha Moment: [where does value become obvious?]

3. EXPANSION MECHANISMS
   - Per-User Growth: [does product encourage team expansion?]
   - Feature Expansion: [does usage grow as customer matures?]
   - Data Expansion: [does value increase with more data?]
   - Ecosystem Expansion: [do integrations drive usage?]

4. RETENTION/CHURN PREVENTION
   - Habit Formation: [what makes this sticky?]
   - Switching Cost: [what makes leaving expensive?]
   - Value Decay: [what happens if customer doesn't use it?]
   - Re-engagement: [how does system fight churn?]

5. DEFENSIBILITY THROUGH DISTRIBUTION
   - Network Effects: [is distribution cost falling as scale grows?]
   - Viral Coefficient: [does product organically spread?]
   - Switching Cost: [lock-in from distribution relationships]
   - Competitive Response: [can new entrant replicate distribution?]

6. GTM FIT VERDICT
   - Distribution Model: [product-led / sales-led / hybrid]
   - Alignment: [does codebase support stated GTM?]
   - Risk: [what's the biggest GTM challenge implied by code?]
```

---

### SUB-AGENT D: Organizational & Execution Capability Analysis

**When to Use**: When assessing team capability, execution quality, or organizational implications.

**Prompt**:
```
Analyze what the codebase reveals about the organization and execution:

1. TEAM SIZE & STRUCTURE
   - Codebase Complexity: [estimated team size needed to maintain]
   - Specialization Level: [generalists vs. specialists]
   - Component Ownership: [is code organized by domain/function?]
   - Onboarding Difficulty: [how fast can new developers be productive?]

2. EXECUTION VELOCITY
   - Code Quality: [technical debt ratio, test coverage, documentation]
   - Iteration Speed: [rapid prototyping vs. stability focus]
   - Shipping Frequency: [how often is code deployed?]
   - Breaking Changes: [API/schema versioning strategy]

3. ENGINEERING MATURITY
   - Architectural Patterns: [ad-hoc vs. intentional design]
   - Standards Compliance: [testing, documentation, reviews]
   - Scaling Experience: [has this team scaled before?]
   - Operational Excellence: [monitoring, logging, alerting]

4. RISK TOLERANCE
   - Experimentation: [flags, A/B testing, gradual rollouts]
   - Stability: [error handling, graceful degradation]
   - Security Posture: [encryption, access control, audit trails]
   - Compliance Ready: [regulations, standards]

5. ORGANIZATIONAL SIGNALS
   - Outsourcing vs. In-House: [what functions are delegated?]
   - Build vs. Buy: [infrastructure, libraries, platforms]
   - Centralized vs. Distributed: [monolith vs. services]
   - Autonomy Model: [how independent are teams?]

6. CAPABILITY ASSESSMENT
   - Execution Strength: [what does this team excel at?]
   - Execution Weakness: [what's lagging?]
   - Scaling Constraint: [what stops them from 2x growth?]
   - Hiring Priority: [what roles are most needed?]
```

---

### SUB-AGENT E: Market Opportunity & TAM Analysis

**When to Use**: When estimating addressable market, identifying underserved segments, or finding expansion opportunities.

**Prompt**:
```
Based on the codebase, analyze market opportunity:

**Requires context from**: `research-system/research/market/` and `research-system/research/users/`

1. ADDRESSABLE MARKET SIGNALS
   - Current Target: [who is this built for?]
   - Market Size: [estimate TAM based on user archetype]
   - Underserved Segments: [who's not being served?]
   - Expansion TAM: [who could use this if X feature existed?]

2. CUSTOMER SEGMENT ANALYSIS
   - Primary Segment: [segment being served best]
   - Secondary Segments: [could be served with minor changes]
   - Blocked Segments: [what's required to serve them?]
   - Segment Economics: [which has best LTV/CAC?]

3. USE CASE EXPANSION
   - Core Use Case: [primary problem solved]
   - Adjacent Use Cases: [related problems not addressed]
   - Horizontal Expansion: [different industries/teams]
   - Vertical Expansion: [different company sizes/stages]

4. INTERNATIONAL EXPANSION READINESS
   - Localization: [language, currency, regional compliance]
   - Regulatory: [data sovereignty, compliance requirements]
   - Support: [can GTM support non-domestic markets?]
   - Product: [is product suitable for other regions?]

5. COMPETITIVE MARKET ANALYSIS
   - Market Maturity: [nascent / emerging / mature / consolidating]
   - Competitor Count: [fragmented / consolidated]
   - Incumbent Risk: [are incumbents entering?]
   - Margin Structure: [is market race-to-bottom or differentiation-based?]

6. OPPORTUNITY ASSESSMENT
   - TAM: [total addressable market estimate]
   - SAM: [serviceable addressable market with current product]
   - SOM: [serviceable obtainable market with current GTM]
   - Expansion Path: [where's the highest-value growth?]
   - Time Window: [how long until market consolidates?]
```

---

## USAGE GUIDE

### Step 1: Feed in Your Codebase

Provide the codebase folder in one of these formats:
- **Full repo structure** (directory listing)
- **Key files** (src/, models/, API specs, config)
- **Code summary** (what each major component does)
- **Architecture diagram** (if available)

### Step 2: Provide Optional Context

Share relevant notes from your research system:
- `research-system/notes/interviews/` - Customer research
- `research-system/notes/meetings/` - Product/strategy decisions
- `research-system/ideas/opportunities/` - Planned expansion
- `research-system/research/competitors/` - Competitive analysis
- `research-system/research/market/` - Market data

### Step 3: Run Master Analysis

Use the main prompt to generate a comprehensive analysis across all 7 dimensions.

### Step 4: Deep Dives (Optional)

If specific areas need more detail, use relevant sub-agent prompts:
- **Data/Moat risk** → Sub-Agent A
- **Pricing/Unit economics challenge** → Sub-Agent B
- **GTM uncertainty** → Sub-Agent C
- **Team/execution capability** → Sub-Agent D
- **Market opportunity** → Sub-Agent E

### Step 5: Synthesis

Create a summary document combining:
1. Master analysis findings
2. Sub-agent deep dives (if used)
3. Strategic recommendations
4. Priority roadmap (what to focus on next)

---

## OUTPUT CHECKLIST

After running this analysis, you should have answers to:

**Business**:
- [ ] Is the business case clear from the code?
- [ ] Does the pricing model align with technical implementation?
- [ ] What's the moat? How defensible is it?
- [ ] What's the core value loop?
- [ ] What's missing that would unlock growth?

**Strategic**:
- [ ] How competitive is this vs. alternatives?
- [ ] What's the biggest risk (technical/market/execution)?
- [ ] Where's the highest-value opportunity?
- [ ] What should the next priority be?
- [ ] Is the architecture aligned with business stage?

**Organizational**:
- [ ] What team capability does this reveal?
- [ ] Where are we strong/weak?
- [ ] What's limiting growth?
- [ ] What hires are most critical?

---

## NOTES

- **Context Matters**: This analysis is more valuable when paired with competitive intelligence and user research from your research-system.
- **Revision**: Treat findings as hypotheses to test, not conclusions. Revisit after major product changes.
- **Specificity**: More detailed codebase input = more specific insights. Generic summaries produce generic conclusions.
- **Sub-Agents**: Use only when that specific dimension is critical to your current decision.
