# RPS-1: Robo Parametric Standard

**Version:** 0.1 (Working Draft)
**Status:** Draft for public comment. Not a normative standard.
**Date of first publication:** 2026-10-19
**Authors:** Dimitar Parov, Illia Yaremchuk
**Publisher:** RoboIns Association (in formation), Zug, Switzerland
**Website:** https://roboins.org
**Contact:** info@roboins.org
**License:** Text CC BY 4.0; JSON schema Apache License 2.0

---

## Status of this document

This is the first public working draft of RPS-1. It was prepared by the founders of the RoboIns Association (in formation) to open a public discussion. It has **not** been peer reviewed, tested in pilots or approved by any technical council, and it does not yet reflect input from insurers, operators, researchers or regulators.

All thresholds, scales, trigger definitions and certification criteria in this draft are **provisional**. Values marked `TBD` or `[to be calibrated]` are intentionally left open and will be set using pilot data and public consultation. Nothing in this document is based on proprietary loss statistics, because no such statistics yet exist for the systems it covers; producing them is one of the purposes of the standard.

RoboIns is not an insurer. This document is not an offer of insurance or financial services. Policies based on RPS may only be issued by licensed insurers under the law that applies to them.

Comments are welcome as GitHub issues in this repository or by email to info@roboins.org.

---

## 1. Purpose

Autonomous AI agents and robots increasingly act without a human approving each decision. When such a system causes harm, responsibility is spread across the model developer, the integrator, the operator and the data provider, and there is little comparable data on what went wrong.

RPS-1 proposes an open, shared vocabulary so that insurers, operators, regulators and courts can describe the same system and the same incident in the same way. It defines:

1. **System classes** (Section 4);
2. a **Core Parameter Matrix** of eight parameters that describes a system's risk profile (Section 5);
3. a **trigger library** of observable incident events (Section 6);
4. **requirements for oracles and evidence** (Section 7);
5. the **Agent Incident Record** (Section 8), with a machine-readable schema in `schema/agent-incident-record.schema.json`;
6. three **certification levels** (Section 9).

The underlying principle is that every autonomous action is attributable to a responsible person or organisation, and that financial cover for defined events can be agreed before an incident occurs.

## 2. Scope

RPS-1 applies to software agents and physical robotic or automated systems that take actions with real-world effect with limited or no human approval of individual actions.

RPS-1 does not certify that a system is safe, does not replace product safety standards (for example ISO 10218 or ISO 13482) or regulatory conformity assessment, and does not define insurance contract terms, premiums or capital requirements.

## 3. Terms and conventions

The key words MUST, MUST NOT, SHOULD, SHOULD NOT and MAY are to be interpreted as described in RFC 2119 and RFC 8174. Because this is a draft, they indicate intended requirements for future versions, not obligations.

| Term | Meaning in this document |
|---|---|
| System | An AI agent, robot or automated system described by an RPS profile. |
| Operator | The person or organisation that deploys the system and sets its mandate. |
| Responsible party | The person or organisation named as accountable for the system's actions; by default the operator. |
| Mandate | The limits within which the system is authorised to act. |
| Profile | The set of Core Parameter Matrix values for one system and configuration. |
| Trigger | A defined, observable event that may form the basis of a parametric payout. |
| Oracle | An independent data source used to confirm that a trigger occurred. |
| Calculation agent | The party that determines, under agreed rules, whether a trigger has occurred. |
| Basis risk | The difference between a parametric payout and the actual loss suffered. |
| Fleet | A set of systems sharing the same base model, software build or vendor. |

## 4. System classes

| Class | Systems | Example triggers |
|---|---|---|
| **1. AI & Software Agents** | LLM and AI agents for customers and operations; financial and trading agents; multi-agent and Web3 agents | Action outside mandate; confirmed data leak; breach of risk limits; base model or provider outage longer than N hours |
| **2. Industrial & Logistics Robotics** | Industrial manipulators; collaborative robots; mobile AGV/AMR | Human contact above the ISO/TS 15066 threshold with emergency stop; collision; unplanned downtime longer than N hours |
| **3. Next-Gen Automated Systems** | Humanoid robots; unmanned aerial, ground and marine systems | Loss of link or navigation; fall; geofence breach; kinetic damage to third parties |

A system that falls into more than one class SHOULD be profiled under each applicable class.

**Fleet risk** is treated separately from individual systems: the simultaneous failure of more than X% of a fleet after a software or model update (trigger T-F1). X is `TBD`.

## 5. Core Parameter Matrix

Every system receives a profile of eight parameters. Until the incident registry accumulates enough data, the profile is intended to stand in for the missing loss history.

| Code | Parameter | What it describes |
|---|---|---|
| AUT | Autonomy | Share of decisions taken without a human, on a scale L0–L5 |
| MAN | Mandate | What the system may do: payment limits, access rights, force, speed |
| KIN | Kinetic energy | The physical harm the system is able to cause |
| ENV | Environment | Closed cell, shared zone or public space |
| OVS | Oversight | Human in the loop, on the loop or after the fact; time to stop |
| PRV | Provenance | Base model, version and vendor, to account for correlation |
| EVL | Evaluation | Testing and conformity evidence (for example ISO 10218, ISO 13482, ISO/IEC 42001, red-teaming) |
| LOG | Logging | Ability to prove what happened, from local logs to signed, attested records |

### 5.1 Provisional scales

The scales below are a starting proposal for discussion. They are not calibrated and MAY change substantially.

**AUT – Autonomy**

| Level | Description |
|---|---|
| L0 | No autonomy: every action is executed by a human. |
| L1 | Assistance: the system proposes, a human approves each action. |
| L2 | Partial: the system acts alone on routine actions; a human approves exceptions. |
| L3 | Conditional: the system acts alone within its mandate; a human is available to intervene. |
| L4 | High: the system acts alone within its mandate; humans review after the fact. |
| L5 | Full: the system may change its own goals or mandate within broad limits. |

**ENV – Environment**

| Level | Description |
|---|---|
| E1 | Closed or virtual environment, no people or third-party assets exposed. |
| E2 | Controlled shared zone with trained personnel. |
| E3 | Public or uncontrolled environment, including third parties. |

**OVS – Oversight**

| Value | Description |
|---|---|
| in_the_loop | A human approves actions before they take effect. |
| on_the_loop | A human monitors in real time and can stop the system. |
| after_the_fact | Actions are reviewed only after they take effect. |

The profile SHOULD also state the measured or specified time to stop the system, in seconds.

**LOG – Logging**

| Level | Description |
|---|---|
| G0 | No reliable logs. |
| G1 | Local logs controlled by the operator. |
| G2 | Logs exported to an independent store with trusted timestamps. |
| G3 | Logs signed in hardware (TPM or TEE) with trusted timestamps and independent storage. |

Scales for MAN, KIN, PRV and EVL are recorded as structured values (limits, energy, model identifiers, test references) rather than levels in this draft. Whether they should be reduced to levels is an open question (Section 11).

## 6. Trigger library

### 6.1 Admission criteria

A trigger MAY be admitted to the standard only if the event is:

1. **Observable:** it leaves measurable traces;
2. **Independently confirmable:** at least one source outside the operator can verify it;
3. **Linked to real loss:** it correlates with actual damage, and the remaining basis risk can be described and disclosed to the buyer before purchase.

### 6.2 Draft triggers

| ID | Class | Trigger | Open parameters |
|---|---|---|---|
| T-A1 | 1 | Action outside mandate (for example a payment above the authorised limit) | — |
| T-A2 | 1 | Confirmed data leak caused by the agent | Confirmation source TBD |
| T-A3 | 1 | Breach of defined risk limits (for example position or exposure limits) | — |
| T-A4 | 1 | Outage of the base model or provider longer than N hours | N TBD |
| T-R1 | 2 | Human contact above the applicable ISO/TS 15066 threshold, with emergency stop | — |
| T-R2 | 2 | Collision with a person, vehicle or asset | Severity threshold TBD |
| T-R3 | 2 | Unplanned downtime longer than N hours | N TBD |
| T-X1 | 3 | Loss of link or navigation | Duration threshold TBD |
| T-X2 | 3 | Fall of a humanoid or legged robot, or uncontrolled descent of an aerial system | — |
| T-X3 | 3 | Breach of a defined geofence or restricted zone | — |
| T-X4 | 3 | Kinetic damage to third parties | Severity threshold TBD |
| T-F1 | All | Simultaneous failure of more than X% of a fleet after a software or model update | X and time window TBD |

Each trigger in a future version will need a precise definition, the evidence it requires, a confirmation method and a documented description of its basis risk. This draft lists the triggers only.

## 7. Oracles and evidence

Evidence used to confirm a trigger SHOULD meet the following requirements:

1. **Attested telemetry:** telemetry and logs signed in hardware (TPM or TEE) where technically possible.
2. **Trusted time:** clocks synchronised via PTP (IEEE 1588) or GNSS, with RFC 3161 timestamps on records.
3. **Independent confirmation:** a quorum of M of N independent oracles. M and N are `TBD` and may depend on the trigger.
4. **Calculation agent:** an independent calculation agent determines whether the trigger occurred.
5. **Dispute window:** the determination becomes final after a dispute window of 7 to 30 days, set per product.
6. **Ledger as journal:** a distributed ledger or smart contract MAY be used as a tamper-evident journal or for automation. It MUST NOT be treated as the source of truth about the physical or digital event.

The basis risk of any product built on RPS triggers MUST be disclosed to the buyer before purchase.

## 8. Agent Incident Record

The Agent Incident Record (AIR) is a single record format for incidents involving systems covered by RPS. It is designed to serve insurers, regulators, researchers and courts at the same time.

Every record answers six questions:

1. **What did the system do?** The action, its time and its context.
2. **Under whose mandate?** The responsible party and the limits it set.
3. **Which model and version?** Base model, vendor and software build (PRV).
4. **What failed?** The RPS trigger that fired and the observed values.
5. **Who confirmed it?** The independent sources and the calculation agent.
6. **What happened next?** Containment, payout and corrective action.

The machine-readable format is defined in [`schema/agent-incident-record.schema.json`](schema/agent-incident-record.schema.json) (JSON Schema 2020-12). An illustrative example is in [`examples/air-example.json`](examples/air-example.json); it is fictional and does not describe a real incident.

### 8.1 Registry principles

The RoboIns incident registry is intended to follow these principles:

- **Pseudonymised by default:** operators and individuals are not named in public data; published statistics are aggregated and anonymised.
- **Independent:** the registry is run by the association, not by an insurer or vendor, under rules approved by its general assembly.
- **Useful to all:** periodic incident reports for regulators and researchers; aggregated access for members.

Personal data in records MUST be minimised and processed in line with applicable data protection law (including the Swiss FADP and, where applicable, the EU GDPR).

## 9. Certification levels

| Level | Name | Meaning |
|---|---|---|
| 1 | RPS Ready | The RPS profile of the system is complete and shared with the insurer or partner. |
| 2 | RPS Verified | An accredited auditor has checked the profile, the logging and the oracle set-up. |
| 3 | RPS Insured | A licensed insurer has issued a policy based on RPS triggers for this system. |

Accreditation rules for auditors and the certification procedure are not yet defined. No certificates are issued under this draft.

## 10. Governance and versioning

- RPS is intended to be maintained by a technical council with quotas for insurance, operators and manufacturers, science and civil society, so that no group holds a majority. Until the council is formed, the founders maintain this draft.
- Versions follow the pattern RPS-1.0, RPS-1.1 and so on. Drafts before 1.0 are numbered 0.x.
- Each version from 1.0 onwards is intended to go through a public consultation of at least 60 days.
- RPS is designed to be compatible with, and not to replace, the EU AI Act (logging, human oversight), the revised Product Liability Directive (EU) 2024/2853, the NIST AI RMF, ISO/IEC 42001, ISO 10218, ISO/TS 15066 and AIUC-1. Mapping to these frameworks is future work.

## 11. Open questions

The following questions are explicitly open for comment:

1. Should MAN, KIN, PRV and EVL be reduced to ordinal levels, and on what basis?
2. What values of N (outage, downtime) and X (fleet failure) are meaningful for pricing?
3. How should T-A2 (data leak) be confirmed independently of the operator?
4. What minimum M of N oracles is realistic for each class?
5. How should correlation through shared base models (PRV) be represented in the profile and in fleet triggers?
6. What is the minimum evidence for a record to be accepted into the registry?
7. How should pseudonymisation be balanced with the needs of courts and regulators?

## 12. Roadmap

| Stage | Content |
|---|---|
| 0. Foundation | Adoption of the statute, publication of the manifesto and RPS-1 v0.1 (this document) |
| 1. Team | President and board members; technical council |
| 2. Pilots | Two or three operators of robots or AI agents; first incident records; calibration of N and X |
| 3. First product | A policy by a licensed insurer on RPS triggers; RPS Insured level |
| 4. Scale | RPS-1.0, periodic incident reports, transition to a foundation |

## How to cite

> Parov, D., & Yaremchuk, I. (2026). *RPS-1: Robo Parametric Standard, Version 0.1 (Working Draft).* RoboIns Association (in formation), Zug, Switzerland. https://roboins.org

---

© 2026 Dimitar Parov, Illia Yaremchuk. Published by RoboIns Association (in formation). Text licensed under CC BY 4.0.
