# Codebase Business Analysis Prompt
## SUB-AGENT DEEP DIVE DECISION GUIDE

This guide helps you decide which optional sub-agents to include in your final prompt based on your business priorities.

---

## SUB-AGENT A: Data Model & Moat Analysis

### When to Include This

**Include if ANY of these apply:**
- ✓ Data accumulation is a key competitive advantage (user behavior, relationships, content)
- ✓ You have high customer switching costs and need to understand why
- ✓ Planning pricing strategy around data lock-in (per-data model, usage-based)
- ✓ Evaluating data portability features (GDPR, data export as competitive move)
- ✓ Building network effects that compound over time
- ✓ Concerned about competitor data copying (easy to replicate or defensible?)

**Skip if:**
- ✗ Product is stateless/functional (no meaningful data accumulation)
- ✗ Switching costs are about integration/workflow, not data
- ✗ Data is transactional only (not a moat)
- ✗ You already know your data advantages cold

### What You'll Discover

**Data Lock-in Mechanisms**:
- Implicit lock-in: Historical data that's valuable but expensive to migrate (communication logs, transaction history, relationship graphs)
- Explicit lock-in: API design that makes data export difficult
- Contextual lock-in: Data only valuable *within* your system (learned models, analytics, scored relationships)

**Schema as Competitive Signal**:
- Normalized design = horizontal/general-purpose (competes on features)
- Denormalized design = vertical-specific or performance-critical (domain expertise defensibility)
- Flexible schema = planned expansion TAM
- Rigid schema = current focus, less expansion planned

**Network Effects**:
- What makes the product more valuable as more users join? (collaboration features, marketplaces, social graphs)
- Does schema architecture support it? (user relationships, cross-user data, feed systems)

**Compliance Risk/Opportunity**:
- GDPR-ready (data subject rights implemented) = European expansion ready
- Audit trails embedded = enterprise-ready
- Data retention policies = regulated industry prepared
- Absence signals = potential liability

### Real-World Examples

**Example 1: SaaS CRM**
- Core lock-in: 2+ years of customer interaction history (emails, calls, deals)
- Switching cost: Rebuilding that history = 6-12 months of new customer context
- Moat strength: Strong—data gets better over time
- Signal: Foreign key design around communication logs and relationship timestamps

**Example 2: Developer Tool**
- Core lock-in: Integration with 20+ vendor APIs in their ecosystem
- Switching cost: Not data, but workflow migration
- Moat strength: Weak if data alone—strong if network effects around template sharing
- Signal: User-generated content tables, collaboration features

**Example 3: Marketplace**
- Core lock-in: Seller reputation, pricing history, buyer preferences
- Switching cost: Seller network effect—platform becomes more valuable with more buyers
- Moat strength: Strong with two-sided network, weak without
- Signal: Many-to-many relationships, recommendations engine, review aggregation

### Sub-Agent A Prompt

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

## SUB-AGENT B: Pricing & Unit Economics Analysis

### When to Include This

**Include if ANY of these apply:**
- ✓ Uncertain about pricing model (per-user vs. per-transaction vs. per-data)
- ✓ Margin pressure or unit economics concern
- ✓ Planning to introduce usage-based pricing
- ✓ Infrastructure costs are unpredictable or scaling non-linearly
- ✓ Considering feature-based tiers but unsure what features to gate
- ✓ Revenue metrics don't align with delivery cost (losing money on some customers)
- ✓ Competitor pricing strategy unclear and you need to infer intent from code

**Skip if:**
- ✗ Pricing model is already locked in and working
- ✗ You have strong unit economics visibility (you already instrument this)
- ✗ Cost structure is simple and well understood
- ✗ Not considering pricing changes in next 6-12 months

### What You'll Discover

**Instrumentation Gaps**:
- What *could* be metered but isn't (hidden revenue opportunity)
- What *is* metered but not monetized (free value given away)
- What's impossible to meter (indicates usage-based pricing won't work)

**Cost Structure**:
- High-margin features (low cost to deliver, high customer value)
- Low-margin features (high cost, little customer value—candidate for removal)
- Fixed costs vs. variable costs (influences pricing model)
- Scaling cliffs (compute, storage, support costs that jump at certain sizes)

**Pricing Model Alignment**:
- Per-user pricing signals: User management, auth, per-user quotas, org limits
- Per-transaction pricing signals: Event tracking, idempotent keys, billing integration
- Per-compute pricing signals: Job tracking, resource allocation, usage metering
- Per-data pricing signals: Storage limits, query counting, bandwidth tracking
- Feature-based pricing signals: Feature flags, admin panels, tier management

**Monetization Failures**:
- Where value exists but isn't captured (e.g., power users using it for free)
- Pricing that doesn't match delivery cost (unsustainable)
- Feature gaps that force customers downward (people can't afford tier they need)

### Real-World Examples

**Example 1: API Platform**
- Instrumentation: Detailed call tracking, latency per endpoint, error rates
- Cost driver: Compute per request (scale non-linearly during spikes)
- Pricing signal: Per-call pricing with volume discounts
- Problem identified: Support costs spike for enterprise customers but not captured

**Example 2: Analytics SaaS**
- Instrumentation: Data ingestion volume, query volume, storage size
- Cost driver: Storage (scales linearly with customer data) + compute (scales with queries)
- Pricing signal: Hybrid—per GB stored + per-query pricing
- Problem identified: Can't meter expensive aggregation queries, losing margin on heavy users

**Example 3: Collaboration Tool**
- Instrumentation: Active user count, API calls, storage
- Cost driver: Compute (doesn't scale with users) + storage (scales with content)
- Pricing signal: Per-user pricing (easy to understand, predictable revenue)
- Problem identified: Per-user doesn't match cost structure; large teams with little content subsidize small teams with lots

### Sub-Agent B Prompt

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

## SUB-AGENT C: Go-to-Market & Distribution Analysis

### When to Include This

**Include if ANY of these apply:**
- ✓ Uncertain whether to pursue product-led or sales-led growth
- ✓ Low activation/onboarding and need to understand friction points
- ✓ Expansion revenue is lower than expected (usage grows but revenue doesn't)
- ✓ Competitor has different GTM model and you need to understand tradeoffs
- ✓ Planning international expansion and need to assess self-serve feasibility
- ✓ Retention is the problem and you need to understand value visibility
- ✓ Viral/network growth mechanisms haven't kicked in despite building them

**Skip if:**
- ✗ GTM model is clear and working (product-led is working, or sales team is efficient)
- ✗ You have strong activation and engagement metrics already
- ✗ Not considering GTM changes in next 6-12 months
- ✗ Product doesn't have choice (e.g., regulated industry with mandatory sales engagement)

### What You'll Discover

**Product-Led vs. Sales-Led Signals**:
- Freemium/free trial infrastructure = product-led bet
- Admin/compliance tools = sales-led/enterprise bet
- Time-to-value in onboarding = how much you can compress sales cycle
- Quota systems and upgrade flows = where monetization happens

**Activation Friction**:
- What must users do before they experience value?
- What could be eliminated (Aha Moment before signup)
- What's intentionally complex (self-selection, qualification)

**Expansion Bottleneck**:
- Per-user growth: Does product encourage team collaboration?
- Feature expansion: Does product reveal more value as customers mature?
- Data expansion: Does product become more valuable as data accumulates?
- Ecosystem expansion: Do integrations drive usage or just enable it?

**Retention Drivers**:
- Habit formation: Daily/weekly usage loops?
- Switching cost: High friction to leaving or low?
- Value decay: Does product degrade without ongoing usage?
- Re-engagement: Are there mechanisms to fight churn?

**Distribution Economics**:
- Is distribution cost falling as you scale (network effects) or rising (CAC inflation)?
- Can you go international at low marginal cost (product-led) or high (sales-led)?
- Is growth viral (organic, low CAC) or paid (CAC scaling)?

### Real-World Examples

**Example 1: Product-Led SaaS (Figma)**
- Signal: Freemium with generous free tier, easy sharing of designs
- Aha Moment: Share a file link, non-user sees design in browser
- Expansion: Team collaboration, org limits, admin tools unlock expansion
- Retention: Daily design work habit, switching cost (designs stored in Figma)

**Example 2: Sales-Led Enterprise (Salesforce)**
- Signal: Complex setup, heavy admin tools, compliance features
- Time-to-value: Months (customization, training, change management)
- Aha Moment: Delayed (enterprise implementations are long)
- Expansion: Ecosystem play (AppExchange), team scaling (per-user pricing)

**Example 3: Hybrid GTM (Slack)**
- Signal: Freemium for team experimentation, but admin/compliance for scaling
- Aha Moment: First integrated bot/workflow (quick, easy)
- Expansion: Team adoption (viral-ish), ecosystem (app integrations)
- Retention: Becomes mission-critical once adopted (high switching cost)

### Sub-Agent C Prompt

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

## SUB-AGENT D: Organizational & Execution Capability Analysis

### When to Include This

**Include if ANY of these apply:**
- ✓ Planning to scale team (need to understand current capability ceiling)
- ✓ Concerned about technical debt or execution quality
- ✓ Evaluating whether to hire engineers vs. partners vs. outsource
- ✓ Preparing for fundraising (investors will assess execution capability)
- ✓ Comparing your engineering velocity to competitors
- ✓ Figuring out what's slowing product development (team constraint vs. architecture constraint)
- ✓ Planning major architectural changes and need to know if team can execute

**Skip if:**
- ✗ Team is stable and execution is not a constraint
- ✗ Not planning significant growth in headcount
- ✗ Not evaluating org structure changes
- ✗ Technical execution is clearly not the bottleneck (market/product-market fit is)

### What You'll Discover

**Team Capacity**:
- Codebase complexity implies team size (10K lines = 1 person, 500K lines = 5+ people)
- Specialization level reveals whether you have depth or breadth
- Onboarding difficulty shows how fast new engineers become productive

**Execution Velocity**:
- Code quality (technical debt level) impacts shipping speed
- Test coverage indicates stability vs. velocity tradeoff
- Documentation reveals knowledge distribution (concentrated vs. shared)
- Deployment frequency indicates iteration pace

**Engineering Maturity**:
- Architecture patterns: ad-hoc (early stage) vs. intentional (scaling stage)
- Standards compliance: testing, code review, monitoring
- Scaling experience: have they done this before?
- Operational excellence: how production-ready is this?

**Risk Tolerance**:
- Experimentation infrastructure (feature flags, A/B testing) = data-driven culture
- Error handling and graceful degradation = production-conscious
- Security posture = early decision to invest
- Compliance-ready (yes/no) = regulatory market consideration

**Organizational Decisions**:
- Build vs. Buy: What functions are outsourced vs. built?
- Centralized vs. Distributed: Monolith vs. microservices (team org implication)
- Autonomy level: How independent are teams?

**The Hidden Constraint**:
- If code is clean/well-organized but rarely ships = product/design is bottleneck
- If code is messy but ships fast = engineering velocity is not constraint
- If team is small but codebase is large = hero culture or high-leverage engineer
- If team is large but codebase is small = product is constrained, not engineering

### Real-World Examples

**Example 1: Early-Stage Startup (5 engineers, $1M ARR)**
- Code quality: Variable (some polished, some hacky)
- Technical debt: Moderate—shortcuts taken for speed
- Scaling readiness: Can handle 5x growth before architecture breaks
- Constraint: Feature requests >> engineering capacity
- Hiring need: Generalists first (can wear multiple hats)

**Example 2: Growth-Stage SaaS (25 engineers, $10M ARR)**
- Code quality: High (standards enforced)
- Technical debt: Low (paying it down)
- Scaling readiness: Can handle 2x growth, then needs infrastructure work
- Constraint: Specific expertise (Kubernetes ops, iOS, database optimization)
- Hiring need: Specialists (deep expertise in high-leverage areas)

**Example 3: Enterprise Software (100 engineers, $100M ARR)**
- Code quality: High (enterprise standards)
- Technical debt: Low (compliance and stability critical)
- Scaling readiness: Built for scale
- Constraint: Organization and communication
- Hiring need: Senior/leadership (team lead, architect, platform)

### Sub-Agent D Prompt

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

## SUB-AGENT E: Market Opportunity & TAM Analysis

### When to Include This

**Include if ANY of these apply:**
- ✓ Uncertain about addressable market size or growth potential
- ✓ Considering market expansion (geographic, vertical, use case)
- ✓ Competitive market and need to understand positioning vs. substitutes
- ✓ Planning to expand beyond current customer segment
- ✓ Evaluating whether to go upmarket (enterprise) or down-market (SMB)
- ✓ International expansion is next priority
- ✓ Deciding between multiple markets and need to rank them

**Skip if:**
- ✗ Market opportunity is already well understood (you have market research)
- ✗ Not planning expansion beyond current market in next 12-18 months
- ✗ Market is clearly huge and not a limiting factor
- ✗ Market research is a separate function (business dev, product marketing)

### What You'll Discover

**Market Size Signals from Code**:
- Data model complexity indicates target customer complexity (SMB vs. enterprise)
- Feature set complexity indicates vertical-specific (healthcare, finance) vs. horizontal (general SaaS)
- Pricing model hints at market (per-user = large teams; per-transaction = high volume)
- Localization/compliance features reveal geographic bets

**Customer Segment Fitness**:
- Primary segment: Who is product built for (implied by UX, features, price)?
- Secondary segments: Who could use this with minimal changes?
- Blocked segments: What customer segments need features this doesn't have?
- Segment economics: Which segment has best LTV/CAC ratio?

**Use Case Expansion**:
- Core use case: Primary problem being solved
- Adjacent use cases: Related problems not addressed
- Horizontal expansion: Different industries/teams with same problem
- Vertical expansion: Different company sizes/stages with same problem

**Market Maturity & Competition**:
- Nascent market: Few competitors, consolidation unlikely, high growth but uncertain
- Emerging market: Growing competitors, consolidation starting, opportunity window narrowing
- Mature market: Incumbent consolidation, differentiation on feature parity, margin compression
- Consolidating market: Race-to-bottom, exit window closing

**Expansion Economics**:
- Can you expand geographically at low cost (product-led) or requires localization?
- Can you go upmarket (enterprise) or trapped at SMB by GTM model?
- Can you serve adjacent use cases or requires significant product work?
- Are margins better upmarket or down-market?

### Real-World Examples

**Example 1: Marketplace (eBay-like)**
- Core TAM: Used goods buyers/sellers (huge addressable market)
- Primary segment: Collectors, hobbyists
- Secondary segments: Small resellers, small businesses
- Expansion: International (requires localization + payment infrastructure)
- Threat: Vertical-specific marketplaces (domain expertise, liquidity)

**Example 2: B2B SaaS (Zendesk-like)**
- Core TAM: Customer service departments (large, multiple per company)
- Primary segment: SMB (easy to sell, quick ROI)
- Secondary segments: Enterprise (better margins, longer sales cycle)
- Expansion: Adjacent use cases (HR, finance, sales)
- Threat: Incumbent CRM platforms (Salesforce) entering space

**Example 3: Developer Tool (GitHub-like)**
- Core TAM: Software developers (growing, large)
- Primary segment: Individual developers, small teams
- Secondary segments: Enterprise engineering orgs
- Expansion: Adjacent use cases (collaboration, code review)
- Threat: Cloud platforms (AWS, Azure) bundling competitive features

### Sub-Agent E Prompt

```
Based on the codebase, analyze market opportunity:

**Requires context from**: research-system/research/market/ and research-system/research/users/

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

## DECISION MATRIX: Which Sub-Agents to Include

| Your Priority | Sub-A | Sub-B | Sub-C | Sub-D | Sub-E |
|--------------|-------|-------|-------|-------|-------|
| **Defensibility** (moat, competition) | ✅ INCLUDE | ✅ INCLUDE | ⚠️ Maybe | ⚠️ Maybe | ✅ INCLUDE |
| **Pricing/Monetization** (revenue model) | ⚠️ Maybe | ✅ INCLUDE | ⚠️ Maybe | ❌ Skip | ❌ Skip |
| **Growth/Scaling** (expansion strategy) | ❌ Skip | ⚠️ Maybe | ✅ INCLUDE | ✅ INCLUDE | ✅ INCLUDE |
| **Operations/Execution** (team building) | ❌ Skip | ⚠️ Maybe | ❌ Skip | ✅ INCLUDE | ❌ Skip |
| **Fundraising/Due Diligence** | ✅ INCLUDE | ✅ INCLUDE | ✅ INCLUDE | ✅ INCLUDE | ✅ INCLUDE |
| **Product Roadmap** (what to build) | ⚠️ Maybe | ⚠️ Maybe | ✅ INCLUDE | ⚠️ Maybe | ✅ INCLUDE |

---

## RECOMMENDED COMBINATIONS

### Minimal (Master Analysis Only)
- **Use when**: You're doing a quick quarterly business review
- **Time**: 30-60 minutes
- **Output**: Executive summary of all 7 dimensions

### Focused: Defensibility & Competition
- **Include**: Sub-A (Data/Moat) + Sub-E (Market/TAM)
- **Use when**: Evaluating competitive position, fundraising, M&A
- **Time**: 2-3 hours
- **Output**: Moat assessment + competitive positioning

### Focused: Growth & Expansion
- **Include**: Sub-C (GTM) + Sub-E (Market/TAM) + Sub-D (Execution)
- **Use when**: Planning scaling, next market expansion, GTM pivot
- **Time**: 2-3 hours
- **Output**: Growth constraints + expansion opportunities

### Focused: Monetization & Economics
- **Include**: Sub-B (Pricing) + Sub-A (Data Lock-in)
- **Use when**: Revenue optimization, pricing change, margin compression
- **Time**: 2-3 hours
- **Output**: Unit economics + pricing recommendations

### Comprehensive (All Sub-Agents)
- **Include**: All 5 sub-agents
- **Use when**: Major strategic decision, significant pivot, fundraising
- **Time**: 4-6 hours
- **Output**: Complete strategic assessment with all dimensions

---

## YOUR DECISION GUIDE

**Ask yourself:**

1. **What's the biggest uncertainty facing this business right now?**
   - Defensibility? → Sub-A + E
   - Monetization? → Sub-B + A
   - Growth? → Sub-C + E
   - Execution? → Sub-D
   - Everything? → All 5

2. **What decision am I making with this analysis?**
   - Pricing change → Sub-B
   - Market expansion → Sub-E
   - Fundraising pitch → All 5
   - Hiring plan → Sub-D
   - Product roadmap → Sub-C + E

3. **How much time do I have?**
   - 1 hour → Master analysis only
   - 2-3 hours → Master + 2 sub-agents
   - 4+ hours → Master + all sub-agents you need

---

**Recommendation for your setup**: Start with the master analysis. After your first pass, you'll know which sub-agents would provide the most valuable insights for your specific product and market position. You can always run deep dives later without re-analyzing the entire codebase.
