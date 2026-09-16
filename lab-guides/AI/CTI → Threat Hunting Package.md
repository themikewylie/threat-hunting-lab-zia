# TICO Prompt: CTI → Threat Hunting Package

## TASK

Act as a senior Cyber Threat Intelligence (CTI) analyst and experienced threat hunter.

Analyze the supplied cyber threat intelligence and transform the intelligence into an **actionable threat hunting package** that a SOC or threat hunting team can use to proactively search its environment for evidence of the Threat Actor's behaviors.

Do not simply summarize the intelligence report.

Your primary objective is to answer:

> **Based on the behaviors described in this intelligence, what should a threat hunter actually search for?**

Extract the Threat Actor's observable behaviors, tactics, techniques, procedures (TTPs), tools, infrastructure, indicators of compromise (IOCs), and attack patterns. Translate those findings into testable hunt hypotheses and practical hunt procedures.

Prioritize **behavior-based hunting** over simple IOC matching because infrastructure, domains, IP addresses, hashes, and tooling may change.

---

# INPUT

Primary CTI source:

https://www.cisa.gov/news-events/cybersecurity-advisories/aa23-320a

Threat Actor / Campaign:

Treat the supplied CTI source as the authoritative source for this exercise.

If the source has been updated since its original publication, identify the version/date being analyzed and distinguish:

* Original observations
* Later updates
* Current observations

Do not silently combine observations from different periods.

You may consult MITRE ATT&CK to validate ATT&CK technique IDs and technique names, but do not introduce additional Threat Actor behaviors unless clearly labeled as supplemental intelligence.

---

# CONTEXT

The finished product will be used by experienced SOC analysts and threat hunters to conduct proactive threat hunting across enterprise telemetry.

Assume the hunting environment may contain:

* Secure Web Gateway / web proxy telemetry (Zscaler Internet Acces - ZIA)

The hunt package must bridge the gap between **intelligence reporting and actual hunting**.

For every relevant CTI observation, reason through this chain:

**CTI Observation → Threat Actor Behavior → ATT&CK Technique → Observable Evidence → Required Telemetry → Hunt Hypothesis → Hunt Logic → Analyst Investigation**

### Analytical Rules

1. **Do not fabricate information.**
   Only attribute behaviors, infrastructure, malware, tools, IOCs, or techniques to the Threat Actor when supported by the source.

2. **Separate facts from analytical inference.**
   Clearly label reasonable hunting extrapolations as:
   **Analyst Inference**

3. **Preserve source fidelity.**
   Do not convert mitigations or generic security recommendations into claimed Threat Actor behavior.

4. **Prioritize TTPs over IOCs.**
   IOCs are useful for retrospective searches, but behavioral hunts should receive greater emphasis.

5. **Treat legitimate tools carefully.**
   The presence of legitimate remote-management, tunneling, administrative, or security software alone is not evidence of compromise. Hunt for contextual anomalies surrounding its execution or use.

6. **Avoid overly broad hunts.**
   A hunt such as "search for PowerShell" is insufficient. Add behavioral context, relationships, timing, parent/child processes, network activity, authentication activity, user context, or other factors that improve fidelity.

7. **Do not assume telemetry exists.**
   State which data source is required for each hunt.

8. **Identify visibility gaps.**
   If a TTP cannot reasonably be observed with common enterprise telemetry, explicitly state that limitation.

9. **Defang IOCs.**
   Present malicious or suspicious domains, URLs, IP addresses, and similar indicators in a safe, defanged format where appropriate.

10. **Assign confidence.**
    Distinguish between:

    * High — directly supported by the intelligence
    * Medium — reasonable behavioral extrapolation
    * Low — speculative; generally exclude these from the final hunt package

Do not create hunts based primarily on Low-confidence assumptions.

---

# OUTPUT

Produce the following **Threat Hunting Package**.

## 1. Executive Hunt Summary

Provide a concise summary containing:

* Threat Actor
* Intelligence source
* Source publication/update date
* Targeted organizations/sectors
* Threat Actor objectives
* Initial access methods
* Major post-compromise behaviors
* Primary tools/malware observed
* Major identity or credential abuse
* Exfiltration/extortion/ransomware behavior
* Most important hunting opportunities

Keep this section brief, concise, and operationally focused.

---

## 2. Attack Flow

Reconstruct the intrusion sequence described by the intelligence.

Use the format:

**Reconnaissance → Initial Access → Credential/Identity Abuse → Execution → Persistence → Discovery → Lateral Movement → Command & Control → Collection → Exfiltration → Impact**

Only include stages supported by the intelligence.

For each stage identify:

* Threat Actor action
* ATT&CK technique
* Observable artifact
* Relevant telemetry

If the intelligence describes multiple intrusion paths, show them separately.

---

## 3. TTP Extraction

Create a table:

| ATT&CK Tactic | Technique | ID | Threat Actor Behavior | Observable Evidence | Data Source | Confidence |
| ------------- | --------- | -- | --------------------- | ------------------- | ----------- | ---------- |

Do not merely copy the ATT&CK table from the CTI report.

Translate each technique into what a hunter could realistically observe.

---

## 4. Tools and Malware

Extract all named:

* Malware
* Ransomware
* Remote-access software
* RMM tools
* Tunneling utilities
* Credential-access tools
* Administrative tools
* Other relevant software

Create:

| Tool | Type | Legitimate/Malicious | Threat Actor Use | Potential Hunt Artifacts |
| ---- | ---- | -------------------- | ---------------- | ------------------------ |

Explicitly identify dual-use/legitimate software so analysts do not treat its presence alone as malicious.

---

## 5. IOC Package

Extract all available:

* Domains
* URLs
* IP addresses
* File hashes
* File names
* Email addresses
* User agents
* Registry artifacts
* Command-line artifacts
* Other indicators

Create:

| IOC | Type | Context | Confidence | Recommended Pivot |
| --- | ---- | ------- | ---------- | ----------------- |

Defang applicable indicators.

Do not invent missing IOC values.

If the report provides patterns rather than literal indicators, identify them separately as **IOC Patterns**.

---

# 6. Hunt Hypotheses

Generate the highest-value behavioral hunt hypotheses from the intelligence.

Use this structure for every hunt:

### HUNT-01 — [Descriptive Hunt Name]

**Intelligence Basis**
Explain exactly what behavior in the CTI prompted this hunt.

**ATT&CK Mapping**
Technique ID and name.

**Hunt Hypothesis**

> If Scattered Spider or an actor using similar tradecraft has compromised the environment, then we may observe [specific behavior] within [specific telemetry].

**Required Telemetry**

List the minimum data sources required.

**Observable Behaviors**

Identify specific artifacts or relationships to investigate.

**Hunt Logic**

Describe the analytical logic independently of any SIEM/query language.

Example structure:

1. Identify...
2. Correlate...
3. Compare...
4. Exclude...
5. Investigate...

**Potential Query**

Provide pseudocode or generic query logic when enough information exists to construct one responsibly.

Do not fabricate field names.

**Expected Benign Activity**

Explain legitimate activity that could trigger the hunt.

**False-Positive Reduction**

Explain how the analyst can distinguish normal activity from potentially malicious behavior.

**Analyst Pivot**

If suspicious activity is identified, explain what the hunter should investigate next.

**Confidence**

High / Medium

##

---

## 7. Network / Web Proxy Hunts

Translate relevant behaviors into hunts suitable for:

* Secure Web Gateway telemetry
* Web proxy logs
* Zscaler Internet Access (ZIA) logs

Look specifically for observable behaviors involving:

* Threat Actor infrastructure
* Newly observed domains
* Lookalike domains
* Remote-access software
* RMM infrastructure
* Tunneling services
* Unusual outbound destinations
* Exfiltration patterns

For each hunt, explain which fields or telemetry characteristics are necessary.

##

---

## 8. Hunt Chaining

Identify opportunities to combine multiple weak signals into higher-confidence hunt logic.

Create a table:

| Signal 1 | Signal 2 | Signal 3 | Why the Combination Matters |
| -------- | -------- | -------- | --------------------------- |

Favor sequences that reconstruct portions of the Threat Actor's attack path.

For example:

**Unusual identity event + new remote-access software + outbound tunneling connection**

is generally more useful than alerting independently on each event.

---

## 9. Hunt Prioritization

Prioritize the hunts using:

**P1 — Hunt immediately**
Strong intelligence support + high impact + good enterprise visibility.

**P2 — High-value hunt**
Strong behavioral relevance but potentially more noise or telemetry dependencies.

**P3 — Supplemental hunt**
Useful supporting or retrospective hunt.

Create:

| Priority | Hunt ID | Hunt | Intelligence Confidence | Expected Visibility | Expected Noise |
| -------- | ------- | ---- | ----------------------- | ------------------- | -------------- |

Do not prioritize solely because an IOC exists.

---

## 10. Investigation Playbook

If a hunt produces a suspicious result, provide analysts with a concise pivot workflow.

Include relevant pivots across:

**User → Identity → Endpoint → Process → Domain → IP → Authentication → SaaS → Remote Access → Other Affected Users/Hosts**

The objective is to help the analyst determine whether the activity represents an isolated anomaly or part of a broader intrusion.

---

## 11. Intelligence Gaps

Identify information that would improve the hunt but is absent from the CTI.

Examples:

* Missing command lines
* Missing process ancestry
* Missing infrastructure
* Missing hashes
* Missing timing relationships
* Missing authentication details
* Missing exfiltration characteristics

Do **not** fill these gaps with invented information.

---

## 12. Hunt Package Summary

Finish with:

### Top 5 Hunts to Run First

Identify five high-value hunts based on:

* Strength of CTI evidence
* Behavioral durability
* Enterprise visibility
* Ability to detect activity beyond static IOCs

For each, provide a one-sentence explanation of why it belongs in the initial hunt campaign.

Then summarize the core hunting strategy in 3–5 sentences.

The final package should enable a threat hunter to move directly from **CTI → hypothesis → telemetry → hunt → investigation** without needing to reread the entire intelligence report.
