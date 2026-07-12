# First Principles Thinking -- Worked Examples

Four examples showing the full analysis flow across different engineering domains.

---

## Example 1: Infrastructure -- "Should we add a Redis cache?"

### Phase 1 -- Intake

> "You want to add Redis to cache API responses because the endpoint is slow. Is that right?"

### Phase 2 -- Socratic Questioning

- *Assumption probing:* "Why caching specifically? Have you profiled where the time is spent?"
- *Evidence:* "What's the current latency, and what's the target? Is there measured data?"
- *Alternative viewpoints:* "What if the slowness is in the query itself, not the lack of a cache?"
- *Implications:* "If we add a cache, what's the invalidation strategy? What happens when cached data goes stale?"

### Phase 3 -- Decomposition

**Ground Truths:**
- `[TRUTH]` P99 latency must be under 200ms for this endpoint (SLA requirement)
- `[TRUTH]` The endpoint currently takes 800ms on average
- `[TRUTH]` The data changes roughly every 30 minutes

**Assumptions Challenged:**

| Assumption | Challenge | Verdict |
|------------|-----------|---------|
| We need an external cache | The bottleneck might be the query, not the absence of a cache | Investigate |
| Redis is the right tool | Application-level memoization might suffice for this data size | Investigate |
| All responses need caching | Maybe only 3 of 20 endpoints are actually slow | Discard |

### Phase 4 -- Reconstruction

- **Path A:** Fix the N+1 query (add eager loading, add a database index). Measure again. Cost: 2 hours. Reversibility: trivial.
- **Path B:** Add application-level memoization with TTL. No new infrastructure. Cost: 1 hour. Limits: single-process, won't help if you scale horizontally.
- **Path C:** Add Redis. Full caching layer with invalidation. Cost: 1-2 days including ops setup. Gives you horizontal scaling but adds operational complexity.

### Recommended Approach

> Start with Path A. Profile the query, fix obvious issues, measure. If latency is still
> above 200ms, try Path B (memoization). Only reach for Path C (Redis) if you have evidence
> that the simpler solutions aren't enough. Each step is cheap and reversible.

---

## Example 2: Application Architecture -- "We need microservices"

### Phase 1 -- Intake

> "You want to break the monolith into microservices because the codebase is getting hard to work with. Is that right?"

### Phase 2 -- Socratic Questioning

- *Clarification:* "What specifically is hard about working with it? Slow builds? Merge conflicts? Unclear ownership?"
- *Assumption probing:* "Does 'hard to work with' mean the architecture is wrong, or that the code needs refactoring within the current structure?"
- *Evidence:* "How many engineers are working on this codebase? How often do teams block each other?"
- *Meta:* "Would a well-structured modular monolith solve the same problems without the operational cost of microservices?"

### Phase 3 -- Decomposition

**Ground Truths:**
- `[TRUTH]` Two teams (Payments and Catalog) ship on different release cycles and block each other
- `[TRUTH]` The deployment pipeline takes 45 minutes for the full monolith
- `[TRUTH]` The database has clear domain boundaries (payments tables vs catalog tables)

**Assumptions Challenged:**

| Assumption | Challenge | Verdict |
|------------|-----------|---------|
| The whole thing needs to be broken apart | Only Payments and Catalog have genuine independence needs | Discard |
| Microservices will make the codebase easier to understand | They add network boundaries, distributed tracing, and service mesh complexity | Discard |
| We need independently deployable services | We need independent release cycles, achievable with modular monolith + feature flags | Modify |

### Phase 4 -- Reconstruction

- **Path A:** Extract only Payments into its own service (clear domain boundary, different release cycle, own database tables). Keep everything else as a modular monolith. Cost: 2-3 weeks. Risk: manageable, one service boundary.
- **Path B:** Restructure into a modular monolith with enforced module boundaries (Gradle/Maven modules, or package-level boundaries). Independent CI pipelines per module. Cost: 1-2 weeks. Risk: low, no new infrastructure.
- **Path C:** Full microservices extraction. Break into 5+ services with API gateway, service mesh, distributed tracing. Cost: 2-3 months. Risk: high operational complexity for a team this size.

### Recommended Approach

> Start with Path B (modular monolith) to get the code organization benefits immediately.
> Then extract Payments (Path A) if the release cycle conflict persists after modularization.
> Path C is premature for a team of this size -- microservices solve organizational scaling
> problems, and those aren't present yet.

---

## Example 3: Authentication -- "Let's implement OAuth2 with JWT"

### Phase 1 -- Intake

> "You want to implement OAuth2 with JWT tokens for user authentication. Is that right?"

### Phase 2 -- Socratic Questioning

- *Clarification:* "Are you federating identity with third-party providers, or is this internal auth only?"
- *Assumption probing:* "Why OAuth2? Do you need the authorization delegation that OAuth2 provides, or just authentication?"
- *Evidence:* "How many users? Is this a single deployment or distributed across multiple services?"
- *Meta:* "What's the simplest thing that correctly verifies user identity and controls access?"

### Phase 3 -- Decomposition

**Ground Truths:**
- `[TRUTH]` Single application, no third-party authentication needed
- `[TRUTH]` Users are employees (known, bounded set of ~200 people)
- `[TRUTH]` Single server deployment (not distributed)
- `[TRUTH]` Session duration matches the workday (~8 hours)

**Assumptions Challenged:**

| Assumption | Challenge | Verdict |
|------------|-----------|---------|
| Need OAuth2 | OAuth2 is for delegated authorization and identity federation. No federation here. | Discard |
| JWT is necessary for stateless auth | Single server means server-side sessions are viable and simpler | Discard |
| Need refresh tokens | Simple session timeout works for workday-length sessions | Discard |
| Need a third-party auth library | With no federation, the auth logic is straightforward | Investigate |

### Phase 4 -- Reconstruction

- **Path A:** Server-side sessions with secure cookies. ~100 lines of code. No external dependencies. Simple to debug and maintain. Limits: tied to a single server, won't work if you go distributed later.
- **Path B:** Simple token-based auth (custom JWT, no OAuth2 framework). Stateless, works across multiple servers if needed. ~300 lines. Limits: you own the token lifecycle logic, need to handle expiry and revocation yourself.
- **Path C:** Full OAuth2 with JWT and refresh tokens. ~1000+ lines or a framework dependency. Gives you federation readiness and industry-standard token flows. Limits: massive overkill for 200 internal users on a single server.

### Recommended Approach

> Path A (server-side sessions). The ground truths (single app, single server,
> known user set, workday sessions) eliminate every reason to reach for OAuth2.
> Path B is a reasonable middle ground if horizontal scaling is on the 6-month roadmap.
> Path C is premature -- revisit only if you add third-party login or federate identity.

---

## Example 4: Database Selection -- "Let's use PostgreSQL"

### Phase 1 -- Intake

> "You're choosing a database for a new service and defaulting to PostgreSQL. Is that right?"

### Phase 2 -- Socratic Questioning

- *Clarification:* "What are your actual data access patterns? Relational queries, key-value lookups, full-text search?"
- *Assumption probing:* "Why a relational database? Do you have relational data, or are you choosing PostgreSQL because it's familiar?"
- *Evidence:* "What's the expected read/write ratio? What's the latency requirement?"
- *Alternative viewpoints:* "If you look at your last 20 queries, how many are joins vs simple lookups by ID?"

### Phase 3 -- Decomposition

**Ground Truths:**
- `[TRUTH]` Data must persist across restarts
- `[TRUTH]` Read latency must be under 50ms at P99
- `[TRUTH]` Write volume is ~100 writes/second
- `[TRUTH]` Data is mostly read, rarely updated
- `[TRUTH]` 90% of access is key-value lookup by ID

**Assumptions Challenged:**

| Assumption | Challenge | Verdict |
|------------|-----------|---------|
| Need a traditional RDBMS | 90% of access is key-value. Relational model may be overkill. | Investigate |
| Need ACID transactions | Only a small subset of operations need atomicity | Modify |
| Need SQL for querying | Most queries are simple ID lookups, not relational joins | Discard |
| PostgreSQL is the safe default | Safe, but potentially over-provisioned for this access pattern | Investigate |

### Phase 4 -- Reconstruction

- **Path A:** Key-value store (Redis with AOF persistence, or DynamoDB). Optimized for the 90% lookup-by-ID pattern. Sub-10ms reads. Cost: 1-2 days setup. Limits: the 10% relational queries need a separate solution.
- **Path B:** PostgreSQL with a key-value access layer. Use PostgreSQL but access it primarily through simple key lookups, not complex joins. Familiar tooling, single system. Cost: 1 day. Limits: paying RDBMS overhead for key-value work, but operational simplicity may be worth it.
- **Path C:** Hybrid -- key-value store for the 90% hot path, small PostgreSQL or SQLite for the 10% relational queries. Best performance for both patterns. Cost: 2-3 days. Limits: two data stores to maintain, potential consistency concerns between them.

### Recommended Approach

> Start with Path B if the team already knows PostgreSQL and the latency target (50ms P99)
> is comfortably met. Measure first. If PostgreSQL can't hit 50ms for the key-value lookups
> under load, move to Path A or Path C. Don't introduce a second data store until you have
> evidence that one isn't enough.
