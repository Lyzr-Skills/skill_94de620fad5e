# First Principles Thinking -- Real-World Examples

Examples from SpaceX, Tesla, and other domains that build intuition for the
methodology beyond software.

---

## SpaceX: Rocket Cost Reduction

### The Conventional View

"Rockets are expensive. A Falcon 9-class rocket costs $60-100 million because
that's what aerospace companies charge."

### First Principles Breakdown

**Step 1: What is a rocket made of?**
Aerospace-grade aluminum, titanium, copper, carbon fiber, engines (metal, sensors, pumps).

**Step 2: What do these materials cost?**
Raw materials for a rocket: approximately $2 million. That's 2% of the quoted price.

**Step 3: Why the 50x markup?**

| Factor | Conventional Approach | First Principles Challenge |
|--------|----------------------|---------------------------|
| Manufacturing | Outsource to aerospace suppliers | Build in-house, control process |
| Design | Over-engineer with massive safety margins | Design for reusability, iterate fast |
| Workforce | Specialized aerospace contractors | Hire smart generalists from other industries |
| Reusability | Expendable (new rocket each launch) | Why throw away a $60M machine after one use? |

**Ground Truths:**
1. Physics allows reusable rockets (nothing fundamental prevents it)
2. Materials cost ~$2M
3. Manufacturing efficiency gains are possible with modern techniques
4. The aerospace industry had no competitive pressure (government contracts)

**Result:** SpaceX builds rockets for ~$30M and reuses them, achieving 10x+ cost reduction.

---

## Tesla: Battery Cost

### The Conventional View

"Battery packs cost $600/kWh. Electric cars will always be more expensive than
gas cars."

### First Principles Breakdown

**Step 1: What is a battery made of?**
Cobalt, nickel, aluminum, carbon, polymers for separators, steel for casing.

**Step 2: What do these materials cost on commodity markets?**
Raw materials: approximately $80/kWh.

**Step 3: Why the 7x+ markup?**

| Factor | 2010 Status | First Principles Opportunity |
|--------|-------------|------------------------------|
| Scale | Low volume production | Build gigafactory, achieve economy of scale |
| Design | Laptop cells repurposed for cars | Design cells specifically for EVs from scratch |
| Supply chain | Multiple intermediaries | Vertical integration, cut out middlemen |
| Chemistry | Commodity cathode formulations | R&D into new chemistries optimized for EVs |

**Ground Truths:**
1. Materials cost ~$80/kWh at commodity prices
2. Manufacturing costs scale down with volume
3. Cell design can be optimized for the EV use case
4. Chemistry improvements are possible (and ongoing)

**Result:** Tesla achieved ~$100/kWh by 2023, making EVs cost-competitive with
gas cars.

---

## Lessons for Software Engineers

### Lesson 1: Question the Industry Standard

**Aerospace said:** "Rockets are inherently expensive."
**Reality:** The industry had no incentive to reduce costs.

**Software parallels:**
- "You need Kubernetes for modern deployment" -- Do you have the scale to
  justify the complexity?
- "Microservices are best practice" -- Do you have the team size to justify
  coordination overhead?
- "Use framework X because it's popular" -- Does popularity mean it fits
  your actual constraints?

### Lesson 2: Materials vs. Finished Product

**Rocket materials:** $2M. **Finished rocket:** $60M. (30x markup)
**Battery materials:** $80/kWh. **Pack cost:** $600/kWh. (7.5x markup)

**Software parallels:**
- Open source components are free -- why is enterprise software expensive?
- Computing power is cheap -- why are cloud bills so high?
- Network bandwidth is commoditized -- why do CDNs charge premium rates?

**The markup comes from:** integration complexity, support overhead, marketing,
inefficiency, and lack of competition. First principles asks: which of these
markups are you paying unnecessarily?

### Lesson 3: Reusability Changes Economics

**Rockets:** Reusing a booster 10 times means 10x cost reduction per launch.

**Software -- what are we "throwing away" after single use?**
- Test data and test environments rebuilt from scratch each time
- Environment configurations manually recreated
- Manual processes that could be automated once
- Knowledge that isn't documented and must be re-discovered

### Lesson 4: Build vs. Buy Decision Framework

**SpaceX:** Builds 70% of components in-house (vs. 30% industry standard).
**Tesla:** Builds battery cells, not just packs.

**First Principles Test for software:**
- If this component is core to your differentiation AND you'll iterate on
  it frequently: **build it**.
- If it's commodity functionality AND stable: **buy it** (or use open source).
- If you're unsure: start with buying, switch to building only when the
  bought solution becomes a genuine constraint.

---

## When This Approach Takes Longer

SpaceX and Tesla took 10+ years to achieve their results. First principles
solutions often require more upfront investment.

**Software lesson:** For MVPs and fast iteration, the conventional path is
often correct. First principles is most valuable when:
- The conventional solution is 10x wrong (not 2x wrong)
- You have time to invest upfront for long-term payoff
- You have domain expertise to avoid naive re-derivation

The goal is not to re-derive HTTP before writing an API endpoint. The goal is
to notice when you're about to add Kubernetes to a single-server application
because "that's what everyone does."
