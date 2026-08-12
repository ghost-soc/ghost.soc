# ghost.soc
# Repo principal — framework, docs, whitepaper
What is Ghost.SOC?

Ghost.SOC is an open-source Detection Engineering framework built for security teams of one. The operational reality of most organizations in Latin America.

Most security frameworks are designed for Fortune 500 companies with enterprise budgets, large teams, and dedicated tooling. Ghost.SOC is designed for the analyst who is the detection team, the response team, and the reporting team simultaneousl, with almost nothing to work with.

The core thesis: Budget is not the problem. Prioritization is.

Ghost.SOC provides a structured, replicable methodology to build meaningful detection and response capability using open-source tooling, risk-based detection engineering, and AI-assisted triage, fully operable on a standard laptop at $0 Licenses cost.


The Five Pillars
+----+----------------------------------+----------------------------------------------------------+
| #  |             PILLAR               |                     WHAT IT SOLVES                      |
+----+----------------------------------+----------------------------------------------------------+
| 01 | Telemetry-First Mindset          | What to instrument first when you can't monitor         |
|    |                                  | everything. Priority: identity > endpoint > cloud > net |
+----+----------------------------------+----------------------------------------------------------+
| 02 | Risk-Based Detection Engineering | How to detect what kills organizations — not everything.|
|    |                                  | Uses MITRE ATT&CK as a compass, not a checklist.        |
+----+----------------------------------+----------------------------------------------------------+
| 03 | Sigma Rules + Minimal Pipelines  | Platform-agnostic detection that scales without         |
|    |                                  | vendor lock-in. One rule, any SIEM backend.             |
+----+----------------------------------+----------------------------------------------------------+
| 04 | Minimalistic Automation / SOAR   | Automating highest-friction tasks without enterprise    |
|    |                                  | tooling. n8n + Python + Purple Team feedback loop.      |
+----+----------------------------------+----------------------------------------------------------+
| 05 | AI-Assisted Triage               | Multiplying analyst capacity without replacing analyst  |
|    |                                  | judgment. Local models. Zero enterprise license.        |
+----+----------------------------------+----------------------------------------------------------+


MVP1 Architecture
The full Ghost.SOC stack runs on a MacBook Air 2020 (8GB RAM, $0 cost):
+------------------+----------------------------------------+-------------------------+
|      LAYER       |                 TOOL                   |        FUNCTION         |
+------------------+----------------------------------------+-------------------------+
| Log Collection   | Wazuh Agent + Graylog (container)      | Lightweight SIEM        |
| Detection        | Sigma CLI                              | Detection rule engine   |
| Threat Intel     | MISP (static feeds)                    | IOC context             |
| Automation       | n8n                                    | Orchestration           |
| Case Management  | IRIS                                   | Incident tracking       |
| AI Triage        | Ollama + Phi-4 Mini                    | Local AI triage         |
| ATT&CK Mapping   | ATT&CK Navigator                       | Strategic prioritization|
+------------------+----------------------------------------+-------------------------+
| Total RAM        | ~5.5 GB active                         | $0 / month              |
+------------------+----------------------------------------+-------------------------+

Full installation guide @https://github.com/ghost-soc/stack-mvp


Framework Evolution
Ghost.SOC scales organically across three maturity levels, same principles, different infrastructure:
+------------------+------------------------+------------------------+------------------------+
|      LAYER       |       01 — MVP         |   02 — INTERMEDIATE    |   03 — OPTIMIZED       |
+------------------+------------------------+------------------------+------------------------+
| Log Collection   | Wazuh Agent + Graylog  | Wazuh All-in-One       | Wazuh Cluster          |
| Detection        | Sigma CLI              | Sigma + backend SIEM   | Sigma + CI/CD pipelines|
| Threat Intel     | MISP static feeds      | MISP self-hosted       | OpenCTI + MISP         |
| Automation       | n8n basic              | n8n + webhooks         | n8n + Shuffle + SOAR   |
| Case Management  | IRIS local             | IRIS + TheHive         | TheHive + Cortex       |
| AI Triage        | Ollama + Phi-4 Mini    | Ollama + Llama 3.1 8B  | Ollama + hybrid API    |
| ATT&CK Mapping   | Navigator (browser)    | Navigator + exports    | Navigator + Purple Team|
+------------------+------------------------+------------------------+------------------------+
| Infrastructure   | MacBook Air · 8GB RAM  | VPS · 4 vCPU · 8GB RAM | VPS · 8 vCPU · 16GB RAM|
| Monthly Cost     | $0                     | ~$20                   | ~$50-100               |
+------------------+------------------------+------------------------+------------------------+


Detection Rules
Ghost.SOC includes a curated and corrected set of Sigma detection rules for four LATAM-prioritized industries. We will add more industries over time. Be patient, or join the team and help us to build the future!
+--------+------------------------------------------+--------------------+-----------+
|   ID   |                  RULE                    |     ATT&CK         |   LEVEL   |
+--------+------------------------------------------+--------------------+-----------+
|                        MANUFACTURING                                               |
+--------+------------------------------------------+--------------------+-----------+
| MFG-01 | Ransomware Mass File Rename              | T1486              | High      |
| MFG-02 | VPN/RDP Authentication Burst             | T1110, T1110.003   | Medium    |
| MFG-03 | Remote Access Tool Execution             | T1219              | Medium    |
| MFG-04 | Large Archive Creation                   | T1560, T1560.001   | Medium    |
| MFG-05 | Internal Network Discovery               | T1016, T1018, T1082| Medium    |
+--------+------------------------------------------+--------------------+-----------+
|                           RETAIL                                                   |
+--------+------------------------------------------+--------------------+-----------+
| RET-01 | Credential Stuffing on Login             | T1110, T1110.004   | High      |
| RET-02 | Checkout Script Modification (FIM)       | T1195.002,T1059.007| High      |
| RET-03 | API Scraping / Enumeration               | T1119, T1595       | Medium    |
| RET-04 | BEC Mail Rule Creation                   | T1114, T1114.003   | High      |
| RET-05 | Ransomware Tooling Backoffice            | T1490,T1562,T1070  | High      |
+--------+------------------------------------------+--------------------+-----------+
|                           FINTECH                                                  |
+--------+------------------------------------------+--------------------+-----------+
| FIN-01 | Suspicious Login Risk State (Azure AD)   | T1078, T1078.004   | High      |
| FIN-02 | MFA Fatigue Pattern (Azure AD)           | T1621, T1111       | High      |
| FIN-03 | Financial API Abuse                      | T1078.004, T1059   | High      |
| FIN-04 | Automated Tool User Agent Login          | T1078, T1110.004   | Medium    |
| FIN-05 | Edge System Exploitation Attempt         | T1190, T1059       | High      |
+--------+------------------------------------------+--------------------+-----------+
|                         TECHNOLOGY                                                 |
+--------+------------------------------------------+--------------------+-----------+
| TEC-01 | GitHub Actions Workflow Modified         | T1195.002, T1059   | High      |
| TEC-02 | Cloud Secret in Process Arguments        | T1552, T1552.004   | High      |
| TEC-03 | Suspicious Package Registry Override     | T1195, T1195.001   | Medium    |
| TEC-04 | Crypto Miner Execution                   | T1496, T1059.004   | High      |
| TEC-05 | Public Cloud Storage Exposure            | T1530, T1537       | High      |
+--------+------------------------------------------+--------------------+-----------+

This is just the beginning!!!!
All rules are validated, corrected, and documented with error analysis.

Full rule set @https://github.com/ghost-soc/detection-rules
