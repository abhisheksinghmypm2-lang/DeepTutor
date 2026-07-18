# Solutions Engineer Training Program — Using DeepTutor as Your Gym

DeepTutor is a real, self-hostable AI product with four install paths, pluggable
LLM/embedding providers, five RAG engines, optional multi-user auth, IM-channel
integrations, and a container story that includes rootless Podman and read-only
rootfs. That is precisely the surface area a solutions engineer works every day:
**understand it, demo it, deploy it, integrate it, debug it, and defend it to a
skeptical buyer.**

This program treats the repo as a customer product you were just hired to sell
and support. Six modules, each with hands-on labs and a checkpoint deliverable —
the kind of artifact you'd actually produce on the job (a whiteboard pitch, a
demo script, a deployment runbook, an RFP response). A bonus module at the end
turns DeepTutor itself into your study coach.

**Time budget:** ~1 module/week at 5–8 hrs/week, or compress to 2–3 weeks
full-time.

---

## Module 1 — Product & Architecture Mastery

*An SE's credibility comes from explaining the product better than the docs do.*

**Study:**
- `README.md` — every collapsed section, not just Get Started.
- `AGENTS.md` — the two-level plugin model (Tools vs. Capabilities), key files,
  dependency layers.
- The system architecture and chat-agent-loop diagrams under `assets/figs/system/`.

**Labs:**
1. Run the app from source (README Option 2). Use every major surface once:
   Chat, Quiz, Research, Visualize, Co-Writer, Book, Knowledge Center, Memory.
2. Trace one chat turn through the code: find where a capability is registered,
   where tools mount, and where the agent loop decides to stop. Start from
   `deeptutor/capabilities/` and `deeptutor/agents/`.
3. Explain the three-layer memory system (L1 traces → L2 surface summaries →
   L3 synthesis) out loud, without notes, in under 3 minutes.

**Checkpoint:** Whiteboard the architecture from memory — frontend, backend,
agent loop, tools, capabilities, RAG engines, memory, storage — then record
yourself giving a 5-minute "what is this product and why is it built this way"
pitch to a technical audience.

---

## Module 2 — Demo Craft

*A demo is a story with clicks, not a feature tour.*

**Study:** The Explore sections of `README.md`; pick the 3 surfaces with the
most obvious business value for a chosen buyer.

**Labs:**
1. Invent a customer: e.g., a university department that wants AI tutoring over
   its own course materials, or a company training team. Write their 3 pain
   points.
2. Build the demo environment: create a knowledge base from 2–3 real PDFs,
   generate a Book from it, set up a persona, seed some memory by having real
   conversations.
3. Script a 15-minute demo: hook → pain → 3 capability moments mapped to the
   3 pains → close. Every click planned; no "let me just find where that is."
4. Break-recovery drill: mid-demo, kill your LLM provider key. Practice
   recovering gracefully (switch profiles in Settings → Models without losing
   the room).

**Checkpoint:** Deliver the demo end-to-end on camera, twice. Second take must
be under 15 minutes with zero dead air.

---

## Module 3 — Deployment Engineering

*SEs own the POC environment. If it doesn't deploy, nothing else matters.*

**Study:** `CONTAINERIZATION.md` in full — overview, Docker, remote/reverse-proxy,
host LLM providers, Podman/rootless/read-only rootfs, runtime configuration,
troubleshooting, security notes. Also `compose.yaml`, `docker-compose*.yml`,
`Dockerfile`, `Dockerfile.runner`.

**Labs:**
1. Deploy all four install paths (PyPI, source, Docker single container,
   CLI-only). Note the workspace/config layout each one produces.
2. Single-container Docker with a named volume; verify settings survive
   container replacement.
3. Compose deployment with the hardened runner sidecar; confirm office-skill
   code execution routes to the sidecar instead of the app container.
4. Put it behind a reverse proxy (nginx or Caddy) with TLS on a single
   published port; verify WebSockets work through the proxy.
5. Connect a host-side local model (Ollama or LM Studio) from inside Docker
   using `host.docker.internal` — the classic "localhost isn't your localhost"
   customer trap.
6. Stretch: rootless Podman with read-only rootfs, per `CONTAINERIZATION.md`.

**Checkpoint:** Write a one-page deployment runbook for "customer wants this on
a single Linux VM behind their own TLS, using their internal OpenAI-compatible
gateway" — commands, config file diffs, and a verification checklist.

---

## Module 4 — Integrations & Configuration Depth

*Most SE work is making the product meet the customer's stack.*

**Study:** The configuration reference in `README.md` (all files under
`data/user/settings/`), the Knowledge Center section (five RAG engines), and
the Partners channel layer.

**Labs:**
1. Configure two different LLM providers as profiles and switch between them
   with the draft-and-apply flow. Add an embedding profile and confirm RAG
   works.
2. Build the same knowledge base twice — LlamaIndex and one graph engine
   (LightRAG or GraphRAG) — and write down when you'd recommend each and what
   each costs in build time.
3. Drive DeepTutor as a machine: `deeptutor run ... --format json`, parse the
   NDJSON stream with `jq`, chain two turns in one session by capturing
   `session_id`. This is the "does it have an API my platform team can use?"
   answer.
4. Wire a Partner to one IM channel (Telegram is the cheapest to set up) and
   message it.
5. Install a skill from EduHub via CLI and explain the import safety gate —
   what is checked, what is stripped, and why a customer's security team
   should care.

**Checkpoint:** A comparison brief (1–2 pages): the five RAG engines — how each
works, when to recommend it, operational cost — written for a technical buyer.

---

## Module 5 — Troubleshooting & Support Fire Drills

*Customers judge vendors by the second call, not the first.*

**Study:** The Troubleshooting section of `CONTAINERIZATION.md`; the release
notes in `assets/releases/` (each one is a catalog of real failure modes:
stuck dev-server locks, CORS on remote Docker, embedding mismatches, non-ASCII
path bugs, GBK encoding on Windows).

**Labs:**
1. Break it on purpose, then fix it from symptoms only (no peeking at what you
   changed): wrong embedding base URL; frontend port conflict; stale
   `web/.next` lock files; deleted API key mid-session.
2. Browse the upstream repo's closed issues. Pick five, and for each write the
   diagnosis you'd have given from the user's first message alone. Compare
   with the actual resolution.
3. Practice the escalation call: reproduce a bug, write the minimal repro, and
   file a mock internal ticket with environment, steps, expected vs. actual,
   and logs.

**Checkpoint:** A troubleshooting decision tree (one page) for "the app starts
but chat doesn't answer" — covering provider config, network, ports, proxy,
and container-networking causes, in the order you'd check them.

---

## Module 6 — Security, Multi-Tenancy & the Enterprise Conversation

*The deal-killer questions come from the security review, not the demo.*

**Study:** Multi-User section of `README.md`; the Code Execution Sandbox
section; Security notes in `CONTAINERIZATION.md`; the skill import gate.

**Labs:**
1. Enable auth, register an admin, create a second user, and assign grants
   (models, KBs, skills). Verify the non-admin sees scoped resources and never
   raw API keys.
2. Map the data layout (`data/user/`, `data/users/<uid>/`, `data/partners/`,
   `data/system/`) and write where secrets, chat history, and audit logs live —
   the "where does our data go?" answer.
3. Disable the subprocess sandbox (`sandbox_allow_subprocess: false`) and
   observe what breaks. Explain the trust decision and why the compose runner
   sidecar is the stronger posture.
4. Mock security questionnaire — answer honestly from the codebase: encryption
   at rest? SSO? Audit logging? Tenant isolation boundaries? What runs
   model-generated code and where? Flag what the product does *not* have; an SE
   who bluffs on a questionnaire is done.

**Checkpoint:** A one-page security overview of DeepTutor for a customer's
security team: architecture trust boundaries, data flows, isolation model,
known gaps, recommended hardened deployment.

---

## Bonus Module — Let DeepTutor Train You Back

The product is a tutoring system. Point it at yourself:

1. Build an **SE-craft knowledge base** from public material you collect on
   discovery questioning, demo technique, and objection handling — plus this
   repo's own docs.
2. Create a **persona** that plays a skeptical enterprise buyer; run discovery
   role-plays in Chat and have it push back on price, security, and "we could
   build this ourselves."
3. Use **Quiz** to generate question sets from the docs KB; wrong answers
   accumulate in your Question Bank for spaced review.
4. Set up a **Mastery Path** over the six modules above and let the gate keep
   you honest.
5. Keep a **Co-Writer** doc as your running playbook: objections you fumbled,
   answers you found, demo lines that landed.

---

## Skill Map

| SE competency | Where this program builds it |
|:---|:---|
| Technical credibility | Modules 1, 4 |
| Demo & storytelling | Module 2 |
| POC / environment ownership | Module 3 |
| Debugging under pressure | Module 5 |
| Security & enterprise sales | Module 6 |
| Discovery & objection handling | Bonus module role-plays |
| Written artifacts (runbooks, briefs, RFPs) | Every checkpoint |

Run the checkpoints for real — the folder of artifacts you finish with *is* an
SE portfolio you can bring to interviews.
