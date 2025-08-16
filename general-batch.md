# Designed to be used with TextBlaze but the / at the start can be replaced with : and added to the end to be :command: if using espanso
# 1) Repo kickoffs (you already use versions of these) 

**A. Ship-First Repo Primer**

```
PROJECT: {{Name}}
MODE: Ship-first, explain-later. Produce runnable code with minimal narration.

SCOPE: HTML-first MVP + progressive enhancement; feature flags for experimental parts; OPFS/IDB for local data; WebCrypto for signing; WCAG 2.2 AA; perf + privacy.
DO: scaffold files; implement core flows; harden (validation, CSP notes); test (unit+1 e2e); doc (README, NEXTGEN_FEATURES); one-command run; CI green.
ACCEPT: `npm i && npm run dev` works; `build`+`preview` work; flags toggle all prototypes; no secrets; docs accurate.
OUTPUT: file tree first, then full file contents; ≤200 words non-code.
```

**B. HTML-First MVP (landing → detail → checkout → receipt)**

```
ROLE: Lead engineer/designer. Deliver runnable code, not essays.
RUNTIME: Win11, Node 20 LTS, Vite 5 (dev only).
FILES: index.html, item.html, checkout.html, receipt.html, assets/{styles.css,app.js,crypto.js}, sw.js, README.md, NEXTGEN_FEATURES.md, package.json, tests/, .github/workflows/ci.yml
FEATURES: listings grid + filters; cart; escrow stub; signed receipt (P-256/Ed25519); verify page; OPFS store; PWA; optional WebRTC peer sync; optional semantic search (Transformers.js/WebGPU).
ACCEPT: one-command run, Playwright e2e path, flags documented.
OUTPUT: tree → full contents (all files).
```

**C. Next-Gen Feature Pack (rebalanced to avoid pure theory)**

```
PROJECT: {{Name}} — Next-Gen Feature Pack
GOAL: Toggleable future features with safe fallbacks.
INCLUDE: experimental integrations (behind flags), emotional-IQ UX, zero-harm guardrails, telemetry hooks OFF by default.
CHECKS: versions & breaking changes; polyfills; perf budget; a11y; i18n; security notes.
DOCS: write NEXTGEN_FEATURES.md with matrix {flag, default, risk, fallback, perf cost}.
DELIVER: code + tests + CI. No essays.
```

# 2) Governance / contracts / trust spine

**D. Donor Intent Router — Contract Spec**

```
ROLE: Solidity architect.
OBJECTIVE: Route funds per contributor intent with verifiable receipts + dispute hooks.
STRUCTS: Intent{beneficiary, percent, lockUntil, allowlist[], notesHash}; Receipt{txHash, intentsMerkleRoot, totals, timestamp, signer, sig}.
FLOWS: deposit→lock; release→split; dispute→pause→arbiter path; rollback conditions.
SECURITY: reentrancy guards; pull over push; Safe-compatible modules; events for every state change.
TESTS: unit (edge cases + fuzz); property tests (sum(percent)=100); fork test against Safe.
OUTPUT: interfaces + implementation + tests + README threat model.
```

**E. Receipt Schema + Verify Page (front-end)**

```
SCHEMA: {listingId, orderId, seller, buyer, subtotal, fees, total, isoTimestamp, fieldsHash, signature, pubKey}
CRYPTO: WebCrypto P-256 & Ed25519; detached signatures; canonical JSON hashing.
UI: paste JSON → verify → ✅/❌ with debug pane.
OUTPUT: `receipt.html`, `assets/crypto.js`, tests.
```

# 3) Testing, CI, quality bars

**F. Tests & CI Boilerplate**

```
SETUP: Vitest (unit), Playwright (e2e), k6 or Artillery stub (load), chaos stub.
CI: Node 20 matrix; steps=install→lint→typecheck→test→build→upload artifact.
ACCEPT: green on first run; artifact attached to release.
OUTPUT: tests/, .github/workflows/ci.yml, README test matrix.
```

**G. Threat Model & Abuse Cases**

```
METHOD: STRIDE + misuse cases.
ASSETS: user vault, receipts, intents, P2P channel, OPFS blobs.
ATTACKS: CSRF, XSS, replays, quota abuse, model poisoning, evidence tamper.
CONTROLS: CSP, COOP/COEP, SameSite, input validation, content hashing, signature checks, TURN rate limits.
OUTPUT: docs/threat-model.md with mitigations + residual risks.
```

# 4) Performance, a11y, i18n

**H. Perf Budget & Profiling**

```
BUDGET: TTI < 3s @ Fast 3G; JS < 170KB gz; images responsive.
TOOLS: Lighthouse CI, Web Vitals (inert by default), perf marks around model loads.
OUTPUT: docs/perf.md + `NEXTGEN_FEATURES.md` perf column.
```

**I. Accessibility & i18n Sweep**

```
TARGET: WCAG 2.2 AA; keyboard-first; color contrast; aria for all controls.
CHECKS: axe run; tab order; focus rings; reduced-motion path.
I18N: key map JSON; RTL toggle; date/number formatters; locale switcher.
OUTPUT: a11y report + `i18n/en.json`, `i18n/rtl.json`.
```

# 5) Data, storage, sync

**J. OPFS/IDB Vault with Export**

```
STORE: OPFS primary + IDB index; navigator.storage.estimate() quota display.
BACKUP: encrypted export (AES-GCM, Argon2id params recorded); import flow.
WARN: site-data clear wipes OPFS → show safety copy CTA.
OUTPUT: assets/store.js + UI.
```

**K. WebRTC Peer Sync (manual, optional)**

```
FEATURE: Offer/Answer text + QR; TURN config slot; DTLS status badge; signed diff bundles.
FAILSAFE: offline reconciliation and conflict strategy.
OUTPUT: assets/p2p.js + tests.
```

# 6) Research + sourcing

**L. Freshness-First Research (browsing model)**

```
TASK: Survey {{topic}}. Include only sources updated within last 18 months; diversity of domains.
OUTPUT: 5–8 bullets w/ dates; quick takeaways; cite each claim; end with “What we’re adopting & why.”
```

# 7) Docs, licensing, releases

**M. README Generator (runnable first)**

```
WRITE: problem, quickstart (≤60s), features, flags, security notes, a11y, perf budget, roadmap.
INCLUDE: badges, commands, known issues, support policy.
```

**N. Delayed Impact-Release Protocol Tagging**

```
INSTRUCTION: Mark prototypes “research-only” or “DIRP-held”; list unlock criteria; add ethics note; link to license exceptions.
OUTPUT: LICENSE appendix + docs/policy/delayed-impact.md
```

**O. Release Checklist**

```
VERIFY: version bump; changelog; CI artifact; signed tag; SBOM (optional); hash list.
PUBLISH: release notes with features, flags, risks, roll-back steps.
```

# 8) Dev environment, packaging, notarization

**P. Devcontainer & Scripts**

```
DEVCONTAINER: Node 20, git, Playwright deps, recommended extensions.
SCRIPTS: `dev`, `build`, `preview`, `lint`, `test`, `e2e`, `format`, `verify-receipt`.
OUTPUT: .devcontainer/, package.json, scripts/.
```

**Q. MasterZIP Bundler (offline-friendly)**

```
GOAL: Produce portable ZIP with build, docs, sample data; no external fetch required.
INCLUDE: /dist, README.pdf (optional), seed.json, verify script.
OUTPUT: scripts/make_masterzip.(ps1|sh)
```

**R. IPFS/Arweave Notarization (optional)**

```
TASK: Hash build + docs; write manifest.json; add IPFS CID/Arweave TX placeholders; DO NOT embed secrets.
OUTPUT: scripts/notarize.(ps1|sh), docs/notarization.md
```

# 9) Multi-agent orchestration (Magentic-One / CrewAI)

**S. Orchestration Job Template**

```
GOAL: Plan/execute {{task}} with tools: WebSurfer, FileSurfer, RepoWriter.
AGENTS: Researcher → Architect → Implementer → Tester → Publisher.
GUARDRAILS: zero-harm; no secrets; respect rate limits.
OUTPUT: runnable plan + diffs; PR or patch; tests green.
```

# 10) Content + comms

**T. Comparison Table / Matrix**

```
TASK: Compare {{tools or vendors}}.
COLUMNS: platform, license, offline?, Windows?, GPU?, API, price, strengths, caveats, best-for.
OUTPUT: markdown table + 3-line recommendation.
```

**U. “Now / Next / Later” Plan**

```
NOW (0–7d): {{…}} | NEXT (8–30d): {{…}} | LATER (30–90d): {{…}}
RISKS: {{…}} | BLOCKERS: {{…}} | METRICS: {{…}}
```

**V. Ethereum.SE Post Template (governance upgrade)**

```
TITLE: {{short, precise question}}
BODY: context, current approach, minimal example, desired properties (immutability, donor intent routing, Safe compat), what we’ve tried, precise questions.
```

---

## /Slash-command expansions (for your text-replacement script)

Use anywhere; they expand into short, directive blocks so models don’t drift.

* `/scaffold` → “Create file tree + full contents for HTML/CSS/JS + README + NEXTGEN\_FEATURES + package.json + sw\.js + tests + CI. One command run. File tree first, then contents.”
* `/nextgen` → “Add toggleable experimental features behind flags with safe fallbacks; update NEXTGEN\_FEATURES.md (flag, default, risk, fallback, perf).”
* `/harden` → “Add validation, threat model, CSP/COOP/COEP notes, anti-replay, signature checks, and document residual risks.”
* `/a11y` → “Enforce WCAG 2.2 AA, keyboard-first, aria labels, reduced-motion path; run axe and fix violations.”
* `/perf` → “Apply perf budget; lazy-load heavy modules; add Web Vitals (disabled by default); document Lighthouse scores.”
* `/tests` → “Write Vitest for utils + Playwright e2e for main flow; CI matrix Node 20; artifact upload.”
* `/docs` → “Update README quickstart; add feature matrix, flags, perf, a11y; changelog entries.”
* `/release` → “Version bump, signed tag, changelog, artifact, SBOM (opt), hash list; publish release notes with rollback.”
* `/receipts` → “Implement detached signature receipts (P-256/Ed25519), canonical JSON hash, verify page.”
* `/webrtc` → “Add manual WebRTC pairing with QR, TURN slot, DTLS status; offline diff bundles.”
* `/opfs` → “OPFS vault + IDB index; quota display; encrypted export/import; wipe warnings.”
* `/i18n` → “Add locale keys, RTL support, date/number formatting; language switcher.”
* `/intent` → “Scaffold Donor Intent Router contract + tests; events; Safe module hooks.”
* `/notarize` → “Hash build; write manifest.json; IPFS/Arweave placeholders; scripts + docs.”
* `/masterzip` → “Build portable ZIP with dist, docs, seeds, verify scripts; no external fetch required.”
* `/plan` → “Produce Now/Next/Later with risks, blockers, metrics.”

Want me to bundle these into a single **PROMPTS.md** you can drop into every repo?
