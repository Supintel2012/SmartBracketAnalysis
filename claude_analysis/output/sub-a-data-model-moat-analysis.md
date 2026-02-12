# SmartBracket Data Model & Defensibility Moat Analysis
## Sub-Agent A Analysis Report

**Generated:** January 2026
**Analyst:** Claude Code Business Analysis Framework

---

## Executive Summary

SmartBracket is a tournament bracket prediction system built on patented Rapid Recursive decision intelligence technology. The data model is relatively simple, with the core competitive advantage residing almost entirely in the **algorithm and its underlying probability data**, not in accumulated user data. The business has generated $2,107 from 445 users over 5 years (~$4.73/user), indicating minimal traction despite strong algorithmic performance.

---

## 1. Entity Mapping

### Core Entities

| Entity | Schema Location | Purpose |
|--------|-----------------|---------|
| **User** | `Application/models/User.js` | Account management, payment status |
| **Bracket** | `Application/models/Bracket.js` | Generated bracket predictions |
| **tempPage** | `Application/models/tempPage.js` | Temporary shareable bracket pages |

### Relationship Graph

```
User (1) -----> (N) Bracket
         lastBracketID (points to most recent)

Bracket (1) <---- (1) tempPage
              bracketID (for sharing)
```

### User Schema Fields

```javascript
{
  email: { type: String, unique: true },
  password: String,  // MD5 hashed (SECURITY ISSUE)
  passwordResetToken: String,
  passwordResetExpires: Date,
  paid: { type: Boolean, default: false },
  lastBracketID: ObjectId,
  appPlatform: String,
  created_at: Date,
  fb_id: String,
  twitter_id: String,
  google_id: String,
  oauth_singIn: Boolean,
  discount: Boolean,
  profile: { name, gender, location, favoriteTeam, website, picture }
}
```

### Bracket Schema Fields

```javascript
{
  picks: String,           // JSON array of 63 picks
  R: String,               // Reward matrix from algorithm
  maxV: String,            // Maximum value from optimization
  answer_q1: String,       // User preference input
  answer_q2: String,       // User preference input
  answer_q3: String,       // User preference input
  page_id: Number,
  yearData: Number,        // Tournament year
  user: ObjectId,          // Reference to User
  reqTime: Date
}
```

### Data Uniqueness Assessment

| Data Type                   | Proprietary?       | Notes                           |
| --------------------------- | ------------------ | ------------------------------- |
| User accounts               | No                 | Standard auth data              |
| Bracket picks               | Partially          | Output of proprietary algorithm |
| R (Reward matrix)           | **Yes**            | Algorithm output                |
| maxV (Optimal value)        | **Yes**            | Algorithm performance metric    |
| `P.npy` (transition matrix) | **YES - CRITICAL** | 262KB proprietary probabilities |
| `statChance*.txt` files     | **YES - CRITICAL** | Historical win probabilities    |
| `pickChance*.txt` files     | **YES - CRITICAL** | Crowd pick distributions        |

---

## 2. Data Accumulation

### What Data Grows Over Time?

| Data Category | Growth Pattern | Current State |
|---------------|----------------|---------------|
| User accounts | Linear with signups | ~445 users (5 years) |
| Generated brackets | ~1 per user per year | ~445-500 brackets |
| Algorithm probability files | Manual annual updates | 7 years of data |
| User preferences (q1-q3) | Per bracket | Limited utility |

### Lock-in Mechanism Assessment: **WEAK**

1. **No Export Functionality**: Users cannot export bracket data, but picks are simple (63 numbers)
2. **No Historical Tracking**: Only `lastBracketID` tracked per user
3. **No Social Graph**: No user connections or community features
4. **Annual Reset**: Each tournament year resets user value to zero

### Network Effects: **NONE**

- No user-to-user interactions
- No community betting pools or leagues
- Algorithm does not improve with more users (fixed statistical model)

### Switching Cost Analysis

```
Switching Cost = $5/year + ~10 minutes to re-enter preferences elsewhere
```

Users lose nothing they cannot recreate elsewhere.

---

## 3. Query Patterns

### Critical Queries (from bracket.js)

```javascript
// Most common - lookup by ID (indexed by default)
Bracket.findById(bracketID, function (err, bracket) { ... })
User.findById(req.user._id, function (err, user) { ... })
```

### Indices Present

| Collection | Index | Type |
|------------|-------|------|
| Users | email | Unique |
| Users | _id | Primary |
| Brackets | _id | Primary |
| Brackets | user | **Missing index** |

### Missing Indices (Performance Risk)

```javascript
// No index on yearData for historical queries
yearData: {type: Number, default: new Date().getFullYear()}

// No index on user field for user's brackets
user: {type: Schema.Types.ObjectId, ref: 'User'}
```

### Denormalization: Bracket Caching

```javascript
// Pre-computed "one-click" brackets cached in memory and file
class BracketCache {
  this.brackets = [];
  this.BRACKET_TXT_FILE = "bracketcache.txt";
}
```

Avoids algorithm computation for default picks.

---

## 4. Data Governance

### Security Issues Identified

1. **MD5 Password Hashing** (models/User.js:40) - Cryptographically broken
2. **No Encryption at Rest** - MongoDB data appears unencrypted
3. **API Keys in Environment** - Standard but single point of failure

### Audit Capability: **MINIMAL**

- `created_at` timestamp on User
- `reqTime` timestamp on Bracket
- No update history, access logging, or modification tracking

### Data Deletion Issues

```javascript
// User deletion does NOT cascade to brackets
User.deleteOne({ _id: req.user.id }, (err) => { ... })
```

- No data export before deletion
- No soft delete pattern
- GDPR Article 17 compliance questionable

---

## 5. Moat Verdict

### Data Moat Strength: **WEAK**

| Factor | Score | Rationale |
|--------|-------|-----------|
| Proprietary data accumulation | 2/10 | Only ~445 users, no compound growth |
| Network effects | 0/10 | None present |
| Switching costs | 1/10 | $5/year, trivial to recreate |
| Data uniqueness | 6/10 | Algorithm parameters are proprietary |

### True Competitive Moat: **THE ALGORITHM**

The actual defensibility comes from:

1. **Patents** (Strong Legal Moat):
   - US #9798700, #10460249, #10546248
   - Japan #6284472
   - Korea #10-2082522

2. **Markov Decision Process Implementation**:
   - Value Iteration and Policy Iteration solvers
   - Bellman operator implementations
   - Proprietary reward function construction

3. **Probability Calibration Data**:
   - `P.npy`: 262KB transition probability matrix
   - `statChance*.txt`: Historical win probability models
   - `pickChance*.txt`: Crowd pick distribution data

4. **Track Record**: ~90th percentile performance over 9 years

### Competitive Threat Analysis

**What a New Entrant Would Need:**

1. Patent license or wait for expiration (2033-2035)
2. Access to ESPN pick distribution data
3. Multi-year validation period for credibility
4. Superior UX (current app is basic)

**Vulnerability:**
- ESPN could trivially build this with their data
- Academic researchers publish similar MDP models
- Low revenue suggests market may not find current value proposition compelling

---

## Recommendations

### Data Model Improvements

1. **Add Historical Performance Tracking**:
```javascript
bracketHistory: [{
  year: Number,
  bracketId: ObjectId,
  finalScore: Number,
  percentile: Number
}]
```

2. **Add Social Features**:
```javascript
LeagueSchema = {
  name: String,
  members: [ObjectId],
  commissioner: ObjectId,
  inviteCode: String
}
```

3. **Security Fixes**:
   - Replace MD5 with bcrypt
   - Add proper indices on `user` field
   - Implement cascade delete for GDPR

---

## Critical Files

| File | Purpose |
|------|---------|
| `Application/models/User.js` | User schema - needs security fixes |
| `Application/models/Bracket.js` | Bracket schema - needs indices |
| `Application/RRToolbox-API/app/MarchMadness.py` | Core algorithm - TRUE IP |
| `Application/RRToolbox-API/app/models/smart_bracket_data/P.npy` | Proprietary transition matrix |
