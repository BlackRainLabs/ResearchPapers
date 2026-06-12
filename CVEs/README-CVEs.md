# Black Rain Labs - CVE Research Papers

## 🛡️ Security Research & Vulnerability Analysis

<div align="center">

![Black Rain Labs](https://img.shields.io/badge/Black%20Rain%20Labs-Security%20Research-00ff88?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-blue?style=for-the-badge)

**Professional Cybersecurity Research & Vulnerability Analysis**

</div>

---

## 📚 Repository Overview

This repository contains **peer-reviewed vulnerability research papers**, technical analyses, and security documentation produced by the **Black Rain Labs Security Research Division**. Each vulnerability has its own dedicated directory containing the full research paper, detection rules, mitigation scripts, and supporting documentation.

---

## 📁 Research Directory Structure

```
CVEs/
├── README.md                          # This file
├── RoguePlanet/                       # Microsoft Defender TOCTOU Race Condition
│   ├── Research.md                    # Full technical paper
│   ├── detection/                     # Detection rules & queries
│   │   ├── sentinel-kql/             # Microsoft Sentinel queries
│   │   └── defender-endpoint/        # Defender for Endpoint rules
│   ├── mitigation/                    # Mitigation scripts
│   │   ├── asr-rules/                # Attack Surface Reduction
│   │   └── powershell/               # Automation scripts
│   ├── tools/                         # Testing utilities
│   └── docs/                          # Supporting documentation
├── MiniPlasma/                        # Windows Cloud Filter Driver LPE
│   ├── Research.md
│   ├── detection/
│   ├── mitigation/
│   ├── tools/
│   └── docs/
├── [CVE-2026-XXXX]/                   # Additional vulnerability research
│   ├── Research.md
│   ├── detection/
│   ├── mitigation/
│   ├── tools/
│   └── docs/
└── [Other Vulnerabilities]/           # More research papers
    └── ...
```

---

## 🔬 Available Research Papers

### **2026 Research Publications**

| Vulnerability | Directory | CVE | CVSS | Type | Status | Date |
|---------------|-----------|-----|------|------|--------|------|
| **RoguePlanet** | [`./RoguePlanet/`](./RoguePlanet/) | Unassigned | 7.8 (High) | LPE - TOCTOU | 🟢 Published | June 2026 |
| **MiniPlasma** | [`./MiniPlasma/`](./MiniPlasma/) | CVE-2026-33825 | 7.8 (High) | LPE - Driver | 🟢 Published | June 2026 |

---

## 📊 Featured Research

### **1. RoguePlanet - Microsoft Defender Zero-Day**

📁 **Directory**: [`./RoguePlanet/`](./RoguePlanet/)

**CVE**: Unassigned (Zero-Day)  
**CVSS Vector**: `CVSS:3.1/AV:L/AC:L/PR:L/UI:N/S:U/C:H/I:H/A:H`  
**MITRE ATT&CK**: [T1068.001](https://attack.mitre.org/techniques/T1068/001/) - Exploitation for Privilege Escalation (Local)

#### Summary

The **RoguePlanet** vulnerability exploits a **Time-of-Check to Time-of-Use (TOCTOU)** race condition in Microsoft Defender's quarantine and remediation pipeline. Despite Microsoft's June 2026 Patch Tuesday release (KB5039212), the exploit remains functional on fully patched Windows 10 and Windows 11 systems.

#### Contents

- 📄 [`Research.md`](./RoguePlanet/Research.md) - Complete technical analysis
- 🔍 [`detection/`](./RoguePlanet/detection/) - KQL queries for Microsoft Sentinel/Defender
- 🛡️ [`mitigation/`](./RoguePlanet/mitigation/) - PowerShell automation for immediate protection
- 🛠️ [`tools/`](./RoguePlanet/tools/) - Testing utilities and PoC analysis
- 📚 [`docs/`](./RoguePlanet/docs/) - Supporting documentation and diagrams

---

### **2. MiniPlasma - Windows Cloud Filter Driver LPE**

📁 **Directory**: [`./MiniPlasma/`](./MiniPlasma/)

**CVE**: CVE-2026-33825  
**CVSS Vector**: `CVSS:3.1/AV:L/AC:L/PR:L/UI:N/S:U/C:H/I:H/A:H`  
**MITRE ATT&CK**: [T1068.001](https://attack.mitre.org/techniques/T1068/001/) - Exploitation for Privilege Escalation (Local)

#### Summary

The **MiniPlasma** vulnerability is a privilege escalation flaw in the Windows Cloud Filter driver (`cldflt.sys`) that allows any local user or application to escalate to SYSTEM privileges on fully patched Windows systems. The vulnerability is a bypass of CVE-2020-17103.

#### Contents

- 📄 [`Research.md`](./MiniPlasma/Research.md) - Complete technical analysis
- 🔍 [`detection/`](./MiniPlasma/detection/) - Detection rules and queries
- 🛡️ [`mitigation/`](./MiniPlasma/mitigation/) - Mitigation scripts and configurations
- 🛠️ [`tools/`](./MiniPlasma/tools/) - Testing utilities
- 📚 [`docs/`](./MiniPlasma/docs/) - Supporting documentation

---

## 📋 Directory Structure (Per Vulnerability)

Each vulnerability directory contains:

```
[Vulnerability-Name]/
├── Research.md                    # Full technical research paper
├── README.md                      # Vulnerability-specific overview
├── detection/                     # Detection rules & queries
│   ├── sentinel-kql/             # Microsoft Sentinel KQL queries
│   ├── defender-endpoint/        # Defender for Endpoint rules
│   └── sysmon/                   # Sysmon configuration rules
├── mitigation/                    # Mitigation scripts & configs
│   ├── asr-rules/                # Attack Surface Reduction rules
│   ├── powershell/               # PowerShell automation scripts
│   └── group-policy/             # GPO templates
├── tools/                         # Testing utilities
│   ├── poc-analysis/             # PoC code analysis
│   └── testing-scripts/          # Validation scripts
└── docs/                          # Supporting documentation
    ├── diagrams/                 # Architecture diagrams
    ├── test-results/             # Lab test results
    └── references/               # Additional references
```

---

## 📈 Research Statistics

| Metric | Value |
|--------|-------|
| **Total Research Papers** | 2+ |
| **Zero-Day Discoveries** | 1 |
| **Assigned CVEs** | 1 |
| **Average CVSS Score** | 7.8 (High) |
| **Active Research** | 2 |
| **Detection Rules** | 10+ |
| **Mitigation Scripts** | 8+ |

---

## 🔍 Research Methodology

All research papers in this repository follow industry-standard documentation practices:

| Standard | Implementation |
|----------|----------------|
| **CVSS Scoring** | CVSS 3.1 vector strings with base scores |
| **MITRE ATT&CK** | Technique mapping for each vulnerability |
| **EPSS Scores** | Exploit Prediction Scoring System estimates |
| **Test Environments** | Documented lab configurations and success rates |
| **Detection Coverage** | KQL queries, behavioral indicators, signatures |
| **Mitigation Guidance** | Immediate, short-term, and long-term recommendations |
| **References** | Cited sources and related research |

---

## 📝 Citation Format

When referencing our research, please use the following format:

```
Black Rain Labs Security Research Division. "[Vulnerability Name] Analysis: 
[Technical Description]." Version [X.X], [Date]. 
https://github.com/BlackRainLabs/ResearchPapers/tree/main/CVEs/[Vulnerability-Name]
```

**Example**:
```
Black Rain Labs Security Research Division. "RoguePlanet Zero-Day Analysis: 
Microsoft Defender TOCTOU Race Condition Vulnerability." Version 1.1, 
June 12, 2026. https://github.com/BlackRainLabs/ResearchPapers/tree/main/CVEs/RoguePlanet
```

---

## 🤝 Contributing

Contributions to this repository are welcome! Please read our [Contributing Guidelines](./CONTRIBUTING.md) before submitting pull requests.

**Areas we're looking for help**:
- Detection rule validation
- Mitigation script testing
- Documentation improvements
- Additional test environment coverage
- New vulnerability research

---

## 📧 Contact

For inquiries, collaboration opportunities, or responsible disclosure:

- **Email**: security@blackrainlabs.com
- **Website**: https://blackrainlabs.com
- **Twitter**: @BlackRainLabs

---

## 📄 License

Unless otherwise specified, all code and documentation in this repository is released under the **MIT License**. See [LICENSE](./LICENSE) for details.

---

## 📋 Disclaimer

© 2026 Black Rain Labs. All rights reserved.

**NOTICE**: This repository and all contents are provided **'AS IS'** for informational, educational, and security research purposes only. No warranties expressed or implied.

**LIMITATION OF LIABILITY**: Black Rain Labs, authors, contributors, or affiliates not liable for any claim, damages, or liability arising from this repository or its use.

**USE AT YOUR OWN RISK**: Users responsible for testing before deployment.

**SECURITY RESEARCH**: Tools may perform reverse engineering, vulnerability analysis, or security testing in good faith, within fair use doctrines and DMCA Section 1201 exemptions. Research on owned/authorized systems or with permission.

**REVERSE ENGINEERING**: For interoperability, security analysis, vulnerability discovery per applicable law.

**RESPONSIBLE DISCLOSURE**: Vulnerabilities disclosed per industry standards.

**THIRD-PARTY**: References do not constitute endorsement.

---

<div align="center">

### **Black Rain Labs Security Research Division**  
*Securing the Future, One Vulnerability at a Time*

---

© 2026 Black Rain Labs | [Website](https://blackrainlabs.com) | [Contact](mailto:security@blackrainlabs.com)

</div>
