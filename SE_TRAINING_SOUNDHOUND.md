# Solutions Engineer Training Plan — SoundHound AI Edition

A training program for a solutions engineer role at **SoundHound AI**, using this
repo (DeepTutor) as the hands-on practice gym. The pairing is deliberate:
SoundHound's Amelia 7 platform is an **agentic AI system** — agents with
personas, tool/skill use, MCP support, enterprise knowledge grounding, memory,
and multi-channel deployment. DeepTutor is an open-source system built on the
same concepts (agent loop, tools, skills, MCP servers, RAG engines, three-layer
memory, Partners on IM channels). Every architectural idea you master here
transfers directly to conversations about the SoundHound stack — and you can
break things in DeepTutor without burning a customer environment.

**Structure:** 7 modules + a bonus module. Each has study targets, labs, and a
checkpoint deliverable. ~1 module/week at 6–8 hrs, or 3 weeks full-time.

---

## The SoundHound product map (learn this cold first)

| Product | What it is | Primary buyer |
|:---|:---|:---|
| **Polaris** | Foundation ASR/voice model — accents, noise, stutters; 200+ patents | Underpins everything |
| **Houndify** | Embedded voice AI API/SDK platform | Device makers, automotive |
| **SoundHound Chat AI** | Generative voice assistant for vehicles/devices | Automotive OEMs |
| **Amelia 7** | Enterprise agentic AI platform (7.3 adds MCP support) | Contact centers, banking, insurance, healthcare |
| **Smart Answering** | AI phone answering for businesses | SMB → franchise restaurants, clinics |
| **Smart Ordering / Dynamic Drive-Thru** | Voice ordering for restaurants/QSR | Restaurant chains |
| **Autonomics** | IT operations automation | Enterprise IT |
| **Vision AI** | Camera perception + voice in vehicles (CES 2026) | Automotive OEMs |
| **Sales Assist** | Retail agent (MWC 2026) | Retail |
| **Voice commerce ecosystem** | In-car/TV agents that order food, book, pay | OEMs + merchants two-sided |

Three vertical stories to internalize: **Automotive** (Chat AI, Vision AI,
voice commerce), **Restaurants/QSR** (Smart Ordering, Dynamic Drive-Thru,
Smart Answering), **Enterprise customer service** (Amelia 7, Autonomics).

---

## Module 1 — Voice AI Foundations

*You cannot sell voice AI without speaking its language.*

**Study:** ASR → NLU → dialog/agent → TTS pipeline; SoundHound's
"speech-to-meaning" differentiator vs. sequential ASR-then-NLU; key metrics —
word error rate (WER), intent accuracy, end-to-end latency, barge-in,
containment rate; edge vs. cloud vs. hybrid tradeoffs (critical for
automotive); wake words and multi-turn context.

**Labs (in DeepTutor):**
1. Configure a speech-to-text and text-to-speech model in **Settings → Models**
   and use voice in Chat. Note every point where latency or a misrecognition
   hurts the experience — that list is your demo-risk checklist for any voice
   product.
2. Write a one-pager: why "speech-to-meaning" (joint recognition +
   understanding) beats a pipeline that transcribes first and interprets
   second. Cover latency, homophones, and domain-constrained vocabulary.

**Checkpoint:** A glossary+metrics cheat sheet (2 pages) you could hand a new
teammate: every term above, what "good" looks like numerically, and which
metric each SoundHound vertical cares about most.

---

## Module 2 — Agentic AI Architecture (Amelia 7 ↔ DeepTutor)

*Amelia 7 is an agentic platform. DeepTutor is your dissectable model of one.*

**Study:** `AGENTS.md` and `README.md` here — the agent loop (think → call
tools → observe → respond), Tools vs. Capabilities, MCP servers, skills,
personas, RAG grounding, three-layer memory. Map each to its Amelia
equivalent: agents, skills/integrations, MCP (Amelia 7.3), enterprise
knowledge, user context.

**Labs:**
1. Run DeepTutor from source and trace one chat turn end-to-end: where tools
   mount, how the loop decides to stop, how `ask_user` pauses for
   clarification — the same pattern as an agent asking a caller a clarifying
   question instead of guessing.
2. Connect an MCP server in **Settings → Chat** and watch tools mount into a
   turn. Being able to explain MCP concretely matters: Amelia 7.3 ships it,
   and enterprise buyers ask what it means for their integration roadmap.
3. Build a **Partner** with a persona (`SOUL.md`), its own library, and an IM
   channel. A Partner is structurally what a deployed enterprise agent is: a
   persona + knowledge + tool policy + channel + memory. Explain the mapping
   in writing.
4. Use `consult_subagent` to have one agent delegate to another — the
   multi-agent orchestration story enterprises now expect.

**Checkpoint:** Whiteboard (from memory, recorded) "how an agentic AI platform
works" for a technical buyer — loop, tools, MCP, knowledge grounding, memory,
channels — using vendor-neutral language you could reuse at SoundHound.

---

## Module 3 — Vertical Fluency & Discovery

*SoundHound SEs sell into three very different rooms.*

**Study:** For each vertical, the buyer, the KPI, and the pain:
- **Automotive:** OEM differentiation, in-car UX, offline/edge constraints,
  voice commerce monetization, Vision AI multimodality. KPI: driver
  engagement, feature adoption, safety (eyes on road).
- **Restaurants/QSR:** labor shortage, missed calls = missed revenue, order
  accuracy, drive-thru throughput, upsell rate. KPI: order accuracy %,
  containment, average ticket lift.
- **Enterprise service (Amelia):** call deflection, containment rate, average
  handle time, CSAT, agent-assist vs. full automation, legacy IVR
  replacement. KPI: containment %, cost per contact, escalation quality.

**Labs (in DeepTutor):**
1. Build one **knowledge base** per vertical from public material (SoundHound
   product pages, case studies, earnings-call transcripts, QSR industry
   reports). These become your grounded study partners.
2. Create three buyer **personas**: a skeptical QSR franchise ops VP, an
   automotive OEM software director, a contact-center transformation lead. Run
   discovery role-plays in Chat against each — practice open questions, pain
   quantification, and handling "we'll just use ChatGPT / build it ourselves."
3. Use **Quiz** on each KB to drill the vertical facts; wrong answers land in
   your Question Bank for spaced review.

**Checkpoint:** Three one-page value briefs (one per vertical): pain →
capability → KPI impact → proof point, each ending with your five best
discovery questions for that buyer.

---

## Module 4 — Demo Craft for Voice Products

*Voice demos fail differently than software demos: mishears, latency, noise.*

**Study:** Your Module 1 demo-risk checklist; the README Explore sections for
what DeepTutor can stage.

**Labs:**
1. Script a 15-minute demo for ONE vertical (recommend QSR — most concrete):
   hook → pain in dollars (missed calls, labor) → three moments (answering a
   call, taking a complex order with modifications, upsell) → KPI close.
2. Stage it in DeepTutor: a restaurant-menu KB, a persona playing the ordering
   agent, voice input/output on. It is not Dynamic Drive-Thru, but it lets
   you rehearse the *choreography* of a live voice demo.
3. Failure drills: rehearse recovery from (a) a misrecognition mid-order,
   (b) provider latency spike, (c) dead API key. The recovery line ("this is
   exactly why the production system constrains vocabulary to the menu
   domain") is scripted, not improvised.
4. Practice the demo with real background noise — voice demos in quiet rooms
   are a lie the field will expose.

**Checkpoint:** The demo, delivered on camera twice; second take ≤15 min with a
deliberately injected failure you recover from smoothly.

---

## Module 5 — Integration & Deployment Engineering

*The SE owns the technical win: telephony, POS, CRM, edge, APIs.*

**Study:** The integration surfaces SoundHound deals touch — telephony/SIP and
IVR replacement (Smart Answering, Amelia), POS systems (Smart Ordering),
CRM/ticketing (Amelia), embedded SDKs and edge/hybrid runtimes (Houndify,
Chat AI), MCP for enterprise tool access. Then `CONTAINERIZATION.md` here in
full — single container, compose with hardened runner sidecar, reverse proxy,
rootless Podman — as your generic deployment craft.

**Labs (in DeepTutor as proxy):**
1. Drive DeepTutor headlessly: `deeptutor run ... --format json`, parse the
   NDJSON stream with `jq`, chain turns via `session_id`. This is the muscle
   for "integrate the agent into our platform" conversations.
2. Wire a Partner to Telegram: webhook auth, credential config, message
   routing — the same shape as connecting an agent to a telephony or contact-
   center channel.
3. Deploy DeepTutor behind a reverse proxy with TLS on one port, WebSockets
   verified — the standard enterprise POC topology.
4. Connect a local model via `host.docker.internal` from inside Docker, and
   write up why an automotive/on-prem buyer cares about where the model runs
   (latency, connectivity loss, data residency).

**Checkpoint:** An integration architecture one-pager for a fictional deal:
"120-location restaurant chain wants AI phone ordering" — call flow from PSTN
to agent to POS, failure/fallback paths (human handoff), and a POC-environment
runbook.

---

## Module 6 — POC Design & Troubleshooting Fire Drills

*Voice AI deals are won in the POC, on metrics agreed in advance.*

**Study:** POC design discipline: define success metrics *before* the pilot
(order accuracy ≥ X%, containment ≥ Y%, p95 latency ≤ Z ms), baseline
measurement, pilot scope (locations, hours, menus), exit criteria.
Troubleshooting: `CONTAINERIZATION.md` troubleshooting section; release notes
in `assets/releases/` as a catalog of real failure modes.

**Labs:**
1. Draft a POC plan for the Module 5 restaurant chain: 3 pilot stores, 4
   weeks, metrics, baseline, weekly checkpoints, rollback criteria, and the
   go/no-go scorecard the buyer signs up front.
2. Fire drills in DeepTutor — break it, then diagnose from symptoms only:
   wrong embedding base URL (retrieval silently degrades — the "agent gives
   wrong answers" ticket), dead API key mid-session, port conflict, stale
   `web/.next` locks. Write the diagnosis path for each.
3. Build the voice-specific triage tree: "the agent mishears / answers wrong /
   is slow / hangs up" → is it ASR, NLU/grounding, tool integration, latency,
   or telephony? What evidence distinguishes them (transcripts, logs, timing
   traces)?

**Checkpoint:** Two artifacts — the signed-metrics POC plan template, and the
voice-agent triage decision tree (one page each).

---

## Module 7 — Security, Compliance & the Enterprise Conversation

*SoundHound's buyers are OEMs, banks, insurers, healthcare — the security
review is the real gate.*

**Study:** The compliance map per vertical: **PCI DSS** (payment in voice
ordering/commerce — how does card data stay out of transcripts and recordings?),
**HIPAA** (patient calls to clinics via Smart Answering/Amelia), **GDPR/CCPA +
biometric-voice laws** (voiceprints, recording consent, data residency),
automotive data governance (in-cabin audio). Then DeepTutor's security surface
as a study model: multi-user auth + grants, per-user workspace isolation, the
code-execution sandbox and hardened runner sidecar, the skill-import gate.

**Labs:**
1. In DeepTutor, enable auth, create an admin and a scoped user, assign
   grants; verify the non-admin never sees raw API keys. Map this to the
   enterprise asks: tenant isolation, credential custody, least privilege.
2. Answer a mock security questionnaire for a voice AI deployment honestly:
   where audio/transcripts are stored and for how long, whether customer data
   trains models, redaction of payment/PHI data in transcripts, encryption in
   transit/at rest, human-review access, incident response. Flag every "it
   depends on configuration" — bluffing on a questionnaire ends deals and
   careers.
3. Write the "does our data train your model?" answer three ways: for a CISO,
   for a procurement form, and for a 30-second exec hallway exchange.

**Checkpoint:** A security & compliance overview (2 pages) for a voice AI
deployment at a healthcare clinic chain: data flows, trust boundaries,
HIPAA-relevant controls, retention, and the hardened deployment you'd
recommend.

---

## Bonus Module — DeepTutor as Your Standing Coach

Keep the gym running through the interview process and into the job:

1. **Mastery Path** over Modules 1–7 with the mastery gate on, so progress is
   graded, not vibes.
2. Weekly **role-play** against the three buyer personas; rotate in new
   objections (price, "Big Tech will crush them", build-vs-buy, model
   commoditization).
3. **Question Bank** review twice weekly — the misses are the syllabus.
4. A **Co-Writer** playbook doc: objections fumbled, answers found, demo lines
   that landed, plus a running log of SoundHound news (earnings, CES/MWC
   launches like Vision AI and Sales Assist) so your story stays current.

---

## Competency map

| SoundHound SE competency | Built in |
|:---|:---|
| Voice AI technical fluency (ASR/NLU/TTS, metrics) | Module 1 |
| Agentic platform architecture, MCP, multi-agent | Module 2 |
| Vertical business cases + discovery | Module 3 |
| Live voice demo craft + failure recovery | Module 4 |
| Telephony/POS/CRM integration thinking, deployment | Module 5 |
| POC design with signed metrics; voice triage | Module 6 |
| Compliance (PCI/HIPAA/GDPR), security reviews | Module 7 |
| Objection handling + staying current | Bonus |

The checkpoint artifacts — vertical briefs, the recorded demo, the integration
one-pager, the POC template, the triage tree, the security overview — double as
an interview portfolio for the SoundHound SE role itself.
