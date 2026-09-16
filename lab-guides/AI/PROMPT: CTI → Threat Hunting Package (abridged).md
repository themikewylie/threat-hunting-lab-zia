#  CTI → Threat Hunting Package (abridged)

## TASK

Act as a senior Cyber Threat Intelligence (CTI) analyst and experienced threat hunter.

Analyze the supplied CTI report and transform it into a concise, structured **Threat Hunting Package**.

Extract the Threat Actor's:

* Objectives and attack behaviors
* Tactics, techniques, and procedures (TTPs)
* MITRE ATT&CK mappings
* Tools and malware
* Indicators of compromise (IOCs)

Do not simply summarize the report. Translate the intelligence into information that helps a threat hunter understand **what behaviors and artifacts could be searched for in enterprise telemetry**.

---

# INPUT

**Primary CTI Source:**

https://www.cisa.gov/news-events/cybersecurity-advisories/aa23-320a

Treat the supplied CTI source as authoritative.

You may consult MITRE ATT&CK to validate technique IDs and names, but do not introduce additional Threat Actor behaviors unless clearly identified as supplemental intelligence.

---

# CONTEXT

The output will be used by SOC analysts and threat hunters to turn CTI into actionable hunting activity.

Assume the primary hunting telemetry is:

* Secure Web Gateway / web proxy telemetry
* Zscaler Internet Access (ZIA)

When analyzing the intelligence, prioritize information that could produce observable artifacts within web, network, DNS, URL, domain, application, user, or related proxy telemetry.

### Analytical Rules

1. **Do not fabricate information.** Only attribute behaviors, tools, malware, infrastructure, IOCs, or techniques to the Threat Actor when supported by the CTI source.

2. **Separate fact from inference.** Clearly label reasonable extrapolations as **Analyst Inference**.

3. **Preserve source fidelity.** Do not convert mitigations or generic security recommendations into claimed Threat Actor behavior.

4. **Prioritize TTPs over IOCs.** Static indicators may change quickly; behavioral tradecraft is generally more durable for threat hunting.

5. **Treat legitimate tools carefully.** Legitimate RMM, remote-access, tunneling, administrative, or security software is not inherently malicious. Explain how the Threat Actor used it.

6. **Identify visibility limitations.** If a TTP would not realistically be visible in ZIA/web proxy telemetry, explicitly state this rather than inventing an observable.

7. **Defang IOCs.** Defang malicious or suspicious domains, URLs, IP addresses, and similar indicators where appropriate.

---

# OUTPUT

Produce only the following four sections.

## 1. Executive Hunt Summary

Provide a brief, operationally focused summary containing:

* Threat Actor / campaign
* Intelligence source
* Publication or update date
* Targeted organizations or sectors
* Threat Actor objectives
* Initial access methods
* Major post-compromise behaviors
* Primary tools and malware
* Credential or identity abuse
* Exfiltration, extortion, or ransomware activity
* Key behaviors most relevant to threat hunting

Keep this section concise.

---

## 2. TTP Extraction

Extract the Threat Actor behaviors described in the intelligence and map them to MITRE ATT&CK.

Create:

| ATT&CK Tactic | Technique | ID | Threat Actor Behavior | Observable Evidence | ZIA Visibility |
| ------------- | --------- | -- | --------------------- | ------------------- | -------------- |

For **ZIA Visibility**, classify each technique as:

* **High** — directly observable or strongly represented in web proxy telemetry
* **Partial** — some related activity may be observable
* **Low/None** — requires endpoint, identity, email, cloud, or other telemetry

Do not simply reproduce the ATT&CK table from the CTI report.

Translate each technique into the **specific behavior or artifact a threat hunter could potentially observe**.

---

## 3. Tools and Malware

Extract all named software and tooling, including:

* Malware
* Ransomware
* Remote-access software
* RMM tools
* Tunneling utilities
* Credential-access tools
* Administrative tools
* Other relevant software

Create:

| Tool | Type | Legitimate / Malicious | Threat Actor Use | Potential Hunt Artifacts | ZIA Visibility |
| ---- | ---- | ---------------------- | ---------------- | ------------------------ | -------------- |

Explicitly identify legitimate or dual-use tools.

For tools potentially visible through ZIA, identify useful artifacts such as associated:

* Domains
* URLs
* Hostnames
* Applications
* Download activity
* Network destinations
* Web requests

Do not assume that use of a legitimate tool alone indicates compromise.

---

## 4. IOC Package

Extract all explicit indicators available in the intelligence, including:

* Domains
* URLs
* IP addresses
* File hashes
* File names
* Email addresses
* User agents
* Command-line artifacts
* Other relevant indicators

Create:

| IOC | Type | Context | Related Tool/TTP | ZIA Huntable? | Recommended Pivot |
| --- | ---- | ------- | ---------------- | ------------- | ----------------- |

Defang applicable indicators.

For **ZIA Huntable?**, use:

* **Yes** — directly searchable within web proxy telemetry
* **Partial** — may produce related observable activity
* **No** — requires another telemetry source

Do not invent missing IOC values.

If the intelligence describes infrastructure or naming conventions without supplying a literal IOC, place these in a separate subsection titled:

### IOC Patterns

Clearly distinguish **explicit IOCs from the CTI source** from patterns or analytical observations.

---

The final output should be concise, evidence-based, and structured so a threat hunter can quickly determine:

**What did the Threat Actor do? → What TTP does it represent? → What tools or infrastructure were involved? → What artifacts can we search for?**
