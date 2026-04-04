# Contributing to ai4soc

Thank you for your interest in contributing. ai4soc is built by and for the
global security community — every contribution, large or small, makes this
course better for everyone.

---

## What We Need Most

In rough priority order:

**🤖 Model testing and compatibility reports**
This course was developed and fully tested with Claude Code. We need community
members to test modules with other models — Ollama local models, OpenRouter,
Gemini, GPT-4o, Mistral — and report what works, what breaks, and what needs
to be adjusted. This is one of the highest-impact ways to contribute.

**🔌 SIEM connectors**
MCP integration guides for Elastic, Splunk, Microsoft Sentinel, QRadar, Datadog,
and other SIEMs. We have Wazuh covered — everything else is community-driven.

**🧪 Lab scenarios**
New threat campaign simulations with realistic synthetic data: alerts, logs,
IOCs, and analyst notes. The more scenarios we have, the more practice analysts get.

**📝 Module content**
Additional workflows, use cases, and exercises for existing or new modules.
Detection engineering, threat hunting, cloud security, and container/Kubernetes
SOC workflows are high-priority gaps.

**🐛 Bug reports and clarity improvements**
Instructions that are confusing, commands that do not work, steps that are
missing — these matter as much as new content.

---

## Before You Contribute

**Read the existing content first.**
Familiarize yourself with `CLAUDE.md`, `README.md`, and at least Module 0
before contributing. Understanding the course philosophy will help you write
content that fits naturally.

**Open an issue before large contributions.**
For anything beyond a typo fix — new scenarios, new modules, new connectors —
open a GitHub issue first to discuss scope, approach, and whether it fits the
roadmap. This avoids duplicated effort and PRs that cannot be merged.

**Keep the education-only framing.**
All content must clearly be for learning purposes. Do not contribute content
that could be mistaken for production-ready tooling without clear disclaimers.

---

## Contribution Process

### Small fixes (typos, broken links, unclear steps)
1. Fork the repository
2. Make your change
3. Open a pull request with a brief description of what you fixed and why

### Model compatibility reports
1. Open a GitHub issue titled: `Model test: [model name] — [works/partial/broken]`
2. Include: model name and version, tier (Ollama/OpenRouter/other), which modules
   you tested, what worked, what did not, and any workarounds you found
3. If you have fixes, open a PR against the relevant module files

### New lab scenario
1. Open an issue first: describe the threat type, ATT&CK techniques, difficulty
   level, and what log sources you plan to include
2. Wait for maintainer feedback before building
3. Follow the existing scenario structure in `lab/alerts/` and `lab/logs/`
4. All data must be fully synthetic — no real IPs, real domains, real hashes
5. Include `analyst_notes` with pivot suggestions, IOC list, and MITRE chain
6. Open a PR with the scenario files and an update to `lab/README.md`

### New SIEM connector
1. Open an issue first using the connector request template
2. Study the Wazuh connector in `connectors/wazuh/` as a reference
3. Your connector guide should cover: prerequisites, installation, configuration,
   a basic test query, and known limitations
4. Include a note on which MCP server or integration library you used
5. Open a PR with the connector folder and an update to the SIEM integration
   table in `README.md`

### New module content
1. Open an issue aligned with the course roadmap
2. Module files follow the naming convention: `X.Y-title.md`
3. Every lesson must end with a concrete artifact the analyst produces
4. Include instructions for all three tiers (Ollama, OpenRouter, Claude Code)
   or clearly note which tiers a feature requires
5. No jargon without explanation — write for a competent analyst, not an expert

---

## Content Standards

**Language:** English only. Write clearly and directly. Avoid unnecessary
hedging and filler phrases.

**Tone:** Practitioner to practitioner. Not academic, not marketing. Assume
the reader is a working analyst who values their time.

**Data:** All lab data must be fully synthetic. Never include real IP addresses,
real domain names, real file hashes, real usernames, or any data that could
be traced back to a real organization or individual.

**Attribution:** If your content is based on public research, threat reports,
or other sources, cite them. Security community knowledge-sharing only works
when attribution is maintained.

**Disclaimers:** Any content that could be used in a production environment
must include a clear note that it requires additional tuning and review.

---

## Code of Conduct

This project follows a simple standard: be respectful, be constructive,
and focus on making the content better for learners. Security is a community
discipline — we build each other up.

Contributions that demean, exclude, or harm others will not be accepted.

---

## Questions

Open a GitHub Discussion or an issue. We read everything and respond to
all good-faith questions.

---

*ai4soc — AI-Augmented Security Operations*
*github.com/blueteamid/ai4soc*
