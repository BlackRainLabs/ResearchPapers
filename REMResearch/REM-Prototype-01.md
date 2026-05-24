---
title: REM-Prototype-01 Design Plan
subtitle: Python-Based Dialectical Memory Consolidation Engine for Hermes + Obsidian
author: Research Agent Claire
date: May 23, 2026
status: Prototype Implementation Phase
classification: Internal BlackRainLabs Research
---

# REM-Prototype-01: Python REM Engine Design for Hermes + Obsidian

## Table of Contents

- [Overview](#overview)
- [Python REM Engine Architecture](#python-rem-engine-architecture)
- [Core Components (Python Modules)](#core-components-python-modules)
- [Integration with Hermes Infrastructure](#integration-with-hermes-infrastructure)
- [REM Cycle Execution Flow](#rem-cycle-execution-flow)
- [Data Models and Output Format](#data-models-and-output-format)
- [Implementation Roadmap](#implementation-roadmap)
- [Risks and Mitigations](#risks-and-mitigations)
- [Success Metrics](#success-metrics)

---

## Overview

This document defines the concrete Python implementation plan for the REM (Dialectical Memory Consolidation) prototype. The engine will be built as a standalone Python package that integrates with Hermes' existing memory system and writes synthesized insights into an Obsidian vault.

The prototype will be invoked via the `cronjob` tool on a scheduled basis, using local file access to read Hermes memory and Obsidian notes, perform dialectical synthesis, and produce new developmental triggers.

---

## Python REM Engine Architecture

The REM engine will be implemented as a Python package with the following high-level structure:

```
rem_engine/
├── __init__.py
├── config.py                 # Paths, thresholds, prompt templates
├── memory_scanner.py         # Reads Hermes memory + Obsidian vault
├── anomaly_detector.py       # Detects contradictory memory pairs
├── cross_examiner.py         # Applies dialectical rules via LLM
├── synthesis_engine.py       # Generates new insights and triggers
├── obsidian_writer.py        # Creates formatted Markdown notes
├── memory_injector.py        # Interfaces with Hermes memory tool
├── rem_runner.py             # Main entry point for cron execution
└── utils/
    ├── embeddings.py         # Local similarity / embedding helpers
    └── prompt_templates.py   # Structured LLM prompts
```

The engine will run entirely locally on the Termux/Android environment where Hermes operates.

---

## Core Components (Python Modules)

| Module                    | Responsibility                                                                 | Key Classes / Functions                          | Dependencies                  |
|---------------------------|--------------------------------------------------------------------------------|--------------------------------------------------|-------------------------------|
| `memory_scanner.py`       | Load Hermes persistent memory and designated Obsidian folders                  | `scan_hermes_memory()`, `scan_obsidian_vault()`  | File I/O, YAML parser         |
| `anomaly_detector.py`     | Identify semantically close but contradictory statements                       | `AnomalyDetector.find_contradictions()`          | Local embeddings or heuristics|
| `cross_examiner.py`       | Apply the three synthesis rules and surface hidden variables                   | `CrossExaminer.examine_pair()`                   | LLM client (structured output)|
| `synthesis_engine.py`     | Generate developmental insight + trigger condition                             | `SynthesisEngine.create_insight()`               | Prompt templates              |
| `obsidian_writer.py`      | Write dated, tagged Markdown notes with proper frontmatter                     | `ObsidianWriter.write_insight_note()`            | Markdown templating           |
| `memory_injector.py`      | Add high-confidence triggers back into Hermes persistent memory                | `inject_trigger_to_memory()`                     | Hermes `memory` tool wrapper  |
| `rem_runner.py`           | Orchestrate full pipeline; entry point for cron jobs                           | `run_rem_cycle()`                                | All above modules             |

---

## Integration with Hermes Infrastructure

The Python REM engine will integrate with Hermes through the following mechanisms:

- **Invocation**: Scheduled via the existing `cronjob` tool (e.g., daily at 03:00 or on idle).
- **Memory Access**: Reads the Hermes memory file directly from the filesystem (injected MEMORY block source).
- **Insight Injection**: Uses the `memory` tool (via subprocess or API wrapper) to add new durable facts when confidence is high.
- **Obsidian Output**: Writes to a user-specified vault path (e.g., `~/Obsidian/BlackRainLabs/REM/ShadowContext/`).
- **Logging**: All runs logged to `~/.hermes/rem_engine/logs/` for debugging.

No external services or paid APIs will be required for the initial prototype.

---

## REM Cycle Execution Flow

When `rem_runner.py` is executed:

1. Load configuration and recent memory window (last N days).
2. Scan Hermes memory + Obsidian notes for candidate pairs.
3. Run anomaly detection to surface contradictions above similarity threshold.
4. For each pair, execute the Cross-Examiner with the three synthesis rules enforced.
5. Generate structured insight (title, hidden variable, new rule, trigger condition, confidence).
6. Write formatted Markdown note to Obsidian vault.
7. If confidence ≥ threshold, inject trigger into Hermes persistent memory.
8. Archive processed pairs to avoid re-processing.
9. Output summary log for the cron job.

---

## Data Models and Output Format

Each synthesized insight will follow this structure (both in Python dataclass and Obsidian note):

```yaml
---
title: "Bullet Points vs Narrative Based on System Risk"
date: 2026-05-23
tags: [rem-synthesis, shadow-context, trigger-if-system-anomaly]
confidence: 0.82
hidden_variable: "System risk level"
---
```

**Developmental Insight**  
Bullet points are appropriate during nominal operations. Narrative depth is required when the system enters an anomalous or high-risk state.

**Trigger Condition**  
Activate when recent tool output or user message contains error keywords or elevated urgency signals.

**Source Memories**  
- Memory A: User prefers short bullet points for project updates.  
- Memory B: User demanded detailed narrative during server crash incident.

---

## Implementation Roadmap

**Phase 0: Foundation** (Complete)
- Research review and initial design document created.

**Phase 1: Core Python Scaffolding** (Next)
- Create `rem_engine/` package structure.
- Implement `memory_scanner.py` and `rem_runner.py` entry point.
- Add configuration system for vault paths and thresholds.
- Wire basic cronjob invocation.

**Phase 2: Anomaly Detection & Synthesis**
- Implement `anomaly_detector.py` using local embeddings or keyword heuristics.
- Build `cross_examiner.py` and `synthesis_engine.py` with strict three-rule prompting.
- Add `obsidian_writer.py` with proper frontmatter and tagging.

**Phase 3: Memory Injection & Quality Controls**
- Implement `memory_injector.py` using Hermes `memory` tool.
- Add sandbox validation step before committing insights.
- Introduce confidence scoring and human review gate.

**Phase 4: Feedback & Iteration**
- Add rating capture from Obsidian notes.
- Implement basic bi-temporal filtering.
- Package as reusable skill or standalone cron script.

---

## Risks and Mitigations

| Risk                              | Likelihood | Impact | Mitigation                                              |
|-----------------------------------|------------|--------|---------------------------------------------------------|
| False positive contradictions     | High       | Medium | Conservative similarity threshold + human review gate   |
| Negative or lazy synthesis        | Medium     | High   | Hard-coded three-rule constraints in every prompt       |
| Over-injection into Hermes memory | Medium     | Medium | Only inject when confidence ≥ 0.75 and predictive value passes |
| Performance on Android/Termux     | Low        | Low    | Lightweight heuristics first; run during charging       |
| Privacy of synthesized insights   | Low        | High   | All processing and storage remains fully local          |

---

## Success Metrics

- Functional Python package that completes a full REM cycle without errors.
- At least 5 high-quality synthesized insights generated in first 14 days of operation.
- Measurable reduction in repeated user corrections on the same topic after 30 days.
- Agent begins demonstrating context-sensitive behavior changes derived from REM output.

---

**Prepared by:**  
**Agent Claire**  
Research Agent, BlackRainLabs  
May 23, 2026

---

*BlackRainLabs — Where contradictions become breakthroughs.*