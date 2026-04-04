# Lab Data — PT Nusantara Digital (Synthetic)

All files in this directory are **fully synthetic**. They are safe to use with
any AI platform, cloud or local. No sanitization required.

The data is modeled after a fictional Indonesian fintech company called
**PT Nusantara Digital** — designed to feel realistic so your learning
translates to real-world SOC work.

---

## ⚠️ Important

If you add your own files to this directory from a real environment,
treat them as sensitive and follow the sanitization guidance in
`docs/sanitization-guide.md` before using them with any AI model.

---

## Directory Structure

```
lab/
├── alerts/       SIEM alert exports — start here for triage exercises
├── logs/         Raw logs — Windows Event, Linux auth, CloudTrail
├── ioc/          IOC lists in CSV format for enrichment exercises
├── reports/      CTI reports for analysis exercises (coming in Phase 2)
└── scenarios/    Structured threat campaigns combining all of the above
```

---

## Alert Files

| File | Scenario | Threat Type | Difficulty |
|------|----------|-------------|------------|
| `alerts/scenario-01-alert-bruteforce.json` | Scenario 01 | RDP Brute Force → Post-login Enumeration | Beginner |
| `alerts/scenario-02-alert-phishing.json` | Scenario 02 | Phishing → BEC → AWS Enumeration | Beginner |

## Log Files

| File | Source | Contents |
|------|--------|----------|
| `logs/windows-event-scenario-01.json` | Windows Security Log | Event IDs 4624, 4625, 4663, 4688 for scenario 01 |
| `logs/linux-auth-srv-app-01.log` | Linux syslog | SSH brute force + normal auth activity |
| `logs/cloudtrail-iam-scenario-02.json` | AWS CloudTrail | IAM enumeration + privilege escalation attempt |

## IOC Files

| File | Scenario | Contents |
|------|----------|----------|
| `ioc/scenario-01-02-ioc-list.csv` | Scenario 01 + 02 | IPs, domains, hashes, URLs, emails with verdicts |

---

## Scenario Connections

Scenarios 01 and 02 share the same threat actor infrastructure (`185.220.101.47`).
This is intentional — it teaches analysts to pivot across incidents and recognize
campaign-level patterns, not just individual alerts.

```
Scenario 01                    Scenario 02
RDP Brute Force          Phishing BEC + AWS Enumeration
     |                              |
     └──── 185.220.101.47 ──────────┘
           (Tor exit node)
           Same C2 IP
           Same threat actor
```

---

## How to Use These Files

Always start from the `alerts/` files — they contain the structured alert data
with analyst notes and pivot suggestions.

Reference the `logs/` files when you need to go deeper into the raw evidence
that generated the alert.

Use the `ioc/` files for enrichment exercises and to practice pivoting across
indicators.

In your agent session, reference files using their relative path:

```
Please read lab/alerts/scenario-01-alert-bruteforce.json and help me triage it.
```

Or use the slash command shortcut:

```
/triage lab/alerts/scenario-01-alert-bruteforce.json
```

---

*ai4soc — AI-Augmented Security Operations*
*github.com/blueteamid/ai4soc*
