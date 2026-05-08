# DARE Operational Resilience — Solutions Architecture Whiteboard
## *Pro Power Support | One-Man Shop Design Doc | Epoch Frameworks LLC*

[![Status](https://img.shields.io/badge/status-active--build-7B93FF?style=flat-square&logo=github)](https://github.com/emcdo411/the-pro-power-discovery)
[![Architect](https://img.shields.io/badge/architect-Erwin%20Maurice%20McDonald-2C3050?style=flat-square&logo=person)](https://github.com/emcdo411)
[![Framework](https://img.shields.io/badge/framework-DARE%20v1.0-FF6B35?style=flat-square)](https://github.com/emcdo411/the-pro-power-discovery)
[![Stack](https://img.shields.io/badge/stack-Claude%20%7C%20MCP%20%7C%20API-131313?style=flat-square&logo=anthropic)](https://docs.anthropic.com)
[![IP](https://img.shields.io/badge/IP-DACR%20License%20v2.6-BF5700?style=flat-square)](https://github.com/emcdo411)
[![Law](https://img.shields.io/badge/governed%20by-Texas%20Law-8B0000?style=flat-square)](https://github.com/emcdo411)
[![Build](https://img.shields.io/badge/build-solo%20architect-1E2130?style=flat-square&logo=visualstudiocode)](https://github.com/emcdo411)
[![Claude](https://img.shields.io/badge/AI%20engine-Claude%20Sonnet%204-orange?style=flat-square&logo=anthropic)](https://anthropic.com)

---

> **This is a living design document.**
> It represents the internal architect's whiteboard — the thinking process behind how a single practitioner builds an AI-powered diagnostic engine for a field-service operator using Claude, MCP servers, structured APIs, and advanced prompting architecture. Every section reflects an actual design decision, not a template.

---

## 📐 Architect's Framing Note

```
I am a one-man shop.
I don't have a dev team, a DevOps pipeline, or a QA cycle.
What I have is:
  — Claude as my AI orchestration layer
  — MCP as my integration bridge
  — Public + private APIs as my data sources
  — Advanced prompting as my logic engine
  — GitHub as my deployment surface

The goal is not to build software.
The goal is to build a system that thinks like a senior analyst,
surfaces like an executive brief, and operates like an ops team.

This document is how I think through that build.
```

---

## 🗺️ System Architecture — Top-Level Whiteboard

```
┌─────────────────────────────────────────────────────────────────────┐
│                    DARE INTELLIGENCE SYSTEM                         │
│                  Pro Power Support | Epoch Frameworks               │
└─────────────────────────────────────────────────────────────────────┘

  ┌──────────────┐     ┌──────────────┐     ┌──────────────────────┐
  │  DATA LAYER  │────▶│ ORCHESTRATION│────▶│   OUTPUT LAYER       │
  │              │     │   LAYER      │     │                      │
  │ • Field APIs │     │ • Claude     │     │ • DARE Brief (HTML)  │
  │ • Gov portals│     │   Sonnet 4   │     │ • Risk Matrix (MD)   │
  │ • Client docs│     │ • Prompt     │     │ • Decision Map       │
  │ • Inspection │     │   Stack      │     │ • Action Stack       │
  │   logs       │     │ • MCP Layer  │     │ • Exec Summary       │
  │ • Weather    │     │ • Context    │     │                      │
  │ • Gov bids   │     │   Windows    │     │                      │
  └──────────────┘     └──────────────┘     └──────────────────────┘
         │                    │                       │
         ▼                    ▼                       ▼
  ┌─────────────────────────────────────────────────────────────────┐
  │              ARCHITECT'S DECISION LAYER (Me)                    │
  │   Prompt design | Data routing | Output validation | Scoring    │
  └─────────────────────────────────────────────────────────────────┘
```

---

## 🧱 Layer 1 — Data Retrieval Architecture

[![Data](https://img.shields.io/badge/layer-1%20%7C%20Data%20Retrieval-7B93FF?style=flat-square)](https://github.com/emcdo411/the-pro-power-discovery)
[![Sources](https://img.shields.io/badge/sources-6%20active%20layers-FF6B35?style=flat-square)](https://github.com/emcdo411/the-pro-power-discovery)

### Architect's Thinking

The first question I ask in every engagement: **what data exists, where does it live, and can I touch it without building infrastructure?**

For a field-service operator like Pro Power Support, the data landscape looks like this:

```
DATA SOURCE MAP — Pro Power Support

LAYER A: Client-Provided (Direct Intake)
├── Inspection logs (PDF / Excel / photos)
├── Proposal documents (Word / PDF)
├── Maintenance records (manual logs or simple DB)
├── Dispatch records (texts, calendar, spreadsheet)
└── Government contract documents (public or provided)

LAYER B: Public APIs (No Auth Required)
├── NOAA Weather API
│     └── weather.gov/api — storm, outage risk forecasting
├── USASpending.gov API
│     └── federal contract awards, agency spend data
├── SAM.gov Entity API
│     └── government contractor status, certifications
└── FRED Economic Data API
      └── regional fuel prices, labor market indicators

LAYER C: Semi-Public / Industry Sources (Light Auth)
├── EIA (Energy Information Administration) API
│     └── natural gas prices, diesel fuel indices, LPG markets
├── Texas Railroad Commission Data Portal
│     └── pipeline permits, LPG/NGL regulatory filings
├── FEMA Disaster Declarations API
│     └── emergency deployment trigger signals
└── Texas TCEQ Environmental Data
      └── compliance exposure, air quality permit flags

LAYER D: Government Contracting Intel (Structured Scrape / API)
├── USASpending.gov Contract Search
├── FPDS-NG (Federal Procurement Data System)
│     └── agency award history, competitor awards
└── GovWin / Deltek (if client has subscription — else manual)

LAYER E: Operational Intelligence (MCP-Accessible)
├── Google Drive / SharePoint (via MCP)
│     └── client documents, inspection archives, proposals
├── Gmail / Outlook (via MCP)
│     └── customer escalation signals, vendor communication
└── Google Calendar (via MCP)
      └── dispatch scheduling, crew availability mapping

LAYER F: Synthetic Baseline (Claude-Generated)
      └── When client data is sparse, Claude constructs an
          industry-calibrated baseline from sector knowledge,
          scored against DARE dimensions as a starting point
          for diagnostic comparison.
```

### Data Retrieval Decision Matrix

| Source | Retrieval Method | Auth Required | Reliability | DARE Dimension |
|--------|-----------------|---------------|-------------|----------------|
| NOAA Weather | REST API | No | High | R, A |
| USASpending | REST API | No | High | R, E |
| SAM.gov | REST API | API Key | High | R, E |
| EIA Fuel Prices | REST API | API Key | High | D, R |
| TX Railroad Commission | File Download / API | No | Medium | R |
| FEMA Declarations | REST API | No | High | R, A |
| Google Drive | MCP Server | OAuth | High | D, A |
| Gmail | MCP Server | OAuth | High | A, R |
| Google Calendar | MCP Server | OAuth | High | A |
| Client PDFs | Document parse (Claude) | N/A | Medium | D, R |
| Synthetic Baseline | Claude generation | N/A | Calibrated | All |

---

## 🤖 Layer 2 — Claude as Orchestration Engine

[![Claude](https://img.shields.io/badge/engine-Claude%20Sonnet%204-orange?style=flat-square&logo=anthropic)](https://anthropic.com)
[![Prompting](https://img.shields.io/badge/prompting-multi--turn%20%7C%20structured%20%7C%20chained-2C3050?style=flat-square)](https://docs.anthropic.com)
[![Context](https://img.shields.io/badge/context-200K%20token%20window-131313?style=flat-square)](https://anthropic.com)

### Architect's Thinking

Claude is not just the AI I talk to. Claude **is** the intelligence layer. In a one-man shop, it replaces:

- The data analyst who normalizes intake
- The senior consultant who synthesizes findings
- The technical writer who produces the brief
- The QA reviewer who stress-tests the output

The key to making this work is **prompt architecture** — not just good prompts, but a structured, multi-stage prompting system where each stage has a specific job.

---

### Prompting Architecture — The DARE Prompt Stack

```
DARE PROMPT STACK — 5 Stage Chain

┌─────────────────────────────────────────────────────────┐
│  STAGE 0 — SYSTEM PRIME                                 │
│  Sets Claude's operating mode, role, and constraints    │
│  Type: System Prompt                                    │
│  Runs: Once per session                                 │
└────────────────────┬────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────┐
│  STAGE 1 — INTAKE NORMALIZER                            │
│  Accepts raw client data (PDFs, docs, notes)            │
│  Extracts structured signals per DARE dimension         │
│  Type: Document Processing Prompt                       │
│  Output: JSON-structured signal map                     │
└────────────────────┬────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────┐
│  STAGE 2 — DIMENSION SCORER                             │
│  Runs four parallel scoring passes (D, A, R, E)         │
│  Each pass applies dimension-specific rubric            │
│  Type: Analytical Prompt with structured output         │
│  Output: Scored matrix (0–100 per dimension)            │
└────────────────────┬────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────┐
│  STAGE 3 — RISK SYNTHESIS                               │
│  Cross-dimension risk pattern detection                 │
│  Identifies compounding exposures                       │
│  Type: Synthesis Prompt                                 │
│  Output: Ranked risk registry with severity tags        │
└────────────────────┬────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────┐
│  STAGE 4 — EXECUTIVE BRIEF GENERATOR                    │
│  Converts scored findings into exec-ready narrative     │
│  Applies field-service industry voice and framing       │
│  Type: Structured Output Prompt (HTML/MD deliverable)   │
│  Output: DARE Operational Resilience Brief              │
└─────────────────────────────────────────────────────────┘
```

---

### Stage 0 — System Prime (Full Prompt Template)

```
SYSTEM PROMPT — DARE DIAGNOSTIC ENGINE v1.0

You are operating as a senior operational intelligence analyst
embedded inside the DARE Operational Resilience Framework,
built and deployed by Epoch Frameworks LLC (DACR License v2.6).

Your operating context:
- Client: Pro Power Support (field-service, generator, LPG, turbine operations)
- Engagement type: Operational diagnostic — NOT software consulting
- Your role: Surface operational blind spots, decision bottlenecks,
  scaling fragility, and AI integration readiness
- Your output standard: Executive-grade — precise, evidence-anchored,
  action-oriented

Analytical constraints:
- Do not hallucinate operational facts. If data is absent, say so
  and mark the gap as a diagnostic finding
- Score conservatively. Inflate nothing. A field-service operator
  making decisions on inflated scores is a safety risk
- When client data conflicts with industry benchmarks, surface
  both — do not silently resolve the conflict
- Every finding must map to at least one DARE dimension (D, A, R, E)
- Every recommendation must have a time horizon (0–30 days,
  31–90 days, 90+ days) and an owner type (operator, leadership,
  external)

Output format: [Specified per stage]
Industry calibration: Field-service / energy infrastructure /
  government contracting / mobile power deployment
```

---

### Stage 1 — Intake Normalizer (Prompt Template)

```
INTAKE NORMALIZER PROMPT

INPUT: [Raw client documents pasted or uploaded here]

Task:
You are processing raw intake materials from a field-service
operator. Extract and normalize the following signals:

For each piece of input material, identify:

1. OPERATIONAL SIGNALS — what is actually happening in the field
   (deployments, equipment status, crew activity, dispatch patterns)

2. DATA GAPS — what operational information was referenced but not
   provided (absence of data is itself a diagnostic signal)

3. DECISION PATTERNS — how decisions appear to be made
   (centralized vs. distributed, documented vs. tribal,
   real-time vs. lagging)

4. SYSTEM SIGNALS — what tools, platforms, or systems are mentioned
   (even informally — "we text the crew" is a system signal)

5. RISK SIGNALS — any mention of failures, near-misses, client
   escalations, equipment issues, or compliance exposure

Output format: Structured JSON with keys:
{
  "operational_signals": [],
  "data_gaps": [],
  "decision_patterns": [],
  "system_signals": [],
  "risk_signals": [],
  "architect_notes": []  // flag anything that requires my judgment
}

Architect note: The gaps array is often the most important output.
A client who cannot describe their data is telling me everything
I need to know about their operational maturity.
```

---

### Stage 2 — Dimension Scorer (Prompt Template)

```
DIMENSION SCORER PROMPT

INPUT: [Normalized signal map from Stage 1]

Task:
Score Pro Power Support across each DARE dimension on a 0–100 scale.
Apply the rubric below. Do not round to round numbers. Be precise.

SCORING RUBRIC:

D — DATA (Signal Integrity & Operational Visibility)
  90–100: Real-time unified operational dashboard, automated alerts,
          full equipment and crew visibility
  70–89:  Mixed — some systems, significant manual gaps
  50–69:  Primarily manual, reactive reporting, no unified view
  30–49:  Fragmented, high dependency on individual knowledge
  0–29:   No structured data infrastructure; decisions are intuitive

A — AGILITY (Deployment Speed & Adaptive Capacity)
  90–100: Sub-hour emergency deployment, clear escalation protocols,
          no single points of failure
  70–89:  Functional but person-dependent coordination
  50–69:  Adequate for normal ops; degrades under stress
  30–49:  Coordination bottlenecks visible; recovery is slow
  0–29:   Fragile; one absence or failure cascades across operations

R — RISK (Hidden Exposure & Operational Continuity)
  90–100: Risks documented, mitigated, monitored, and insured
  70–89:  Key risks known; mitigation partial or informal
  50–69:  Risks recognized but unmitigated; some unknown exposure
  30–49:  Significant undocumented exposure; reactive risk management
  0–29:   Risk is invisible until it becomes a crisis

E — EVOLUTION (Scalability & AI Readiness)
  90–100: Documented processes, standardized workflows, AI-ready
          data architecture, low founder dependency
  70–89:  Partially standardized; scaling possible but strained
  50–69:  Growth possible; quality and speed will suffer
  30–49:  Scaling requires founder involvement in most decisions
  0–29:   Current model cannot scale without fundamental redesign

For each dimension:
- Provide the score
- Provide 3–5 specific evidence citations from the signal map
- Identify the single highest-leverage intervention for that dimension
- Flag any scoring uncertainty and its cause

Output: Structured scoring report per dimension
```

---

### Stage 3 — Risk Synthesis (Prompt Template)

```
RISK SYNTHESIS PROMPT

INPUT: [Scored dimension outputs from Stage 2]

Task:
You are a senior risk analyst reviewing a field-service operator's
operational diagnostic. Cross the four scored dimensions and identify:

1. COMPOUNDING RISKS — where two or more dimension weaknesses
   interact to create a combined exposure greater than either alone
   Example: Low D score (no real-time data) + Low A score
   (person-dependent dispatch) = catastrophic failure risk during
   simultaneous emergency deployments

2. SINGLE POINTS OF FAILURE — people, systems, or processes
   whose absence or failure would halt operations

3. INVISIBLE RISKS — exposures not visible in normal operations
   that only materialize under stress (weather events, contract
   surges, key employee absence)

4. SCALING BREAKPOINTS — the specific operational thresholds
   where current architecture will fail as the business grows

For each risk identified:
- Assign severity: CRITICAL | HIGH | MODERATE | LOW
- Assign likelihood: HIGH | MEDIUM | LOW
- Assign time horizon: IMMINENT (0–30 days) | NEAR (31–90) |
  LATENT (90+ days)
- Identify the trigger condition that would activate the risk
- Propose the minimum viable mitigation

Output: Risk registry, sorted by severity × likelihood score
```

---

### Stage 4 — Executive Brief Generator (Prompt Template)

```
EXECUTIVE BRIEF GENERATOR PROMPT

INPUT: [Risk registry + scored dimensions + signal map]

Task:
Generate the DARE Operational Resilience Brief for Pro Power Support.

Voice: Senior advisor. Direct. Evidence-anchored. No corporate filler.
This brief will be read by an operator who built this business with
their hands. Respect that. Be precise. Earn the trust.

Required sections:
1. Executive Summary (150 words max — the essential truth)
2. DARE Dimension Scorecard (visual table format)
3. Top 5 Findings (specific, ranked, evidence-cited)
4. Risk Registry Summary (top risks from Stage 3)
5. Priority Action Stack (ranked by impact × ease)
6. AI Integration Pathway (where and how AI fits)
7. Next Phase Recommendation

Formatting constraints:
- Section headers: clear and direct (no fluff words)
- Findings: one sentence each, then evidence, then implication
- Action stack: columns for Action | Owner | Timeline | Impact
- No passive voice in recommendations
- Every claim must trace to a specific signal from the intake data

Output format: Markdown (for GitHub) or HTML (for delivery)
```

---

## 🔌 Layer 3 — MCP Integration Architecture

[![MCP](https://img.shields.io/badge/layer-3%20%7C%20MCP%20Integration-2C3050?style=flat-square)](https://docs.anthropic.com/mcp)
[![Servers](https://img.shields.io/badge/MCP%20servers-Google%20Suite%20%7C%20Drive%20%7C%20Calendar-4285F4?style=flat-square&logo=google)](https://developers.google.com)
[![Protocol](https://img.shields.io/badge/protocol-Model%20Context%20Protocol-131313?style=flat-square)](https://docs.anthropic.com/mcp)

### Architect's Thinking

MCP is how Claude reaches outside its context window into live systems. For a one-man shop, this is the infrastructure multiplier — it means I don't have to manually pull data into every session. Claude can retrieve it.

```
MCP INTEGRATION MAP — Pro Power Support Engagement

┌──────────────────────────────────────────────────────────┐
│                   CLAUDE (Orchestrator)                  │
│                    claude-sonnet-4                       │
└──────────┬───────────┬──────────────┬────────────────────┘
           │           │              │
           ▼           ▼              ▼
    ┌──────────┐ ┌──────────┐ ┌──────────────────┐
    │  Google  │ │  Gmail   │ │  Google Calendar │
    │  Drive   │ │   MCP    │ │       MCP        │
    │   MCP    │ │          │ │                  │
    └────┬─────┘ └────┬─────┘ └────────┬─────────┘
         │            │                │
         ▼            ▼                ▼
  Client docs    Customer email   Dispatch schedule
  Inspection     escalation       Crew availability
  archives       threads          Emergency window
  Proposals      Vendor comms     mapping
  RFP files      Bid responses
```

### MCP Server Configuration

```javascript
// mcp_config.json — Epoch DARE Engagement Layer

{
  "mcpServers": {
    "google-drive": {
      "url": "https://drivemcp.googleapis.com/mcp/v1",
      "purpose": "Client document retrieval — proposals, inspections, contracts",
      "query_patterns": [
        "inspection report Pro Power",
        "generator proposal 2025",
        "maintenance log",
        "government contract"
      ]
    },
    "gmail": {
      "url": "https://gmailmcp.googleapis.com/mcp/v1",
      "purpose": "Operational signal mining from email threads",
      "query_patterns": [
        "customer escalation",
        "deployment issue",
        "equipment failure",
        "emergency response",
        "bid submission"
      ]
    },
    "google-calendar": {
      "url": "https://calendarmcp.googleapis.com/mcp/v1",
      "purpose": "Dispatch and crew scheduling visibility",
      "query_patterns": [
        "field crew schedule",
        "deployment window",
        "maintenance visit",
        "inspection appointment"
      ]
    }
  }
}
```

### What I Ask MCP to Retrieve — Session Protocols

```
MCP RETRIEVAL PROTOCOL — Session Start

On engagement session initialization, Claude executes the
following retrieval sequence via connected MCP servers:

STEP 1 — Drive: Pull last 90 days of documents matching
         ["inspection", "proposal", "contract", "maintenance"]
         → Normalize → Stage 1 intake

STEP 2 — Gmail: Pull last 30 days of threads containing
         keywords: ["urgent", "failure", "delay", "escalation",
         "issue", "complaint", "emergency"]
         → Extract signal type and sender domain
         → Stage 1 intake: risk_signals array

STEP 3 — Calendar: Pull next 30 days of scheduled events
         → Map crew deployment density
         → Identify scheduling concentration risk
         → Stage 1 intake: operational_signals array

STEP 4 — Synthesize all retrieved signals into unified
         Stage 1 output before scoring begins

Architect note: The MCP retrieval is the engagement's nervous
system. Without it, I'm relying on client self-reporting only.
With it, the system can compare what leadership says with what
the documents and emails actually show. That gap is often where
the most important findings live.
```

---

## 🌐 Layer 4 — External API Integration

[![APIs](https://img.shields.io/badge/layer-4%20%7C%20External%20APIs-FF6B35?style=flat-square)](https://github.com/emcdo411/the-pro-power-discovery)
[![NOAA](https://img.shields.io/badge/API-NOAA%20Weather-00618A?style=flat-square)](https://www.weather.gov/documentation/services-web-api)
[![EIA](https://img.shields.io/badge/API-EIA%20Energy-003087?style=flat-square)](https://www.eia.gov/opendata/)
[![USASpending](https://img.shields.io/badge/API-USASpending.gov-1E5AA8?style=flat-square)](https://api.usaspending.gov)
[![SAM](https://img.shields.io/badge/API-SAM.gov-003366?style=flat-square)](https://open.gsa.gov/api/sam/)
[![FEMA](https://img.shields.io/badge/API-FEMA%20Disasters-CC0000?style=flat-square)](https://www.fema.gov/about/openfema/api)

### Architect's Thinking

This is where the system gets real intelligence — not from what the client tells me, but from what the world is actually doing around them. For a field-service operator in Texas, these signals are not optional context. They are operational triggers.

```
API INTEGRATION LAYER — Call Architecture

┌─────────────────────────────────────────────────────────────────┐
│  API CALL SEQUENCE — Contextual Intelligence Layer              │
└─────────────────────────────────────────────────────────────────┘

CALL 1 — NOAA Weather API (Risk Layer)
Endpoint: https://api.weather.gov/alerts/active?area=TX
Purpose:  Real-time storm/outage risk for Texas deployment zones
Maps to:  R dimension (Risk) — deployment trigger conditions
          A dimension (Agility) — emergency response readiness

CALL 2 — EIA API (Market Layer)
Endpoint: https://api.eia.gov/v2/natural-gas/pri/sum/data/
API Key:  [EIA_API_KEY]
Purpose:  Natural gas and LPG price indices — margin exposure
Maps to:  R dimension — contract margin risk under commodity shifts
          D dimension — proposal data accuracy vs. current market

CALL 3 — FEMA Disaster Declarations API (Demand Signal)
Endpoint: https://www.fema.gov/api/open/v1/disasterDeclarations
          ?state=TX&declarationType=DR
Purpose:  Active and recent disaster declarations in TX
Maps to:  A dimension — emergency deployment demand signals
          E dimension — scalability requirement triggers

CALL 4 — USASpending.gov (Contract Intelligence)
Endpoint: https://api.usaspending.gov/api/v2/search/spending_by_award/
Purpose:  Agency contract history in energy/generator services
Maps to:  R dimension — competitive win/loss exposure
          E dimension — government contract growth pathway

CALL 5 — SAM.gov Entity API (Compliance Layer)
Endpoint: https://api.sam.gov/entity-information/v3/entities
API Key:  [SAM_API_KEY]
Purpose:  Pro Power's SAM registration, certifications, NAICS codes
Maps to:  R dimension — government contracting compliance status
          E dimension — certification gap vs. target contracts
```

### API Response Routing to Prompt Stack

```javascript
// Pseudo-code — API orchestration layer
// Runs before Stage 1 prompt executes

async function buildContextLayer(clientId) {
  const [
    weatherAlerts,
    fuelPrices,
    femaDeclarations,
    contractHistory,
    samStatus
  ] = await Promise.all([
    fetchNOAA({ state: 'TX' }),
    fetchEIA({ commodity: 'natural-gas', region: 'TX' }),
    fetchFEMA({ state: 'TX', type: 'DR', days: 90 }),
    fetchUSASpending({ naics: ['221111', '811310'], state: 'TX' }),
    fetchSAM({ entityName: 'Pro Power Support' })
  ]);

  return {
    external_context: {
      weather_risk: weatherAlerts.features.map(a => a.properties),
      market_context: {
        natural_gas_price_index: fuelPrices.response.data,
        price_trend: calculateTrend(fuelPrices)
      },
      emergency_demand: femaDeclarations.DisasterDeclarations,
      competitive_landscape: contractHistory.results,
      compliance_status: samStatus.entityData
    }
  };
}

// This object gets injected into Stage 1 prompt
// as external_context — giving Claude real-world
// environmental signals to cross-reference against
// client-provided operational data
```

---

## 🧩 Layer 5 — Advanced Prompting Techniques Deployed

[![Prompting](https://img.shields.io/badge/technique-chain--of--thought-7B93FF?style=flat-square)](https://docs.anthropic.com/claude/docs/chain-of-thought)
[![Prompting](https://img.shields.io/badge/technique-structured%20output-FF6B35?style=flat-square)](https://docs.anthropic.com)
[![Prompting](https://img.shields.io/badge/technique-role%20priming-2C3050?style=flat-square)](https://docs.anthropic.com)
[![Prompting](https://img.shields.io/badge/technique-document%20grounding-131313?style=flat-square)](https://docs.anthropic.com)
[![Prompting](https://img.shields.io/badge/technique-multi--turn%20state-BF5700?style=flat-square)](https://docs.anthropic.com)

### Technique 1 — Role Priming with Constraint Injection

```
Why I use it:
Claude's default output is helpful but generic.
For DARE, I need a specific analytical posture —
conservative scoring, evidence citation, and no hedging.

How it works:
The system prompt establishes both a ROLE (senior analyst)
and a set of HARD CONSTRAINTS (score conservatively,
cite evidence, surface gaps). The role sets the voice.
The constraints set the ceiling on what Claude will claim.

Key line in the system prompt:
"Score conservatively. Inflate nothing. A field-service
operator making decisions on inflated scores is a safety risk."

This single constraint changes the output quality
more than any other instruction.
```

### Technique 2 — Structured Output with JSON Anchoring

```
Why I use it:
In a one-man shop, I need Claude's output to be
machine-readable AND human-readable simultaneously.
JSON anchoring means Stage 1 output becomes Stage 2 input
without manual reformatting.

How it works:
Every stage prompt ends with an explicit output schema.
Claude is instructed to produce a specific JSON structure.
The schema is validated before the next stage runs.

Critical design note:
I always include an "architect_notes" key in the schema.
This is where Claude flags things it is uncertain about
or that require my judgment before proceeding.
This prevents silent errors from cascading through
the prompt chain.
```

### Technique 3 — Document Grounding (Evidence Citation Requirement)

```
Why I use it:
Without grounding, Claude will synthesize plausible-sounding
findings that have no basis in the actual client data.
In an operational diagnostic, a hallucinated finding
delivered to an operator as fact is a professional liability.

How it works:
Every scoring prompt requires Claude to cite specific
evidence from the Stage 1 signal map for every finding.
The citation format is: [Source: doc_name | signal_type]

If Claude cannot cite evidence, it must surface the
finding as INFERRED (low confidence) rather than
OBSERVED (evidence-backed).

The INFERRED/OBSERVED distinction becomes part of the
scoring rubric — INFERRED findings score at 70% weight
of their face value.
```

### Technique 4 — Chain-of-Thought Scoring

```
Why I use it:
Operational scoring requires reasoning, not just output.
A score of 42 on Data means nothing without
the reasoning that produced it.

How it works:
Each dimension scoring prompt requires Claude to
show its work before giving a score:

Step 1: List all positive signals observed
Step 2: List all negative signals / gaps observed
Step 3: Apply rubric band
Step 4: State the score and the deciding factor
Step 5: Identify the highest-leverage intervention

This chain-of-thought output also becomes audit trail
— if the client challenges a finding, I have the reasoning.
```

### Technique 5 — Multi-Turn State Management

```
Why I use it:
A DARE engagement is not a single prompt.
It's a session that spans days, with new documents,
discovery call transcripts, and follow-up questions
added incrementally.

How it works:
I maintain a state document (dare_session_state.md)
that accumulates all normalized signals from Stage 1
across multiple sessions.

At the start of each new session, this state doc
is injected into the system prompt context.
Claude operates with cumulative intelligence
rather than starting fresh each time.

Session state structure:
{
  "engagement_id": "PPSupport-2026-001",
  "session_count": [n],
  "cumulative_signals": { ... },
  "open_architect_flags": [ ... ],
  "scoring_status": { D: [score], A: [score], R: [score], E: [score] },
  "next_session_priority": [ ... ]
}
```

---

## 📡 Layer 6 — Data Flow Diagram (Full System)

```
FULL SYSTEM DATA FLOW — DARE Intelligence Engine

CLIENT DOCUMENTS          EXTERNAL APIs            MCP SERVERS
     │                        │                        │
     │ (PDF, Excel, Word)      │ (NOAA, EIA, FEMA,      │ (Drive, Gmail,
     │                        │  SAM, USASpending)      │  Calendar)
     ▼                        ▼                        ▼
┌────────────────────────────────────────────────────────────┐
│              INTAKE AGGREGATOR                             │
│  • Document parser (Claude native)                         │
│  • API response formatter                                  │
│  • MCP retrieval normalizer                               │
│  Output: Unified raw signal object                         │
└──────────────────────────┬─────────────────────────────────┘
                           │
                           ▼
┌────────────────────────────────────────────────────────────┐
│              STAGE 1 — INTAKE NORMALIZER                   │
│  Claude Sonnet 4 | System Prime active                     │
│  Input: Unified raw signal object                          │
│  Output: Structured JSON signal map                        │
│  Flags: Data gaps, architect review items                  │
└──────────────────────────┬─────────────────────────────────┘
                           │
                           ▼
┌────────────────────────────────────────────────────────────┐
│              STAGE 2 — DIMENSION SCORER                    │
│  Four parallel scoring passes (D, A, R, E)                │
│  Input: Stage 1 JSON + Session state                       │
│  Output: Scored matrix with evidence citations             │
│  Chain-of-thought reasoning logged per dimension           │
└──────────────────────────┬─────────────────────────────────┘
                           │
                           ▼
┌────────────────────────────────────────────────────────────┐
│              STAGE 3 — RISK SYNTHESIS                      │
│  Cross-dimension compounding risk detection                │
│  Input: Scored matrix                                      │
│  Output: Risk registry (severity × likelihood × horizon)   │
└──────────────────────────┬─────────────────────────────────┘
                           │
                           ▼
┌────────────────────────────────────────────────────────────┐
│              STAGE 4 — EXECUTIVE BRIEF                     │
│  Field-service voice, evidence-anchored narrative          │
│  Input: All prior stage outputs + session state            │
│  Output: DARE Operational Resilience Brief                 │
│          (Markdown for GitHub / HTML for delivery)         │
└──────────────────────────┬─────────────────────────────────┘
                           │
                           ▼
┌────────────────────────────────────────────────────────────┐
│              /output DIRECTORY                             │
│  dare-resilience-brief.html    ← Client-facing             │
│  operational-risk-matrix.md    ← Internal / review         │
│  decision-latency-map.md       ← Visual deliverable        │
│  ai-workflow-assessment.md     ← Integration pathway       │
│  priority-action-stack.md      ← Action table              │
└────────────────────────────────────────────────────────────┘
```

---

## 🤔 Architect's Decision Log

This section is where I log live design decisions as they get made during the build. It is a working document.

```
DECISION LOG

[2026-05-07] — Chose Claude Sonnet 4 over Opus
Reason: Cost-per-token matters in a one-man shop.
Sonnet 4 is sufficient for structured analytical output
at this complexity level. Opus reserved for final
executive brief generation where nuance matters most.

[2026-05-07] — Added synthetic baseline to data sources
Reason: Pro Power may not have mature documentation.
Rather than blocking the diagnostic on sparse intake,
Claude generates an industry-calibrated baseline and
scores the client against it. Gap between actual and
baseline IS the finding.

[2026-05-07] — Chose MCP over manual document upload
Reason: Manual upload requires me to curate documents
before every session. MCP lets Claude retrieve what it
needs based on keyword queries. Less friction = more
sessions completed. Time is the scarce resource here.

[2026-05-07] — Added architect_notes key to all JSON schemas
Reason: Claude will sometimes produce findings I need
to validate before they go in the brief. The notes key
creates a review gate between stages that I control.
This prevents confident-sounding hallucinations from
reaching the deliverable.

[2026-05-07] — Structured output over narrative mid-stage
Reason: JSON between stages makes the chain portable.
If I need to change Stage 2 logic, I don't have to
rerun Stage 1. I feed the same JSON to a revised
Stage 2 prompt. This is the solo architect's version
of a modular codebase.

[NEXT] — Evaluate whether FEMA API adds real signal
         or noise for Pro Power's current footprint.
         Need first discovery call data to calibrate.
```

---

## 📋 Repository File Map

```
the-pro-power-discovery/
│
├── README.md                          ← This document (SOW + architecture whiteboard)
│
├── /architecture                      ← This layer
│   ├── WHITEBOARD.md                  ← Full system design (this doc)
│   ├── dare_session_state.md          ← Live session state accumulator
│   ├── mcp_config.json                ← MCP server configuration
│   └── api_manifest.md                ← External API endpoints + auth notes
│
├── /prompts                           ← The prompt stack
│   ├── stage_0_system_prime.md        ← System prompt (role + constraints)
│   ├── stage_1_intake_normalizer.md   ← Document processing prompt
│   ├── stage_2_dimension_scorer.md    ← Four-dimension scoring prompt
│   ├── stage_3_risk_synthesis.md      ← Cross-dimension risk prompt
│   └── stage_4_exec_brief.md          ← Executive brief generator prompt
│
├── /legal
│   ├── NDA.md                         ← Mutual NDA (Texas law)
│   ├── SOW.md                         ← Full SOW document
│   ├── MOBILIZATION_FEE.md            ← Fee schedule and payment terms
│   └── CHANGE_ORDER_TEMPLATE.md       ← Scope expansion control
│
├── /frameworks
│   ├── dare-framework.md              ← DARE dimension definitions + scoring rubrics
│   ├── decision-latency-model.md      ← Decision architecture methodology
│   └── ai-readiness-scoring.md        ← AI integration readiness rubric
│
├── /diagnostics
│   ├── dare-dimension-d.md            ← Data signal integrity audit
│   ├── dare-dimension-a.md            ← Agility and deployment speed audit
│   ├── dare-dimension-r.md            ← Risk exposure matrix
│   ├── dare-dimension-e.md            ← Evolution and scalability readiness
│   ├── friction-mapping.md            ← Friction point identification
│   └── founder-dependency-audit.md    ← Dependency concentration scoring
│
├── /intake
│   ├── raw/                           ← Client documents as received
│   ├── normalized/                    ← Stage 1 JSON output
│   └── api_responses/                 ← Cached external API responses
│
├── /output
│   ├── dare-resilience-brief.html     ← Executive findings brief (client-facing)
│   ├── operational-risk-matrix.md     ← Scored risk map
│   ├── decision-latency-map.md        ← Decision flow visualization
│   ├── ai-workflow-assessment.md      ← AI integration pathway
│   └── priority-action-stack.md       ← Ranked intervention list
│
├── /engagement
│   ├── phase-1-diagnostic.md          ← Days 1–30 session log
│   ├── phase-2-architecture.md        ← Days 31–60 session log (Option B/C)
│   ├── phase-3-validation.md          ← Days 61–90 session log (Option B/C)
│   └── discovery-notes.md             ← Executive discovery session notes
│
└── /architect-log
    ├── DECISION_LOG.md                ← Design decision audit trail
    ├── OPEN_FLAGS.md                  ← architect_notes items requiring review
    └── SESSION_NOTES.md               ← Raw session observations
```

---

## ⚡ Quick Reference — Architect's Cheat Sheet

```
┌─────────────────────────────────────────────────────────┐
│  DARE ENGINE — QUICK REFERENCE                          │
├─────────────────────────────────────────────────────────┤
│  AI Model:      Claude Sonnet 4 (claude-sonnet-4-20250514) │
│  Final Brief:   Claude Opus 4 (for nuance-critical output) │
│  MCP Servers:   Google Drive | Gmail | Google Calendar  │
│  External APIs: NOAA | EIA | FEMA | USASpending | SAM   │
│  Prompt Stages: 0 (System) → 1 → 2 → 3 → 4            │
│  Output Types:  JSON (internal) | MD + HTML (delivery)  │
│  State Doc:     dare_session_state.md                   │
│  Review Gate:   architect_notes key in every JSON schema │
│  Scoring:       0–100 per DARE dimension                │
│  Evidence Req:  OBSERVED vs. INFERRED (70% weight)      │
│  Voice:         Direct | Evidence-anchored | Field-grade │
└─────────────────────────────────────────────────────────┘
```

---

## 🛡️ IP + Governance Notice

[![IP](https://img.shields.io/badge/IP-DACR%20License%20v2.6-BF5700?style=flat-square)](https://github.com/emcdo411)
[![Owner](https://img.shields.io/badge/owner-Erwin%20Maurice%20McDonald-2C3050?style=flat-square)](https://github.com/emcdo411)
[![Entity](https://img.shields.io/badge/entity-Epoch%20Frameworks%20LLC-131313?style=flat-square)](https://github.com/emcdo411)
[![Law](https://img.shields.io/badge/governed%20by-Texas%20Law-8B0000?style=flat-square)](https://github.com/emcdo411)

The DARE framework, prompt architecture, scoring rubrics, dimension model, risk synthesis methodology, and all analytical logic in this repository are proprietary to:

**Epoch Frameworks LLC | Erwin Maurice McDonald | Fort Worth, Texas**
**© McDonald (2026) | DACR License v2.6**

This repository is an **engagement and design layer only.**
The underlying diagnostic engine, scoring mechanisms, and framework architecture are not open-sourced or disclosed beyond what is necessary to operate this engagement.

No license to underlying frameworks is granted or implied by use of this repository.

---

*DARE Operational Resilience System | Architecture Whiteboard v1.0*
*Epoch Frameworks LLC | DACR License v2.6 | McDonald (2026) | Fort Worth, TX*
*Governed by the laws of the State of Texas | Venue: Tarrant County, Texas*
*Not licensed legal, technical, or financial advice.*
