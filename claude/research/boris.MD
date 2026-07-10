# Locations Finder: Ownership Case & Marketing Value Roadmap

## Executive Summary

Locations Finder should remain under Bank Marketing Tech ownership. It is not utility infrastructure — it is the highest-intent local marketing surface the bank operates, and its value depends on the serving layer living with the same team that owns the marketing pages consuming it. Splitting ownership creates a dependency seam exactly where personalization needs to be fast, and forfeits our position in an agent-first future where location data becomes a core AI integration surface.

---

## 1. Current State

**What we own today:**

- **Bank site (capitalone.com)** — contributions to /bank: marketing materials, product direct pages, entries into account opening.
- **Locations Finder backend** — ingestion from internal sources of truth, an ElastiCache layer providing 30–40ms access to location data, served via GraphQL API.
- **Locations web front end** — locations.capitalone.com, the consumer-facing location finder app.
- **Bank site integrations** — location data consumed on various bank site pages and components.
- **Customer team relationship** — Services Finder (native iOS/Android in the Capital One apps) consumes our API.
- **Events infrastructure** — the platform where internal teams customize and provision events (e.g., Café events), delivered via its own app. This platform is not under ownership question, but it is directly coupled to Locations Finder: locations carry the Café data that events attach to.

**Key architectural point:** We do not maintain the sources of truth. We consume them, cache them, and serve them fast. The cache exists *because* our consumers need speed — the serving layer and its consumers were designed to co-evolve under one roof.

---

## 2. The Ownership Argument

### 2.1 Organizational friction math

- Today: a marketing-driven change to location data or presentation is **one team, one sprint**.
- After a split: **two backlogs, two prioritization negotiations, and an SLA conversation** for every personalization or marketing feature that touches location data.
- Handoff does not reduce total effort — it redistributes and *inflates* it by adding coordination overhead, while introducing a new team that must build context we already have.

### 2.2 The co-location design principle

The 30–40ms cache is not incidental. It exists to make location data usable at page-load time on marketing surfaces. The consumers (bank site pages, locations web app) and the serving layer are two halves of one system. Splitting them puts an organizational boundary through the middle of a latency-sensitive design.

### 2.3 Events + Locations synergy (stays broken if split)

- We also own the events infrastructure — and events are inherently *located*. Café events attach to Café location data that Locations Finder serves.
- Today, an end-to-end experience (event created internally → enriched into the location cache → surfaced on locations.capitalone.com and bank site) lives **entirely within one team**.
- If Locations Finder leaves, the events platform loses direct access to its most natural distribution channel, and every events-on-locations feature becomes a cross-team dependency. The split doesn't just cost the locations roadmap — it strands the events roadmap too.

### 2.4 Agent-first future

- In an agent-first world, "where's the nearest Café with a coin machine open Sunday?" is answered by an AI assistant calling an API — **ours**.
- Whoever owns the serving layer owns the agent integration surface. Handing it off means every future agent/assistant marketing experience routes through another team's roadmap.
- Concrete near-term proof points we can prototype cheaply (see §4): MCP server, assistant tooling, natural-language search.

---

## 3. Locations as a Marketing Surface

Reframe: Locations Finder is not a utility. It is high-intent, high-SEO marketing real estate with a direct line to account opening.

### 3.1 Localized product marketing on location pages

- Location detail pages carry strong local SEO value and high visit intent.
- Inject market-specific content: Café events, local CD/savings rate promos, "open an account at this branch" CTAs feeding directly into the account opening funnels we already own on /bank.
- Result: locations becomes a marketing channel with **attributable conversions**.

### 3.2 Geo-personalization API for bank site

- Extend the GraphQL API so any bank site page can ask "what's near this visitor?"
- Examples: nearest Café module on the homepage, ATM density messaging on checking product pages, branch availability on product direct pages.
- The 30–40ms cache is exactly what makes this feasible at page-load time — this is the infrastructure argument expressed as a feature.

### 3.3 Local SEO and discovery

- Structured data (schema.org LocalBusiness), consistency with Google Business Profile feeds, landing pages optimized for "bank near me" queries.
- Classic marketing tech work; our ingestion pipeline is the backbone of it.

### 3.4 Café-as-marketing

- Cafés are a brand marketing play by design. Event listings, RSVP flows, and ambassador content on locations.capitalone.com turn the web app into a brand engagement property rather than an address lookup.

### 3.5 Measurement and attribution

- Instrument the funnel: found a location → visited → opened account (even directionally at first).
- If locations traffic demonstrably feeds account opening, the ownership question answers itself.

---

## 4. Backend / API Value-Add Prototypes

| Idea | Description | Why it strengthens the case |
|---|---|---|
| **MCP / agent-ready endpoint** | Wrap the GraphQL API in an MCP server or agent-friendly interface | Positions the team as the agent integration surface for location data |
| **Assistant "locations tool"** | A tool the bank's AI assistant can call for location answers | Cheap demo — the hard part (fast serving) already exists |
| **Enriched location intelligence layer** | Add marketing-relevant attributes to the cache: Café event schedules, local offers, appointment availability | Turns a "dumb cache" into a location intelligence platform that clearly belongs in marketing tech |
| **Events-to-locations pipeline** | Auto-flow events from our events platform into the location cache and out through the GraphQL API | Zero cross-team dependencies — we own both ends. Fastest possible proof of the integrated-ownership value |
| **Geo-personalization endpoints** | "What's near this visitor" queries optimized for page-load use | Directly powers §3.2; showcases why latency + ownership matter together |
| **Embeddable location widgets** | A drop-in component other bank site teams (or partners) can use, powered by our API | More consumers = more entrenchment = stronger ownership case |

---

## 5. locations.capitalone.com Feature Ideas

- **"Plan your visit" flows** — appointment booking entry points, what-to-bring checklists for account opening. Moves users deeper into the funnel instead of stopping at address lookup.
- **Personalized results** — signed-in users see relevant services first (their card → nearest fee-free ATM network; prospects → Cafés and branches with bankers).
- **Café events hub** — searchable events, RSVPs, add-to-calendar. Turns a utility into a repeat-visit destination. Uniquely low-lift for us: we own the events provisioning platform, so this is an internal integration, not a cross-team ask.
- **Smart / natural-language search** — "notary near me open now" instead of filter checkboxes. AI-flavored demo that showcases the API and is a stepping stone to the agent story.
- **Travel mode** — "where can I use my card fee-free on my trip": ATM network coverage abroad. Genuinely useful, marketing-friendly, and differentiating.

---

## 6. The Through-Line for the Pitch

Every idea in this document is fast to prototype **only if** the team building the experience also owns the data layer. That is the ownership argument expressed in feature form:

1. **Locations is a marketing surface**, not infrastructure — high intent, local SEO, direct funnel into account opening.
2. **The serving layer and its consumers are one system** — splitting them adds friction precisely where speed matters.
3. **Events and locations are one story** — we own both platforms; splitting locations severs the events roadmap from its distribution channel.
4. **The agent-first future runs through this API** — owning it means owning the next generation of location-driven marketing experiences.

**Recommended next step:** the events-to-locations pipeline (§4) may be the single strongest first prototype — it requires zero cross-team coordination, produces a visible marketing feature (Café events hub, §5), and demonstrates exactly what integrated ownership makes possible. Pair it with a one-page attribution plan (§3.5) for the senior director conversation.
